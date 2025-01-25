웹 성능 최적화를 수행하는 이유

- 서비스 관점에서도 웹 성능 최적화는 중요하다
- 로딩 속도가 사용자의 경험에 큰 영향을 미친다.
- 0.1초의 개선이 conversion rate를 증가시킨다.

웹 성능을 어떻게 측정해야하는지

- 속도 정의 (Google measure web performance)
    - 세가지 지표가 존재 (Core Web Vitals)
        - LCP - Largest Contentful paint
            - 화면이 얼마나 빨리 뜨느냐 (loading speed)
        - FID - First Input Delay
            - 얼마나 빨리 반응하느냐 (scring interacivity)
        - CLS - Cumulative Layout Shift
            - 얼마나 비주얼이 안정적이냐
- ⇒ 이 지표들이 첫 반응 기준으로 측정이 되기 때문에 지표 개선이 논의됨
- ⇒ 따라서 INP로 개선 (올해 초에 Web Vitals가 대체될 예정이다)
    - INP
        - 모든 input들을 다 체크하고, 모든 지연시간의 평균을 측정

Core Web Vitals Tools

![](https://i.imgur.com/bhPel2Y.png)

- lighthouse
- web vitals JS
    - frontend extention
    - 브라우저에 상관없이 정보 수집 가능 (크롬에 종속 x)

## LCP Optimization

1. LCP 자원이 HTML 내에서 빨리 찾아져야 한다.
2. LCP 자원이 우선시되서 다운로드 될 수 있도록 해야한다.
3. 돈을 투자해서 CDN을 쓴다.
    1. 사용자가 가장 가까운 서버에서 데이터를 전달해주는 것
    2. 물리적으로 최대한 가까운..

실제 사례)

- 이미지는 `<img>` 엘리먼트에 넣고 src 나 srcset 속성을 이용
    - 첫번째 렌더링할때 필요하구나 알려줄수있음
- SSR 사용
- 내부에서 호스팅하는 이미지가 아닐 경우, `<link rel="preload">` 태그 이용
    - 브라우저에게 빨리 로드해야하는 소스라는것을 알려줌
- `fetchpriority="high"` 속성을 `<img>` 태그에 넣어줌
    - LCP 이미지를 우선적으로 브라우저가 다운로드하게됨
- `loading="lazy"` 속성을 `<img>` 엘리먼트에 넣어줌
    - LCP가 아닌 이미지를 나중에 로딩될 수 있도록

## CLS Optimization

- 컨텐츠의 명확한 사이즈를 설정함
- animation/transitions 같은 흔들리는 css들은 지양
- bfcache 사용
    - before after caching ⇒ 모든 지표를 향상시킬수있는 도구
    - 캐싱을 막아놓지는 않았는지 확인

실제 사례)

- width, height 지정
- aspect-ratio 사용
    - width 만 지정해서 height 유동적으로 지정되게
- min-height 지정
    - 요소가 덜밀리게 방지 가능

## FID Optimization

- 긴 작업을 잘게 쪼갬
- 불필요한 자바스크립트 피하기
    - 어려운 부분, 웹을 빠삭하게 아는 사람이 개선 가능
- 큰 렌더링 업데이트 피하기
    - 부분부분만 업데이트 해라

실제 사례)

- 긴 task 를 작게 쪼개기
    - webpack 과 같은 도구를 이용하여 chunk 가 가능하게
- 메인 쓰레드에 잠깐의 자원을 양보
- coverage tool 이용
- code splitting
- 트랙킹 tag를 덜어내라 (불필요한것들)
- requestAnimationFrame()은 지양
- DOM 사이즈를 작게 유지

reference
[23년 6월 Tech 세미나 - 웹 프론트엔드 성능 최적화 방법 및 적용 사례](https://www.youtube.com/watch?v=BEwv4to9OWY)