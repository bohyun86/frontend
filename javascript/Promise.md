
### 상황 설명
친구가 그녀의 회사에서 Junior 프론트엔드 개발자 포지션이 열리면 여러분을 고용하겠다고 약속했다고 가정해 봅시다. 그래서 여러분은 모든 자바스크립트 주제를 학습하기 시작했습니다. 다음 두 달 동안 프론트엔드 관련 기능들을 기억하면서 친구의 응답을 기다리는 동안 공부할 예정입니다. 만약 결과가 긍정적이라면, 첫 번째 진짜 개발자 직업을 축하하기 위해 큰 파티를 열 계획이지만, 그렇지 않다면 자바스크립트를 계속 공부할 것입니다. 여기서 "약속"은 현재 단순히 합의만 존재하고, 실제 결과가 나왔을 때 친구가 연락해 준다는 의미입니다.

프로그래밍에서도 동일한 개념이 존재합니다. 어떤 함수가 결과를 기다리고 있다가 응답을 받으면 그 결과에 따라 특정 동작을 수행하는 것입니다. 그 동작은 결과가 긍정적인지 또는 부정적인지에 따라 다릅니다. 이제 이러한 개념이 어떻게 작동하는지 자세히 살펴보고 구체적인 예를 들어보겠습니다.

### Promise 문법
우선 Promise의 기본 구조를 살펴보겠습니다:

```javascript
let promise = new Promise(function(resolve, reject) {
  // 실행할 코드
});
```

위 코드에서 새로운 Promise를 만들면 그 내부에 'executor'라고 불리는 함수가 있습니다. 이 함수는 Promise가 만들어지자마자 바로 실행됩니다. 결과가 무엇인지 즉시 알 수 있는 것이 아니라, 그저 나중에 값을 받게 될 것이라는 것만 알 수 있습니다. Promise의 가장 큰 장점은, 값이 계산되는 동안에도 프로그램이 계속 실행된다는 것입니다. 실행된 함수가 완료되면 그제서야 결과를 볼 수 있습니다.

예를 들어, 사용자가 웹사이트에 접속했을 때 아바타 이미지가 로드되지 않더라도 페이지 전체를 볼 수 있습니다. 사용자는 30초 동안 빈 페이지를 보고 기다리지 않고 웹사이트와 상호작용을 할 수 있으며, 아바타가 로드되면 그 자리에 이미지가 표시됩니다.

executor 함수에는 두 개의 인자가 있습니다: `resolve`와 `reject`. 이 두 함수는 약속(Promise)이 성공적으로 이행되었는지 여부에 따라 호출됩니다.

- **`resolve(value)`**: 약속이 성공적으로 이행되었을 때, 결과 값을 전달합니다.
- **`reject(error)`**: 약속이 실패했을 때, 에러 객체를 전달합니다.

중요한 점은 executor가 **`resolve`** 또는 **`reject`** 중 하나만 호출한다는 것입니다. 두 함수 모두를 호출할 수는 없습니다.

### 예시
앞서 가정한 여러분의 프로그래밍 직업 상황으로 돌아가 봅시다. 아래는 그와 관련된 Promise 코드입니다:

```javascript
const myFriendHasApprovedMyPosition = true;

let promise = new Promise(function(resolve, reject) {
  if (myFriendHasApprovedMyPosition) {
    resolve("Hurrah! 이제 나는 진짜 개발자가 되었어!");
  } else {
    reject(new Error("어이쿠! 더 공부해야겠네 =("));
  }
});
```

이 예제에서는 `myFriendHasApprovedMyPosition`이라는 상수에 따라 Promise가 결정됩니다. 만약 값이 `true`라면, `"Hurrah! 이제 나는 진짜 개발자가 되었어!"`라는 메시지와 함께 `resolve` 함수가 호출되고, 만약 `false`라면 `Error` 객체와 `"어이쿠! 더 공부해야겠네 =(")`라는 메시지를 함께 `reject` 함수가 호출됩니다. 이 예제에서는 값이 `true`이기 때문에 `resolve` 함수가 실행됩니다.

