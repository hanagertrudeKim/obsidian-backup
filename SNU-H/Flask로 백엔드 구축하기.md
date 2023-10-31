


##  setting Flask
- Flask 설치
```python
pip3 install Flask
```

- `api.py` 작성
```python
from __future__ import print_function
from flask import Flask, request, jsonify
from dicom_deidentifier import run_batch_or_not, run_deidentifier_batch, run_deidentifier

app = Flask(__name__)

@app.route('/', methods=["GET"])
def hello():
return "Server active!  


@app.route('/deidentify', methods=['GET'])
def deidentify_dcm_folder():
# Get the folder path from the query parameters
	src_path = request.args.get('path')
	result = run_deidentifier(src_path)
	return jsonify(result)  


@app.route('/deidentify/batch', methods=['GET'])
def deidentify_dcm_folders():
# Get the folder path from the query parameters
	src_path = request.args.get('path')
	result = run_deidentifier_batch(sr_path)
	return jsonify(result)

  
# :5000 is the flask default port.
# You can change it to something else if you would like.
if __name__ == '__main__':
app.run(host='127.0.0.1', port=5000, debug=True)
```

- 테스트 서버 돌려보기
	- flask 서버 실행 
	- 서버가 정상 작동하는것을 확인했다.
```python
python3 src/server/api.py src  
```

## electron 호출
처음에는 axios로 호출하려했으나, electron에서 `net` 이라는 메서드를 이용하여 http 요청을 한다는것을 발견하였다.
이를 이용하여서 api 를 호출해보자.

- 우선, 호출하려는 정보가 들어있는 request 를 정의해준다
```js
const request = net.request({
	method: 'GET',
	protocol: 'http:',
	hostname: '127.0.0.1',
	port: 5000,
	path: `/deidentify?path=${dicomPath}`,
});
```

#### node 의 request 로 호출
- 이제 request 를 통해 api 를 호출해준다.
	- `request.on` 을 통해 요청을 해준다
	- `response.on` 을 통해 응답을 받아온다
	- `resquest.end` 로 요청이 끝났음을 알려준다 (이게 없으면 요청이 가지않는 현상이 일어남) 
```js
request.on('response', (response) => {
	console.log(`STATUS: ${response.statusCode}`);
	
	response.on('data', (chunk) => {
		console.log(`BODY: ${chunk}`);
		resolve();
	});
		
	response.on('error', (error: any) => {
		console.log(`ERROR: ${JSON.stringify(error)}`);
		reject(error);
	});
});
  
request.end();
```

이렇게 코드를 작성해준뒤, 호출을 해보니 500에러가 뜨는것을 발견했다.
원인을 찾기 위해, 일단 api 요청은 잘 가는지 확인해보기로 했다. 
`/deidentify` 코드를 수정해서, return 값을 확인해보자.
```python
@app.route('/deidentify', methods=['GET'])
def deidentify_dcm_folder():
	return "yes!!!!!"
```
- 놀랍게도 요청이 잘간다.

그렇다면, 문제는 deid script 와의 연동에 있을것같다.
기존의 문제를 나열해보자면,
- 명령행 옵션으로 스크립트가 실행됨
- `run_deidentifier()` 에 path 인자를 넘겨주어 실행시키는 함수로 수정해야한다.

일단은 스크립트를 커맨드로 정상 작동할수있도록 되돌린 상태에서 필요없는 인자값의 default 를 None 으로 바꿔준다.
그리고 나서, src_path 를 외부에서 인자로 넘겨줄수있도록 코드를 수정해본다.

수정하고 api를 호출해보니 일단 deid 된 파일은 생성이 되었다!!!
근데 문제는 node.js 관련 [[ERR_EMPTY_RESPONSE]] 에러가 난다.

GET 메서드의 return 값이 잘못되어서 그런가 찾아봤는데, REST Api 원칙에 어긋나는 정의를 하고있었다.
내 목적에 맞는 메서드인 POST 로 다시 바꿔주었다.
```info
#### GET, POST 방식

- GET → 통상적으로! 데이터 조회(Read)를 요청할 때  
    예) 영화 목록 조회  
    → 데이터 전달 : URL 뒤에 물음표를 붙여 key=value로 전달  
    → 예: google.com?q=북극곰
- POST → 통상적으로! 데이터 생성(Create), 변경(Update), 삭제(Delete) 요청 할 때  
    예) 회원가입, 회원탈퇴, 비밀번호 수정  
    → 데이터 전달 : 바로 보이지 않는 HTML body에 key:value 형태로 전달
```

#### axios 로 호출 

그리고 쓰기 불편한 electron 의 net 모듈보다는 간편한 axios 를 통해 코드를 리팩토링 해주었다.
```js
async function runPython(dicomPath: string) {
	try {
		const response = await axios.post('http://127.0.0.1:5000/deidentify', {
		path: dicomPath,
	});
	
	console.log(`STATUS: ${response.status}`);
	console.log(`HEADERS: ${JSON.stringify(response.headers)}`);
	console.log(`BODY: ${response.data}`);
	
	} catch (error: any) {
		console.error(`ERROR: ${error.message}`);
		throw error;
		// 에러 처리 또는 예외 조치를 취합니다.
	}
}
```

`socket hang out` 이라는 난생 처음보는 에러가 나왔다.
=> 찾았다. main 의 return 을 지정해주지않고, 기존의 `sys.exit('success')` 통해 코드 강제 종료시키면서 success 를 반환하니 거기서부터 꼬인것같다
아래와 같이 성공하면 원래 의도한대로 'success'를 반환할수있게 고쳐주었다.

```python
def main(src_path):
	if run_batch_or_not(src_path):
		run_deidentifier_batch(src_path)
		return "success"
	else:
		run_deidentifier(src_path)
		return "success"
  
	# 작업이 실패하면 종료 코드 'error'을 반환
	return "error"
```

와.... 성공했다...ㅠㅠ

