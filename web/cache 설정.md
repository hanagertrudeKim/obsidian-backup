# Cache-Control, Expires를 빠뜨리면 브라우저에서 무슨 일이 일어날까?

http 캐시를 적극적으로 설정하고 제어함으로써 웹 성능을 높일 수 있다는 사실이 있습니다.

현재 우리 프론트엔드 트랙에서 캐시 설정을 직접 다루지는 않습니다. 그렇지만 앞으로의 웹 성능과 이해를 위해 다양한 생명 주기를 가지는 캐시를 다루는 방법에 대해 아는것은 꼭 필요합니다.

그런데 브라우저 캐시 관련 헤더인 `Cache-Control` , `Exprires` 를 빠뜨리면 무슨 일이 일어나는지, 또한 브라우저 캐시 설정이 왜 중요한지 살펴보겠습니다.

## 웹 캐시

- 실제 작업을 하며 빌드된 사항이 적용이 안될때 `캐시 삭제해라` `강한 새로고침 해라` 등등의 말을 들은 적이 있을 겁니다.(cc. 최원빈님) 이것들도 브라우저 캐시와 관련된 사항이라고 볼 수 있죠.
- 클라이언트는 서버로부터 HTTP 요청을 통해 필요한 리소스 (HTML, CSS, JS, 이미지 등)를 불러옵니다. 네트워크를 통해 이 과정을 수행하는건 느리고 비용이 많이 듭니다.
- 여기서 발생할 수 있는 불필요한 네트워크 재요청을 방지하기 위해 웹 캐싱을 사용합니다.
- `Cache-Control` `Expries` 등과 같은 우리가 자주 봐온 헤더는 캐시 동작과 관련된 헤더로, 서버의 비용을 줄이고 클라이언트의 성능을 향상시키는 데 유용하게 사용됩니다.

### Cache-Control, Expires 헤더를 기입하지 않은 경우

- 누군가 의도했든 의도하지 않았든 `Cache-Control` `Expires` 헤더 모두 기입하지 않은 경우, 브라우저는 어떻게 동작할까요?
- 대표적인 브라우저인 크롬의 캐시 동작을 살펴보기 위해 Chromium의 코드를 확인해보았습니다.

```jsx
HttpResponseHeaders::GetFreshnessLifetimes(const Time& response_time) const {
  FreshnessLifetimes lifetimes;
  if (HasHeaderValue("cache-control", "no-cache") ||
      HasHeaderValue("cache-control", "no-store") ||
      HasHeaderValue("pragma", "no-cache")) {
    return lifetimes;
  }
...
if ((response_code_ == net::HTTP_OK ||
       response_code_ == net::HTTP_NON_AUTHORITATIVE_INFORMATION ||
       response_code_ == net::HTTP_PARTIAL_CONTENT) &&
      !must_revalidate) {
    // TODO(darin): Implement a smarter heuristic.
    Time last_modified_value;
    if (GetLastModifiedValue(&last_modified_value)) {
      // The last-modified value can be a date in the future!
      if (last_modified_value <= date_value) {
        lifetimes.freshness = (date_value - last_modified_value) / 10; //여기!
        return lifetimes;
      }
    }
  }
```

- Chromium의 `http_response_headers.cc` 를 살펴보니 freshness(유효성)이 `(date_value - last_modfied_value) / 10` 로 할당되는 것 같습니다.
- 이는 Date와 LastModified의 차이값의 10분의 1로 추정하여 유효성(freshness) 계산을 한다는 것입니다. [MDN 문서](https://developer.mozilla.org/ko/docs/Web/HTTP/Caching#%EC%9C%A0%ED%9A%A8%EC%84%B1_%EA%B2%80%EC%82%AC_%ED%9C%B4%EB%A6%AC%EC%8A%A4%ED%8B%B1)를 보니 `heuristic` 이라는 동작으로 캐시 헤더를 지정하지 않으면 **웹 스팩을 기반으로 대략적으로 캐시 작동을 합니다.**

### 그럼 캐시도 알아서 동작하니 문제가 없는걸까?

휴리스틱 캐시는 문제가 많이 생겨날 수 있습니다. 컨텐츠 배포를 했더라도 웹 배포후 특정 기기에서 이전 캐시 된 콘텐츠가 노출될 수 있고, 변경사항이 실제로 적용되는데 더 많은 시간이 걸릴 수 있습니다.

간단한 해결책으로는 사용자가 캐시를 비워 리소스를 새로 가져오도록 하는 것 입니다.

실제 개발자 창을 열고 새로고침 버튼을 우클릭하면 ‘캐시 비우기 및 강력 새로고침’ 을 선택할 수 있습니다.

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/8723af7a-c25d-4fde-b295-d69f32afbbd8/d36871bd-3eab-4348-b14f-45c973d2c5d4/Untitled.png)

그러나 근본적으로 캐싱에 관한 문제가 발생하면 명시적으로 헤더를 설정하여 캐시 문제를 해결해주는게 좋다고 생각합니다. 캐시 유효시간을 사용하여 정확한 시간에 업데이트를 보장하는 방법을 써보면 좋을것같습니다.

실제 토스에서는 html은 배포 주기에 체크되어야 하는 리소스이기 때문에 `Cache-Control` 값으로 `max-age=0, s-maxage=31536000` 을 설정한다고 합니다. 그러나 JavaScript 나 CSS 파일은 프론트엔드 웹 서비스를 빌드할 때마다 새로 변경사항이 적용될 수 있도록 임의 버전 번호를 부여해 설정합니다.

⇒ 만약 토스의 캐시 설정에 대해 더 자세히 알고싶다면 [여기를 클릭](https://toss.tech/article/smart-web-service-cache)

## 요약

1. 브라우저 캐시 설정을 따로 하지 않을 경우 크롬, 사파리 등의 브라우저는 알아서 캐시 동작을 해줍니다.
2. 자동으로 캐시 설정이 된다는건 문제가 발생할 수 있는 지점이기 때문에 이를 확인해주는것이 중요합니다.
3. 각각의 특성에 따라 캐시를 섬세히 제어함으로 안전하고 빠르게 HTTP 리소스를 로드할 수 있으며, 트래픽 비용을 절감할 수 있습니다.

참고자료

- [https://web.dev/articles/http-cache](https://web.dev/articles/http-cache)
- [https://developer.mozilla.org/ko/docs/Web/HTTP/Caching](https://developer.mozilla.org/ko/docs/Web/HTTP/Caching)