실제 상황에서는, 개발자들이 주로 Promise를 시간이 걸리는 함수 실행에 사용합니다. `setTimeout`을 사용한 예시를 보겠습니다:

```javascript
let promise = new Promise(function(resolve, reject) {
  setTimeout(() => resolve("완료되었습니다!"), 5000);
});
```

이 상황에서 브라우저는 Promise 함수를 즉시 실행하지만, `resolve` 함수는 5초 후에 호출됩니다. 이러한 방식은 원격에서 데이터를 가져와야 할 때 특히 유용합니다.

### Promise의 상태
Promise는 **`state`**라는 상태 속성을 가진 객체입니다. Promise는 언제나 다음 세 가지 상태 중 하나일 수 있습니다:

1. **`pending`**: 초기 상태로, Promise가 실행되었지만 아직 결과가 나오지 않은 상태입니다.
2. **`fulfilled`**: Promise가 성공적으로 완료되어, `resolve` 함수가 호출된 상태입니다.
3. **`rejected`**: Promise가 실패하여, `reject` 함수가 호출된 상태입니다.

이 과정을 이해하기 위해 상태 전환 과정을 나타내는 다이어그램을 생각해 보세요:

처음에는 Promise가 **`pending`** 상태입니다. 성공하면 `resolve(value)`를 호출하여 상태를 **`fulfilled`**로 바꾸고, 실패하면 `reject(error)`를 호출하여 상태를 **`rejected`**로 변경합니다. 중요한 것은 Promise의 상태는 **한 번만 변경**될 수 있다는 점입니다.

### 결론
Promises는 시간이 걸리는 함수를 실행할 때 다른 프로세스를 멈추지 않고도 작업을 진행할 수 있게 해주는 편리한 기능입니다. Promises를 사용하면 함수 실행을 바로 시작하고 결과는 프로세스가 완료된 후에 설정할 수 있습니다. 특히 대용량 데이터를 로드할 때 유용합니다. 

---

물론이죠! 이제 Promise와 관련된 메서드들 `.then`, `.catch`, `.finally`에 대해 자세하게 설명드리겠습니다. 이번 주제에서는 Promise가 완료된 이후 결과를 어떻게 다루는지 배워보도록 하겠습니다.

### `.then`
`.then` 메서드는 약속(Promise)의 결과가 긍정적이든 부정적이든 이를 처리하는 데 사용됩니다.

예를 들어, 바쁜 학생들이 시험 날짜를 추적할 수 있도록 도와주는 프로그램을 만들고 있다고 해봅시다. 현재 날짜를 기준으로 사용자가 시험을 놓쳤는지 여부를 알려주는 Promise를 만듭니다. 만약 시험이 아직 치러지지 않았다면 "시험 준비를 해야 합니다"라는 메시지로 resolve하고, 그렇지 않다면 "이런! 시험을 놓쳤습니다!"라는 에러 메시지로 reject합니다.

```javascript
const examDate = new Date(2020, 7, 5);
const promise = new Promise(function(resolve, reject) {
  const currentDate = new Date();
  if (currentDate < examDate) {
    resolve("시험 준비를 해야 합니다");
  } else {
    reject("이런! 시험을 놓쳤습니다!");
  }
});
```

이제 결과에 따라 성공일 경우 메시지를 콘솔에 출력하고, 실패일 경우 알림(alert)을 띄우도록 해보겠습니다. 이를 위해 `.then` 함수를 사용하며, 두 개의 매개변수를 받습니다. 하나는 긍정적인 결과를 처리하는 함수이고, 다른 하나는 예외를 처리하는 함수입니다. 만약 Promise가 resolve되었다면, 성공 상태 함수를 호출하고 결과를 콘솔에 출력합니다. 반면에, Promise가 reject되었다면 실패 상태 함수를 호출해 에러 메시지를 처리합니다.

