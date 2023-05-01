# Lint

#exlint #stylelint


1.  **eslint**
    
    1.  eslint 란?
        
        1.  코드 엄격성을 높여줌 —> 특히 자바스크립트 문법 검사할때 유용
        2.  코드를 깔끔하게 짜기 위한 툴
        3.  npm or yarn 을 통해 설치
        4.  eslint prettier 도 있음
        5.  **.eslintrc.json 파일**을 통해 수정하고 실행함
    2.  eslint-config-prettier
        
        1.  eslint 와 prettier을 같이 사용하는 경우에 충돌이 일어날수있다.
        
        —> eslint-config-prettier을 설치하면 중복관리되는 스타일을 eslint에서 피할수있다.
        
2.  **stylelint**
    
    1.  esint와 기능 똑같지만 **scss에 적용**됨.
    2.  stylelint는 node 개발 환경에서 동작한다. 따라서 노드 패키지 관리를 위해 package.json파일이 필요하다. (명령어 : npm init)
    3.  **stylelintrc.json 파일** 을 통해 수정하고 실행함.