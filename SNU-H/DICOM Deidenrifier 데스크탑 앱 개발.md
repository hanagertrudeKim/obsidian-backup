---
date: 2023.09.01
title: electron을 이용한 데스크탑 앱 개발
tags:
  - electron
  - typeScript
  - python
  - dicom
---

### 파이프 라인

1. 파이썬 or 파이썬 스크립트 돌릴 수 있는 아무 언어로 desktop application 만드는 방법 찾아보기
2. Form 형식의 GUI 만들어보기
3. Form submit 했을때 간단한 파이썬 스크립트가 돌아가고 csv 파일로 데이터 세이브되는 파이프라인 만들어보기

- 대략적인 흐름
	- 의료 이미지 데이터 업로드 ->
	- form 채우기 -> form 제출 -> 
	- 앱에서 form 정보 바탕으로 의료 이미지 데이터 deidentification하는 스크립트 돌려서 데이터 준비 ->
	- deidentification 된 이미지 & 간단한 정보 적힌 csv 파일 다운로드

- form 
	- "subj",
            "patient_id",
            "patient_name",
            "ct_date",
            "slice_thickness",
            "number_of_slices",
            "study_description",
            "series_description",

### 초기 세팅

1. 개발 tool 정하기
	- [[Electron]]
		- 개인적 선택 이유
			- linux서버 기반으로 돌아가는 데스크탑 앱 필요
			- 파이썬 스크립트를 돌릴수있는 언어 선택
			- 웹스택만으로 빠르게 개발할수있음

	- 이외의 프레임워크
		- react : 바닐라 js 보다는 익숙한 개발을 위해 선택
		- typescript : 코드 퀄리티, 타입 안정성을 위해 사용
		- react router : 라우팅 라이브러리로 사용. but, 기존에 사용하던 `BrowserRouter`가 아닌 파일 시스템의 라우팅임으로 `HashRouter` 사용할 예정
		- Material UI : GUI의 빠른 개발과 퀄리티를 위해 ui 라이브러리 사용 + ts 호환 최고

### 과정