```javascript
promise.then(
  function successStatus(response) {
    console.log(response);
    return response;
  },
  function failStatus(error) {
    console.log(error);
    return error;
  }
);
```

이와 같이 `.then` 메서드는 Promise의 결과를 받아 적절한 동작을 실행하는 데 사용됩니다. `.then`의 두 인자는 선택적이기 때문에, 필요에 따라 생략할 수 있습니다.

### `.catch`
에러만 처리하고 싶은 경우가 있습니다. 이때에는 `.then`의 첫 번째 인자를 null로 전달하는 방법도 있지만, 보다 간편하게 `.catch`를 사용할 수 있습니다.

```javascript
promise.catch(function failStatus(error) {
  console.log(error);
  return error;
});
```

`.catch` 메서드는 Promise가 실패했을 때 수행하고자 하는 동작을 지정하는 데 사용됩니다. 이 경우, 우리는 에러 메시지를 알림으로 표시합니다.

한 Promise에 `.then`과 `.catch`를 함께 사용하는 것도 가능합니다.

```javascript
promise
  .then(function successStatus(response) {
    console.log(response);
    return response;
  })
  .catch(function failStatus(error) {
    console.log(error);
    return error;
  });
```

이렇게 하면 `.then`으로 성공 상태를 처리하고, `.catch`로 에러 상태를 처리할 수 있습니다.

### `.finally`
`.finally` 메서드는 Promise가 완료되었을 때, 즉 성공 또는 실패 여부에 관계없이 어떤 동작을 수행하고 싶을 때 사용됩니다. 아래는 사용 예시입니다:

```javascript
promise
  .then(function successStatus(response) {
    console.log(response);
    return response;
  })
  .catch(function failStatus(error) {
    console.log(error);
    return error;
  })
  .finally(function stopLoader() {
    console.log("로더가 멈췄습니다");
  });
```

위 예시에서 `"로더가 멈췄습니다"`라는 메시지는 Promise가 완료된 후에 출력됩니다. Promise가 resolve되든 reject되든 상관없이 사용자가 이를 보게 됩니다. 이는 로더를 멈추거나 기본적인 인사말을 표시하는 등의 반드시 필요한 동작을 처리할 때 유용합니다.

### Promise 체이닝 (Promise Chaining)
여러 개의 스크립트가 순서대로 실행되어야 할 때 Promise 체이닝을 사용할 수 있습니다. 예를 들어, 먼저 사용자의 역할을 불러온 후, 그다음 사용자 정보를 로드하고, 마지막으로 개인 설정에 따른 배너를 로드해야 하는 경우가 있다고 해봅시다. 이런 상황에서 Promise 체이닝을 사용하면 앞 단계가 완료된 후 다음 요청을 호출할 수 있습니다.

```javascript
loadData("https://mywebsite.com/loadRoles")
  .then(function() {
    return loadData("https://mywebsite.com/loadUserInfo");
  })
  .then(function(user) {
    return loadData(`https://mywebsite.com/loadBanner_${user.id}`);
  })
  .catch(function(error) {
    console.log("이런! 에러가 발생했습니다!");
  });
```

위의 코드에서 `.catch` 메서드는 체인 전체의 모든 단계에서 발생하는 에러를 처리합니다. 중요한 것은 각 단계에서 Promise를 반환해야 코드가 비동기적으로 실행된다는 점입니다.

### 결론
이번 주제에서는 Promise의 결과를 다루기 위한 세 가지 주요 메서드에 대해 배웠습니다. 요약해보자면:

- **`.then`**: 결과에 따라 특정 동작을 수행하는 데 사용됩니다.
- **`.catch`**: 에러를 처리하기 위해 사용됩니다.
- **`.finally`**: Promise가 완료되었을 때(성공 여부와 관계없이) 특정 동작을 실행하는 데 사용됩니다.

