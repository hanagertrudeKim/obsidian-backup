
참고 문서 : https://datatracker.ietf.org/doc/html/rfc6749

## 토큰 인증
- 토큰은 서버 인증과 다르게 **stateless** 함.
	- 서버가 상태를 보관하고있지않다. 
	- = 서버가 한번 발급한 토큰에 대해서 더이상 제어할 수 X
	- = 토큰이 탈취되면 서버에서 제지할 방법이 없음

## access token 
- 수명이 있다.


## refresh token
- access token을 새롭게 발급받을수있는 수단



## 로직

### access token이 만료
- access_token이 만료되면 서버에서 401 error 발생
- local storage에 저장되어있던 refresh_token을 통해 access_token을 재요청

### refresh token이 만료
- 둘다 만료되었을 경우 local or session storage에 저장되어있는 토큰을 remove
- 다시 로그인페이지로 redirect 해서 로그인 요청
- 새로 갱신된 access_token, refresh_token을 storage에 저장



- 최초 유저 갱신 필요함
	- 최상단? 에서 최초 1회만 하는게 중요함