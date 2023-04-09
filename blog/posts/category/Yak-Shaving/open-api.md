API 명세서를 작성하면 좋은점

문서가 없을 경우의 문제
기존에 겪어왔던 문제점
* 문서의 내용이 명확하지 않을 경우 해석의 차이로 인한 오류가 발생한다.
* 문서가 없다.
* 갱신하지 않아 오래된 정보.
* 이해할 수 없는 언어로 작성된 정보

최소한의 문서 구조
필수항목 2개

* openapi: OAS의 버전을 작성합니다.
* info: API의 설명, 작성자, 연락처 등의 정보를 작성합니다.
  * title: API의 이름을 작성합니다.
  * version: API의 버전을 작성합니다. OAS의 버전과 혼동하면 안됩니다.
* paths: API의 엔드포인트들을 작성합니다.

아래는 예시입니다.
```yaml
openapi: 3.1.0
info:
  title: A minimal OpenAPI document
  version: 0.0.1
paths: {} # No endpoints defined
```