-  [[DICOM]] 정리
- [[DICOM Deidentifier]] 정리
#### electron + python
- electron으로 받은 폴더를 deid 스크립트로 돌려야하는데, 폴더를 전달할 방법과 서버가 필요한것인지 등등의 문제가 발생

	1. electron의 ipc 모듈을 사용하여, 백엔드 구축 ([[Electron]])
		1. ipc 모듈로 충당 가능할까 생각했지만, 다른 backend 프레임워크인 Express등을 보았을때 ipc 모듈과 비슷한 형태
		   (구체적인 데이터베이스가 필요한지는 추후 문제일듯) 
		2. 결국엔 ipc main 에서 python 으로 연동하는 코드가 핵심 문제일듯싶다 
	2. ipc-main 이랑 파이썬이랑 연동할때 전달할 정보
		1. folder path
			- but, 로컬에서 얻어온 path 를 통해서 접근이 가능할까?
			- => 실제 스크립트를 돌려보니, 같은 파일 디렉토리가 아니더라도, 접근이 가능하다..
			  => so, 파일 path 를 넘기는것이 핵심인것같음
		2. 어떻게 path를 얻어오고, 어떻게 넘길것인가?
			- folder를 input으로 받으니, 각각의 file의 path만 받아와지는것을 확인했다. 
			- folder path 를 받아오는 방법은 electron 의[[ Electron#dialog | dialog]] 모듈을 사용하여, 얻어올수있을것같다.
```js
const result = await dialog.showOpenDialog(mainWindow, {
	properties: ['openDirectory'],
	});
```

- dialog 의 메소드인 `openDirectory` 를 사용하여 파일 디렉토리를 받아온다.
- `result` 를 출력해보니, 다음과 같이 filePaths로 dicom folder가 받아와진다.
```js
{
  canceled: false,
  filePaths: [ '/Users/hana/Desktop/KU39009_20220921' ]
}
```

- 이 path를 이용해서 폴더에 접근할수있을것같다. 파이썬 스크립트에 인자로 넘겨주면,
```js
const folderPath = result.filePaths[0];

// path module을 사용하여 script 경로정의
const pythonScript = path.join(__dirname, './dicom_deidentifier.py');

// spawn으로 python 스크립트 돌리기
const pythonProcess = spawn('python3', [pythonScript, folderPath]);
```
- [[ node.js에서 path 모듈 다루기 | node.js 의 path 모듈에 대한 정리 ]]
- `spawn` 메서드를 사용해 node.js 환경에서 python 인터프리터를 실행시키면... 완성

- 이제 nodejs 에서의 모든 준비는 다 끝났으니, 파이썬 스크립트의 최종 수정을 거치면 된다
- => [[DICOM Deidentifier#Node 와 Python 사이에 Data 전달 | Node와 Python 사이에 Data 전달]]
  ✅ 이로써 data 돌리는 파이프라인은 완성되었다.

#### dicom deid 실행 시점 변경
- 원래는 ipc-dicom 채널이름의 ipcMain으로 보내지는 순간 (= select folder 버튼이 눌리는 순간) 에 deid 가 되도록 설정해놨다.
- 다른 form들을 받을 가능성이 있음으로 deid 시점을 form 입력을 완료하고 난 후, submit 시점으로 변경하고싶었다.
- 해결 방법
	- ipc 채널을 두개 만들고
	  1. dicom folder path 받는 ipc
	     - reply 로 path 를 다시 renderer 로 보내, 폴더가 업로드 되었다는걸 확인할수있음
	  2. dicom 포함 다른 form data 제출 ipc
	     - 이 시점에 form 데이터들 서버에 저장
	     - python died 스크립트 실행
- 두개의 ipc 정의
(추후의 리팩토링이 조금 필요할듯싶다)
```js
///dicom 파일만을 다루는 main-ipc
ipcMain.on('ipc-dicom', async (event) => {
console.log('응답');
const result = await selectFolder();

event.reply('ipc-dicom-reply', result);
});

/// form data를 다루는 main-ipc
ipcMain.on('ipc-form', async (event, arg) => {
console.log(arg);
runPython();

event.reply('ipc-form-reply', '제출을 완료했습니다');

});
```

- renderer-ipc 에도 reply 를 보내, 업로드 완료된 사실과, form 제출의 성공 여부를 알수있게 수정해주었다. 
```js
//upload function
function selectFolder() {
window.electron.ipcRenderer.sendMessage('ipc-dicom');

// main ipc에서 응답 받기
window.electron.ipcRenderer.on(
	'ipc-dicom-reply', (arg: any) => {
		console.log('ipc-dicom-reply:', arg);
		setFilePath(arg);
	});
}

//form submit function
const clickBtn = (e: any) => {
e.preventDefault();
console.log(formValues);

// main ipc로 form data 보내기
window.electron.ipcRenderer.sendMessage(
	'ipc-form',
	JSON.stringify(formValues)
);

// main ipc에서 응답 받기
window.electron.ipcRenderer.on(
	'ipc-form-reply', (arg: any) => {
		console.log('ipc-form-reply:', arg);
	});
};
```


### 에러 정리

1. 서버로 받은 데이터를 어떻게 파이썬 스크립트로 통계를 돌릴지의 문제가 발생
	- 어떻게 전달할것인가????
		- 일단 node js 에서 파이썬 돌리는 방법을 알아보니
			- child-process 모듈 spawn을 통해 파이썬 실행
			  stdout의 이벤트 리스너로 실행 결과 받음
			- 자바스크립트에서 파이썬 함수 호출
			- 파이썬 함수 호출하면서 매개변수 전달 ( 데이터 전달 )
			- 서버 사이드 관련 에러
				- spawn이라는 메서드는 nodejs 기반이라 react파일로 컴파일하면 충돌일어남
				- ==> js 파일로 바꿨더니 해결함. 백엔드쪽 메서드인줄 몰랐자나 ~
				- 그냥 마치 spring 으로 프엔 짜는 황당한 짓을 하고있었ㅇ..
			- => **BUT, python이 없는 환경에서는 실행 불가함** 
		- Flask 서버 구축
			- python이 없는 환경에서 돌리려면, 서버 필요함
			- => [[Python Flask 백엔드백엔드 서버 구축]] 
				- python 을 사용하여 머신러닝 작업을 할 경우에 flask를 사용해 서버를 많이 구축하는것같다
				- 문제는 서버를 생성하는데, aws 를 이용해 서버를 사야한다는거...?
				- 병원에서 사용하고있는 서버가 있는지 문의해야겠다
				- =>서버 확인하기
			- => AWS lambda 이용(일단 보류)
				- 병원에서 사용하고있는 서버가 없다면, 따로 flask 같은걸 이용해서 서버를 구축할 필요없이 aws 에서 제공해주는 lambda 를 이용하여서 서버리스 서비스를 이용할수있다.
				- 훨씬 구현이 간편해지고, 편리해질것같다 (연결만 하면 되는듯 )
				- 근데 건당 비용이 발생한다는 점에서...조금 걸린다.
		- 결론 => 모르겠다.. 머리 식히고 다시 해야지..
2. ipc 관련 에러로, 이에 관해서 다시 정리해봄 ==> [[Electron]]
3. 배포 관련 에러 => [[Electron 배포]]