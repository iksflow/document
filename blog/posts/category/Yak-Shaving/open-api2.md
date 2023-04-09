# Endpoints
앞서 알아본 내용에서 아래의 항목들이 필수 구성요소라고 배웠다. 
이번에는 paths에 엔드포인트를 정의하는 법을 정리해본다.
* openapi: OAS의 버전을 작성합니다.
* info: API의 설명, 작성자, 연락처 등의 정보를 작성합니다.
  * title: API의 이름을 작성합니다.
  * version: API의 버전을 작성합니다. OAS의 버전과 혼동하면 안됩니다.
* paths: API의 엔드포인트들을 작성합니다


```yaml
openapi: 3.1.0
info:
  title: Tic Tac Toe
  version: 1.0.0
paths: 
  /board:
    get:
      summary
    put:
```

/board : Paths Object
get, put, post ..: Path Item Object
summary, desription, responses, etc.. : Operation Object
"HTTP Code1": Responses Object
description, content, .. : Response Object

Paths Object는 직접적으로 서버 URL에 붙게되므로 반드시 슬래쉬로 시작해야한다.

Operation Object
오퍼레이션 객체는 summary, description 을 제공하고 기본적으로 작업의 매개변수, 페이로드, 가능한 서버 응답등을 서술한다.

Responses Object
Responses Object는 요청에대한 서버의 응답으로 기대되는 답변 모음이다.  
각 필드이름은 HTTP 응답 코드이고 ""로 감싸져있다. 
그리고 이것의 값은 Response Object라고 한다.

Response Object
Response Object는 응답이 어떤 의미인지 알리기 위해 반드시 description을 포함하고 있고 HTTP응답 코드에 대한 의미를 보충한다.
하지만 가장 중요한 필드는 content 입니다.
content는 응답 가능한 페이로드에 대해 설명합니다. 

```yaml
openapi: 3.1.0
info:
  title: Tic Tac Toe
  description: |
    This API allows writing down marks on a Tic Tac Toe board
    and requesting the state of the board or of individual squares.
  version: 1.0.0
paths:
  # Whole board operations
  /board:
    get:
      summary: Get the whole board
      description: Retrieves the current state of the board and the winner.
      responses:
        "200":
          description: "OK"
          content:
```