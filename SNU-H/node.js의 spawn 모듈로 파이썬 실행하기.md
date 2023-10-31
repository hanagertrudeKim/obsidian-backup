

node.js 에서 제공하는 spawn 모듈을 이용하여 파이썬을 실행해보자.

path module을 사용하여 script의 상대 경로를 정의해준다.
```js
const pythonScript = path.join(__dirname, './dicom_deidentifier.py');
```

spawn으로 python 스크립트로 python3 을 실행한다음, 해당 인자값을 넣어준다.
```
const pythonProcess = spawn('python3', [pythonScript, folderPath]);
```




```ad-note
참고 : [[node.js의 path 모듈 다루기| node.js 의 path 모듈에 대한 정리 ]]
```

