# SCSS

- 문법
    - 중첩의상황
        
        ```scss
        .box {
          font: {
            weight: bold;
            size: 10px;
            family: sans-serif;
          };
          margin: {
            top: 10px;
            left: 20px;
          };
          padding: {
            bottom: 40px;
            right: 30px;
          };
        }
        ```
        

- 변수 설정
    - 변수의 유효 범위 → {  }
    - 변수 재할당 가능( 변수에 변수를 할당 )

```scss
$변수이름: 속성값;

$color-primary: #e96900;
$url-images: "/assets/images/";
$w: 200px;

.box {
  width: $w;
  margin-left: $w;
  background: $color-primary url($url-images + "bg.jpg");
}
```

- #{ }
    - 코드의 어디든지 변수값 사용 가능
    
    ```scss
    $family: unquote("Droid+Sans");
    @import url("http://fonts.googleapis.com/css?family=#{$family}");
    ```