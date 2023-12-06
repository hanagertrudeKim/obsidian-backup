
## Electorn의 dialog module을 사용하여 사용자 로컬 디렉토리 path 가져오기
### dialog

> Display native system dialogs for opening and saving files, alerting, etc.

- **파일을 열거나 저장할수있는 모듈**
- 알림을 표시하기 위한 네이티브 시스템 대화 상자를 표시
- 파일이나 디렉터리를 다중으로 선택하는 예시 코드
	- 렌더러 프로세스에서 열고싶다면, `remote` 를 통해 접근
```js 
const { dialog } = require('electron')
console.log(dialog.showOpenDialog(
	{ properties: ['openFile', 'multiSelections'] }))
```
#### method
- dialog 모듈은 다음과 같은 메서드를 가지고있다.
```js
dialog.showOpenDialogSync([browserWindow, ]options)
```
- browerWindow(opt)
	- allows making it modal in parent window
- options : object
	- buttonLabel : custom label for the confirm btn
	- filters : filefilter[]
	- properites : features the dialog should use
		- openFile : allow file select
		- openDirectory : allow directories select
			- window 와 linux 에서는 don't allow both a file, directory selector 
		- "build": "concurrently \"npm run build:main\" \"npm run build:renderer\"",

### dialog module 을 사용하여 디렉토리 path 가져오기

html 의 input 태그를 사용한다면 디렉토리의 path가 아닌 파일 각각의 path를 가져오게 된다.
그렇지만, 내가 필요한것은 디렉토리의 path 이기 때문에, electron 내장 모듈인 dialog module을 사용하였다. 

아래와 같이 dialog의 `showOpenDialog`의 메서드로 `openDirectory` 를 이용하여 사용자의 디렉토리 path를 받아올수있게 해주었다.
```js
const result = await dialog.showOpenDialog(mainWindow, {
	properties: ['openDirectory'],
	});
```

- `result` 를 출력해보니, 다음과 같이 filePaths로 dicom folder가 받아와진다.
```js
{
  canceled: false,
  filePaths: [ '/Users/hana/Desktop/KU39009_20220921' ]
}
```

