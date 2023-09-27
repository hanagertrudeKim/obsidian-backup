---
tags:
  - electron
  - build
  - nodeJS
  - dicom
date: 2023.09.21
---

## 개요
- [ERB](https://electron-react-boilerplate.js.org/docs/packaging)의 환경에서 앱 packaging 

## command
- 앱 build 하기
```js
npm run build
//or
npm run rebuild
```

- 앱 package
```js 
npm run package
```

## error
- 앱을 빌드해 dicom 파일을 업로드하면, de-identifier 기능이 작동하지않음
- 예상되는 문제들은
  1. 파이썬 스크립트 인식 불가
  2. node-pty 모듈이 돌아가지 않는 문제 


1. **파이썬 스크립트 인식 불가**
- 우선 경로가 제대로 들어가는지부터 확인해보자.

1. 파이썬 경로 얻어오기
	- 파이썬 경로를 받아와 실행 명령어는 다음과 같음
	- `__dirname`을 통해 현재 디렉토리의 위치와 `dicom_deidenrifier.py` 을 합쳐서 디렉토리 위치를 구함
```js
const pythonScript = path.join(__dirname, 'dicom_deidentifier.py');
const command = `python3 ${pythonScript} ${folderPath}`;
```

2. 파이썬 경로가 존재하는지 확인하기
	- 실제 build된 파일을 보면, 해당 디렉토리에서 파이썬 파일을 찾을수없었다.
	- 우선 python script 가 어느 경로로 들어가야하는지 확인해보자
```js
const pythonScript = path.join(__dirname, 'dicom_deidentifier.py');
```
출력된 경로 : `/Users/hana/Desktop/dicom-electron-app/release/build/mac-arm64/DicomElectron.app/Contents/Resources/app/dist/main/dicom_deidentifier.py` 


3. 해당 경로로 파일 디렉토리 빌드 설정 수정하기
	- package.json파일에서 `extraResource` 를 편집해 died 파일이 빌드시 해당 디렉토리 안으로 들어갈수있도록 수정해주었다.
```js
"extraResources": [
	"./assets/**",
	{
		//died 파일을 to 디렉토리에 추가하는 설정
		"from": "src/main/dicom_deidentifier.py",
		"to": "Contents/Resources/app/dist/main"
	}
],
```

- 다시 fs 모듈을 사용하여 해당 파일이 디렉토리에 있는지 확인해보았다.
	- 결과값 => 파일 없음 (???)
```js
ipcMain.on('ipc-dicom', async (event) => {
const result = await selectFolder();

fs.access(
	path.join(__dirname, 'dicom_deidentifier.py'), //python상대경로 구하기
	fs.constants.F_OK,
		(err: any) => {
			if (err) {
			event.reply('ipc-form-reply', '파일 없음'); //해당 경로에 파일 없음
			} else {
			event.reply('ipc-form-reply', '파일 있음'); //해당 경로에 파일 있음
		}
	}
);
  
event.reply('ipc-dicom-reply', result);
});
```

- 왜... 들어가지않을까.. 무엇을 더 만져야할까.. 방법 더 찾아보기ㅜ
- `extraResource` 의 설정된 경로가 잘못된건가싶어 다시 경로를 설정하였다
```js
"extraResources": [
	{
		"from": "./assets",
		"to": "Contents/Resources/app/assets"
	},
	{
		"from": "src/main",
		"to": "Contents/Resources/app/dist/main",
		"filter": ["**/*.py"]
	}
],
```



=> 근데 진짜 갑자기 생각해보니까, 파이썬이 설치되지 않는 환경에서 사용이 불가능하다. 왜냐면 명령어 자체가 python3... 으로 시작하기때문에...  (나 뭐한거냐)
=> **모두 기각!!**
