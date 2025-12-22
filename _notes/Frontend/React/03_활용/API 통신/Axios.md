---
tags:
  - WIP
---

>[[HTTP ❌]] 요청을 수행하기 위한 [[PROMISE ❌]] 기반 HTTP 클라이언트를 제공하는 서드파티 라이브러리
# 왜 쓰는걸까?
당신이 만약 프론트엔드 역할로서 웹사이트에서 백엔드에서 서로 값을 주고 받거나 할려면 서버 요청용 [[API ❌]]는 필수적일 것이다.

HTTP 요청을 수행하는 방법 중에는 여러가지가 있겠지만, 가장 인기 있는 것은 ==Axios==와 ==Fetch==일 것이다. 그 중 Axios에 대해 알아보자.
# 왜 유명할까?
==Axios가 인기 있는 이유를 뽑자면 아래와 같은 특징을 가지고 있어서라고 생각한다.==

- **자동 JSON 데이터 변환:** 서버에서 받은 데이터를 자동으로 [[JSON ❌]] 형식으로 변환한다.

- **응답 시간 초과:** 서버와의 요청 시간 초과를 설정할 수 있어, 서버의 응답이 오래 걸리는 경우 오류로 처리해버릴 수 있다.

- **HTTP 인셉터:** 요청과 응답을 처리하기 전에 가로채어 수정하거나 추가 로직을 추가할 수 있다.

- **다운로드 진행률:** 다운로드 및 업로드 진행도를 추적이 가능하다. 사용자에게 피드백을 제공하거나 필요에 따라 요청 취소가 가능하다.

- **동시 요청:** 여러 데이터 요청을 동시에 수행하고, axios.all 및 axios.spread를 사용하여 하나의 응답으로 결합할 수 있다.
---
## **자동 JSON 데이터 변환**
만약 ==fetch==와 같은 데이터를 자동으로 json 형태로 변환해주는 기능이 없다고 가정하자.

그렇다면 우리는 매번 서버에서 받은 데이터를 웹사이트에서 json 형태로 바꾸기 위해 아래와 같은 코드를 작성해야한다.
```js
const response = await axios.get('/api/data);
const data = JSON.parse(response.data); // 직접 파싱
```
이는 매번 `JSON.parse()`를 호출해야 하므로 직접 타입을 체크해가며 로직을 구현해야한다. 결국 코드가 장황해지고 실수할 확률이 높아진다.

---
## Axios의 구성요소
### Request Method
axios를 통해 GET, POST, PUT, DELETE를 요청할 수 있다. axios의 ==Request Method==는 다음과 같이 구성된다.

- GET : `axios.get(url[, config])`
- POST : `axios.post(url, data[, config])`
- PUT : `axios.put(url, data[, config])`
- DELETE : `axios.delete(url[, config]`

Request Method를 사용하기 위해선 axios 뒤에 `.`을 붙여 소문자로 원하는 요청을 작성하면 된다.

### Params
==일반적인 axios의 4가지 기본 메서드를 사용하기 위해선 다음과 같은 파라미터를 작성해야한다.==

- Method
- Url
- Data (optional)
- Params (optional)

파라미터를 사용하여 다음과 같이 요청이 가능하다.
```jsx
axios({
    method: "get",
    url: "url",
    responseType: "type"
}).then(function (response) {
    // response Action
});
```

만약 POST 메서드에서 data를 전송하기 위해서는 url 밑에 data Object를 추가하면 된다.
```jsx
axios({
    method: "post",
    url: "url",
    data: {
		name: "홍길동",
		age: 25
	},
    responseType: "type" // 보통은 json
}).then(function (response) {
    // response Action
});
```

### 단축된 axios 메서드
==axios에선 일반적인 메서드 호출을 좀 더 간단하게 작성할 수 있다. 두 방식 모두 똑같은 메서드를 호출하지만, 코드 가독성, 유연성 등을 고려하면 단축된 방식이 더 선호된다.==

#### axios.get() 
