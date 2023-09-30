---
tags:
  - electron
  - build
  - nodeJS
  - dicom
date: 2023.09.21
---

## pipeline~
- [ERB](https://electron-react-boilerplate.js.org/docs/packaging)의 환경에서 앱 packaging 

<예상 파이프라인>
- exe 파일을 dist 파이썬 실행폴더에 빌드
	- Python 스크립트를 실행 가능한 .exe 파일로 빌드
	- pyinstall 이용
- api.exe 파일을 node의 `spawn` 을 통해 백그라운드 서버 실행 
	- `child_process` 모듈의 `spawn` 또는 `execFile` 메서드를 사용하여 Python .exe 파일을 실행하고 백그라운드 서버를 구동
	  (ex. `pyProc = require('child_process').execFile(script, [port])`)
- axios 를 통한 http 호출
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

- 