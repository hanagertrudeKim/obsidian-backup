---
tags:
  - electron
  - build
  - nodeJS
  - dicom
  - Backend
date: 2023.09.21
---

## pipeline~
- [ERB](https://electron-react-boilerplate.js.org/docs/packaging)의 환경에서 앱 packaging 

<예상 파이프라인>
- exe 파일을 dist 파이썬 실행폴더에 빌드
	- python 스크립트를 실행 가능한 .exe 파일로 빌드
	- pyinstall 이용
- api.exe 파일을 node의 `spawn` 을 통해 백그라운드 서버 실행 
	- `child_process` 모듈의 `spawn` 또는 `execFile` 메서드를 사용하여 Python .exe 파일을 실행하고 백그라운드 서버를 구동
	  (ex. `pyProc = require('child_process').execFile(script, [port])`)
- axios를 통한 http 호출
	- 요청 및 응답 처리가 정확한지 확인
- 앱이 종료될때, exe 파일 서버 종료
	- Electron 애플리케이션이 종료될 때 백그라운드 서버도 종료
	- Electron 애플리케이션의 이벤트를 사용하여 서버를 정상적으로 종료하는지 확인

### 1. exe 실행파일 빌드
우선 **pyinstall** 이라는 라이브러리로 파이썬 배포를 위한 파일과 파이썬 빌드를 진행한다.
#### pyinstall
- 파이썬은 실행 파일이 아닌 소스코드로 제공됨 
	- => 클라이언트 환경에서 파이썬 인터프리터를 설치해야만 사용할수있음
- pyinstall을 사용하면 파이썬 파일을 실행파일(.exe) 형태로 패키징
	- 사용자가 별도의 인터프리터 없이도 해당 프로그램 사용가능 (내가 찾던거..)
	- 파이썬 프로그램의 배포와 실행이 쉬워짐
	- 다양한 운영체제에서 동작하는 실행파일 지원

- 실행파일 빌드하기
	- `-F` : 한 실행파일로 빌드함
```python
sudo pyinstaller -F api.py
```

### exe 파일로 flask 임시 서버 실행
electron 윈도우가 시작되는 시점에서, flask 서버가 실행하도록 해준다.
```js
function createWindow() {
	...
	// Flask 서버 시작
	const SERVER_PATH = app.isPackaged
	? path.join(process.resourcesPath, 'dist/api')
	: path.join(__dirname, '../../dist/api');
	  
	const { execFile } = require('child_process');
	const flaskProcess = execFile(SERVER_PATH, ['src']);
	
	// Flask 서버 출력을 로깅
	flaskProcess.stdout.on('data', (data: any) => {
	console.log(`flask stdout: ${data}`)
	})
	  
	flaskProcess.stderr.on('data', (data: any) => {
	console.error(`flask stderr: ${data}`);
	});
}
```
- path 관련 모듈을 찾을수없다는 오류가 나서, 다시 pyinstall 을 실행해보았다.
	- 원인은, `pydicom` 의 submodule을 pyinstall 이 읽지못해, 직접 추가해줘야한다고한다.
	  api.spec 의 `hiddenModules` 에 해당 모듈을 추가해주었다.
```ruby
from PyInstaller.utils.hooks import collect_submodules


hiddenimports = collect_submodules('pydicom'),
```
- 해당 package 를 잘 읽어오는것을 확인했다.

**주의사항
- spec 내용을 적용해 다시 EXE 파일을 만들 때, 반드시 .spec 파일을 pyinstall로 불러와줘야한다.
```python 
pyinstaller --onefile api.spec
```


### axios 를 통한 http method 호출
이전에 미리 짜놓은 [[Flask로 백엔드 구축하기#axios 로 호출| axios 호출]] 을 통해 method 를 사용하여 deid 를 진행하도록한다.

### flask 서버 종료 
이제 window 가 종료될때 서버도 같이 종료될수있게 서버 종료 코드를 추가해준다.
```js
mainWindow.on('closed', () => {
mainWindow = null;

// Electron 애플리케이션 종료 시 Flask 서버도 종료
if (flaskProcess) {
	flaskProcess.kill();
}
});
```



### 문제 해결
#### Flask 서버 실행 속도 지연
- 앱을 실행시키고나서 실행 버튼을 한번 누르고, 서버가 연결될 정도로 서버 실행 속도가 너무 느리다
- 아마 electron 안의 로직들이 동기적으로 처리되면서 생기는 문제들같다 
  => 비동기처리를 해주어서 서버 실행 속도를 높여보는 방식으로 진행해보았다

우선,  flask 서버를 실행하는 함수를 분리해준다.
`pythonProcess` 변수를 지정해, 프로세스가 한번 진행하여 서버가 켜진다면 다시 서버를 실행하지않도록 flag 역할을 실행할수있도록 하였다. (앱이 다시 재실행되어도, 서버는 재실행되지않는다.)
또한 `path.resolve()` 를 사용해 절대 경로를 통해 좀 더 serverPath를 빠르게 읽어올수있도록 설정하였다.
```js
// Flask 서버 path
const SERVER_PATH = app.isPackaged
	? path.join(process.resourcesPath, 'dist/api')
	: path.join(__dirname, '../../dist/api');
  
let pythonProcess: any; // Python 프로세스를 저장하는 변수

//flask 서버 실행 함수 정의
function startFlaskServer() {
if (!pythonProcess) {
	pythonProcess = execFile(path.resolve(SERVER_PATH), ['src']);
	// Flask 서버 출력을 로깅
	pythonProcess.stdout.on('data', (data: any) => {
		console.log(`flask stdout: ${data}`);
	});
  
	pythonProcess.stderr.on('data', (data: any) => {
		console.error(`flask stderr: ${data}`);
	});
}
```

그리고나서 electron 프로그램을 실행하는 함수(`createWindow()`) 로 넘어가서 함수를 비동기적으로 호출해준다. 
또한 서버를 종료하는 함수도 함께 수정해준다.
```js
const createWindow = async () => {
		mainWindow = new BrowserWindow({	
			show: false,	
			width: 1024,
			height: 728,		
			icon: getAssetPath('icon.png'),
			webPreferences: {
			preload: app.isPackaged
			? path.join(__dirname, 'preload.js')
			: path.join(__dirname, '../../.erb/dll/preload.js'),
		},
	});
	...
	
	startFlaskServer();
	  
	mainWindow?.on('closed', () => {
		if (pythonProcess) {
		pythonProcess.kill();	
		pythonProcess = null;
	}	
	mainWindow = null;
	});
});
```
 - 확연히 이전보다 속도가 높아진것을 확인했다...
 - 이제 진짜 진짜 빌드를 해보자
#### backend 서버 빌드 오류
백엔드 폴더가 같이 빌드되지않아, package 된 앱에서 flask 서버가 실행되지 않는다. 
문제를 해결하기 위해 빌드에서 패키징되는 과정을 정리해보자. 크게 두가지로 나뉘어본다면,

1. webpack 으로 빌드됨
2. `electron-builder` 를 통해 패키징을 시작
현재는 builder 가 특정 파일들을 패키징하도록 설정되어있는데, 주요한 dist 파일만 추가되어있다.
	이곳에 backend 폴더를 추가해주어 추가적으로 빌드되게 만들어준다!
```json
"files": [
	"dist",
	"node_modules",
	"package.json",
	"backend" //요로콤
],
```

또한 빌드될때마다, dist 폴더를 포함해서 다 초기화되어서 무슨 현상인가 봤는데, package script 설정이 그렇게 되어있었다. (ERB 기본 설정인것같다)

기존의 명렁어 설정을 살펴보면,
dist 랑 script 캐시 지우기 -> 다시 빌드 -> 빌드된 파일로 패키징 이런 순서로 이루어진다.
```json
"package": "ts-node ./.erb/scripts/clean.js dist && npm run build && electron-builder build --publish never",
```

이를 바꿔주어 패키징만 따로 할수있도록 해주었다.
```json
"scripts": {
"package": "electron-builder build --publish never",
```


여기까지 수정하고나니, 패키징 파일에 잘 들어가고 빌드가 잘 되었다. 
이제 서버 접속만 다시 해서 서버배포까지 하면 진짜 끝~! 이 되었으면 좋겠다 제발...