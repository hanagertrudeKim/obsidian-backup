
- **pyqt 기본 세팅**
- **qt deisgner앱으로 ui 구축**
- **ui 에 기능 연결하기**
- **deid 스크립트 연결하기**
- **배포** **완료하기**


ui 업데이트
```
pyuic6 mainwindow.ui -o MainWindow.py
```

앱 구축
```python
python3 app.py
```

- pyinstaller 로 앱 빌드
```python
pyinstaller --onefile --icon=cloud.icns app.py
```

- pyinstaller app.spec 업데이트
```python
pyinstaller app.spec
```

