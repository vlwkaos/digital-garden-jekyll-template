---
title: '관리하기 쉬운 코드'
author: 'vlwkaos'
published: true
---

컴퓨터 과학에서 문제 해결 능력을 중요시 여기는 이유는 아마 컴퓨터를 통해 원하는 결과를 결과를 항상 얻을 순 없기 때문이다. 비슷한 이유로 개발자에게도 문제 해결 능력이 요구된다. 하지만 개발자는 컴퓨터 앞에 앉아 키보드를 두드리기 전에 한가지 문제를 더 해결해야한다(개발자에게 국한된 얘기는 아니다). 어떻게 하면 **[[때로는 중복코드가 더 낫다|빠른 시간 내]]**에 **오류 없는 정확한 구현**을 할 수 있을까? 두 마리 토끼를 모두 잡을 수 있다면 좋겠지만 그럴 수 없는 경우가 많다. 특히 프로젝트가 커질 수록 그렇다. 혼자 개발하는 경우 규모가 작기 때문에 구조를 크게 신경 쓰지 않아도 빠르게 원하는 결과를 얻을 수 있지만 여러 사람이 작업하는 코드는 일종의 규칙(코딩 컨벤션) 없이는 금방 스파게티 코드가 되어버려 관리가 힘들어진다. 어느정도 시간이 지난 후에도 개발을 빠르고 정확하게 하기 위해서는 초기에 [[기술 부채|프로젝트의 이해]]와 관리가 용이하도록 코드를 짜야한다. 다행히 규모에 상관없이(Scalable) 유용한 클린 코드를 작성하기는 어렵지 않다. 사실 모든 코드 아키텍쳐는 근본적으로 지금 설명하려는 코드 형태를 취하고 있기 때문에 이것만 알아도 클린 코드를 작성할 수 있을 것이다.

### 1. 복잡한 코드
아래 코드는 url를 조립한뒤 데이터를 요청하여 받아오고 원하는 데이터를 반환하는 코드이다. 

```javascript
// * 정확한 javascript가 아닐 수 있음

async function getUserName(user_id) {
    const baseUrl = 'https://some.domain.com/api/user';
    const url = `${baseUrl}/${id}`
    const response = await axios({ // 데이터 입출력
      method: 'get',
      url,
      responseType: 'json'
    });
    const userData = response.data; // 데이터 입출력
    if (!userData.name) throw new Error('no name')
    return userData;
}
```

한 함수 내에서 여러 동작을 하고 있어 보기 좋지 않다. 비교적 짧아서 이해하기 어려운 코드는 아니지만 함수가 더 복잡하고 길어질 수 있는 큰 프로젝트에 대한 비유라 생각하자.

가장 먼저 코드 이해에 방해되는 부분은 데이터를 요청하고 받는 부분이다. 이 부분을 다른 곳으로 이관해보자.

### 2. 데이터 요청과 받는 부분을 추상화 시키자

```javascript
async function getUserName(id) {
    const baseUrl = 'https://some.domain.com/api/user';
    const url = `${baseUrl}/${id}`
    const userData = await apiGetRequest(url); // 데이터 입출력
    if (!userData.name) throw new Error('No user name')
    return userData;
}

async function apiGetRequest(url) {
    const response = await axios({
      method: 'get',
      url,
      responseType: 'json'
    });
    return response.data;
}
```

원래 함수를 이해하기 한결 쉬워졌다. 다만 아직 문제가 남아있다. 원래 함수를 테스트하고 싶을 때 `apiGetRequest` 함수도 매번 같이 호출해야 한다. 그리고 요청할 url을 만드는 동작도 매번 실행된다. 이런 것을 [커플링(Coupling)](https://ui.toast.com/weekly-pick/ko_20150522)이라고 하는데 코드 의존성을 키우고 테스트를 어렵게 한다. 이는 소프트웨어가 망하게 되는 지름길이다.

커플링을 알아차릴 수 있는 좋은 방법은 해당 함수를 요상한 방법을 동원하여 모킹(mocking)하지 않고 테스트 할 수 있는가이다. 예를 들어 위의 예제에서 `getUserName`을 테스트하기 위해서 `apiGetRequest`함수를 HTTP요청을 보내지 않는 가짜 함수로 대체하지 않으면 안된다.

더 나은 방법은 없을까?

### 3. 데이터 요청과 받는 부분을 제외하고 모두 추상화시키자

```javascript
async function getUserName(id) {
    const url = makeUrl(id);
    const data = apiGetRequest(url); // 데이터 입출력
    return getUserNameFromData(data);
}

function makeUrl(id) {
    const baseUrl = 'https://some.domain.com/api/user';
    const url = `${baseUrl}/${id}`
    return url;
}

function getUserNameFromData(data) {
    if (!userData.name) throw new Error('no name');
    return userData;
}

// 데이터 입출력
async function apiGetRequest(url) {
    const response = await axios({
      method: 'get',
      url,
      responseType: 'json'
    });
    return response.data;
}
```

이렇게 하면 맨 위의 함수만이 데이터 입출력을 담당하게 되고 나머지(`getUserNameFromData`, `makeUrl`)는 결과 값이 입력 값에만 좌지우지 되는 순수함수(pure function)형태가 되어 테스트가 쉬워진다. 

결론적으로 외부와 내부를 연결해주는 **껍데기**(**imperative** **shell**)와 데이터 조작만을 다루는 **기능부**(**functional** **core**) 둘로 나뉜 구조가 되었다. 이런 구조는 함수형 프로그래밍이 지향하는 바이기도 하다.

좀 더 크게 놓고 봤을 때 이런 과정을 거치도록 소프트웨어를 만들면 된다.

1. 껍데기로 데이터를 받아온다.
2. 받은 데이터를 시스템에서 받아들일 수 있는 형태로 가공한다.
3. 기능부로 데이터를 입력한다.
4. 기능부에서 데이터를 조작한 결과값을 받는다.
5. 결과 데이터를 밖으로 보낼 수 있는 형태로 가공한다.
6. 껍데기를 통해 데이터를 내보낸다.

이렇게 만들면 만드는 프로그램의 기능을 테스트하기 훨씬 수월해진다.

---

> 이 글은 `danuker`의 [블로그 글](https://danuker.go.ro/the-grand-unified-theory-of-software-architecture.html
)을 참조하여 만들었습니다. 원문에서 사용된 코드와 이미지는 다른 라이센스를 가지고 있어 사용하지 않았습니다. 클린 코드 작성을 설명하는 부분과 코드 아키텍쳐에 대한 설명은 대강 번역하여 간접 인용하였습니다. 원문의 라이센스는 [Creative Commons Attribution 4.0 license](https://creativecommons.org/licenses/by/4.0/)를 따릅니다.
