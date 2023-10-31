

이제 [CLOUDTYPE](https://app.cloudtype.io) 을 이용하여 배포를 해준다.

우선 flask 의존성을 추가해주기 위해 requirement.txt가 필요하다.

 - requirements.txt 생성 명령어
```python
pip3 freeze > requirements.txt
```

cludtype을 살펴보면, start command 가 `gunicorn -b 0.0.0.0:5000 app:app` 로 gunicorn 이라는 패키지로 실행되는것을 알수있다. 이를 위해 gunicorn을 requirements 에 추가해주자

- `gunicorn` 설치
```python
pip3 install gunicorn
```
- requirements.txt 종속성 업데이트를 해준다.
```python
pip3 freeze > requirements.txt
```

이제 CLOUDTYPE 설정 명령어를 다듬(?)어준다.
```python
gunicorn -b 0.0.0.0:5000 [Flask를 import 하여 어플리케이션을 실행하는 파일명]:app 
# [예시]cloudtype.py 에서 Flask를 import 하여 어플리케이션을 실행 
gunicorn -b 0.0.0.0:5000 cloudtype:app
```

그리고 빌드해주면 끝난다.
