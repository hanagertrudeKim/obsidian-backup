---
tags:
  - nodeJS
---

## 운영체제
- unix 운영체제와 window 운영체제는 전혀 다른 문자로 directory 를 구성한다
	- unix : `/` 문자 사용 , PATH 환경변수 `:` 사용
	- window `\` 문자 사용 , PATH 환경변수 `;` 사용
```js
//unix
$ pwd
/Users/daleseo

//window
cd
C:\Users\daleseo
```
=> **따라서 directory 를 단순히 string으로 간주해 접근하면, 특정 운영체제에서만 돌아가는 문제가 발생한다.**
*특히나, [[Electron]] 같은 크로스 플랫폼을 지원하는 곳에서는 더 중요해진다.

## path module
- node js 에서는 path module 을 제공하여 개발자들이 이러한 위험없이 경로를 다룰수있게한다.
### usage
```js
//불러오기 (ES modules)
import path from 'path';

//path 조합
path.join(__dirname, './dicom_deidentifier.py')
```
- `join()` 함수
	- 파일 경로를 조작하고 생성해줌
		- 여러개의 문자열을 가변인자로 받아서 하나의 완전한 경로로 조합해줌
	- `__dirname`
		- 현재 스크립트 파일이 위치한 디렉터리 반환
		- `__dirname`와 `'./dicom_deidentifier.py'` 문자열을 합쳐서 Python 스크립트 파일의 전체 경로를 생성
	- 비슷한 함수로 `resolve()` 함수가 있음
	  
- `basename()` 함수
	- 주어진 경로에서 파일 이름을 얻음
```js
path.basename("/Users/daleseo/test.txt")
// 'test.txt'
```

- `extname()` 함수
	- 확장자 얻기
```js
path.extname("/Users/daleseo/test.txt")
// '.txt'
```

- `isAbsolute()` 함수
	- 절대 경로 확인
```js
path.isAbsolute("/Users/daleseo/test.txt")
//true
path.isAbsolute("./test.txt")
//false
```

- `relative()` 함수
	- 상대 경로 구하기
```js
path.relative("/Users", "/Users/daleseo/test.txt")
// 'daleseo/test.txt'
```
