---
sticker: emoji//2b50
tags:
---
[시각화 자료](https://claude.site/artifacts/13453064-4a8c-467c-8e87-5119c08f28e2?fullscreen=true)

개발을 하다 보면 '동기', '비동기', 'Blocking', 'Non-blocking'이라는 용어를 자주 마주하게 됩니다. 이 개념들은 단순히 기술 면접에서 물어보는 이론적인 질문거리가 아니라, 실제로 우리가 만드는 애플리케이션의 성능과 사용자 경험에 직접적인 영향을 미치는 핵심 요소입니다.

> 웹 브라우저에서 버튼을 클릭했을 때 페이지가 멈추는 현상
> 서버가 수천 명의 사용자를 동시에 처리하지 못하는 문제
> 모바일 앱에서 데이터를 불러오는 동안 화면이 정지되는 현상

이 모든 것들은 동기/비동기와 Blocking/Non-blocking의 개념을 제대로 이해하고 적용했는지에 따라 해결 여부가 결정됩니다.

특히 JavaScript와 Node.js의 인기와 함께 비동기 프로그래밍은 선택이 아닌 필수가 되었습니다. 프론트엔드에서는 사용자 인터페이스의 반응성을 높이기 위해, 백엔드에서는 더 많은 동시 접속자를 처리하기 위해 이 개념들을 활용하고 있습니다.

이 글에서는 동기/비동기와 Blocking/Non-blocking이 정확히 무엇을 의미하는지, 어떤 차이점이 있는지, 그리고 실제 개발에서 어떻게 적용되는지를 명확하게 알아보겠습니다. 이론적인 개념을 넘어 실제 코드 예제와 함께 살펴보면서, 이 개념들이 왜 현대 소프트웨어 개발에서 이토록 중요한지 이해할 수 있을 것입니다.

복잡하게 느껴질 수 있는 이 개념들을 제대로 이해한다면, 더 효율적인 코드를 작성하고 더 나은 사용자 경험을 제공하는 애플리케이션을 개발할 수 있을 것입니다.  저는 프론트엔드 개발자이기 때문에, JS를 주로 예시를 들며 작성했습니다.

# 동기/비동기 + Blocking/Non-blocking 개념 정리

먼저 간단히 표로 정리해봤습니다.

| 구분        | 동기/비동기                                                   | 블로킹/논블로킹                                             |
| --------- | -------------------------------------------------------- | ---------------------------------------------------- |
| **핵심 질문** | "작업 완료 여부를 누가 신경 쓰는가?"                                   | "제어권을 누가 가지고 있는가?"                                   |
| **동작 방식** | - 동기 : 요청자가 완료 여부를 직접 확인  <br>- 비동기 : 요청받은 쪽이 완료 후 알려줌   | - 블로킹 : 제어권을 넘겨주고 대기  <br>- 논블로킹 : 제어권 유지하며 다른 작업 수행 |
| **예시**    | - 동기 : 전화 통화 (즉시 응답 필요)  <br>- 비동기 : 문자 메시지 (응답 기다리지 않음) | - 블로킹 : 엘리베이터 앞에서 기다림  <br>- 논블로킹 : 계단으로 이동          |

즉, 작업/함수 등의 실행에 대해 **작업의 흐름**과 **제어권**이라는 서로 다른 측면을 설명합니다.

1. **동기/비동기는 "작업 완료 확인 방식"**에 관한 것
    - 동기는 "완료를 기다린다."
    - 비동기는 "완료를 기다리지 않는다."
2. **블로킹/논블로킹은 "제어권"**에 관한 것
    - 블로킹은 "제어권을 넘긴다."
    - 논블로킹은 "제어권을 유지한다."

## 동기(Synchronous)와 비동기(Asynchronous)

**작업 완료 여부를 확인하는 방식**에 대해서 정의한 패러다임입니다.
### 동기(Synchronous)

동기는 작업의 **실행 순서**와 **완료 시점**에 관한 개념입니다.

#### 정의
한 작업이 시작되면 그 작업이 완료될 때까지 다음 작업이 기다리는 방식입니다.

#### 특성

- 코드가 순차적으로 실행
- 현재 작업이 끝나야만 다음 작업으로 넘어감
- 작업 결과를 즉시 반환
- 흐름이 예측 가능하고 직관적

#### 예시

```javascript
console.log("첫 번째");
console.log("두 번째");
console.log("세 번째");
// 항상 "첫 번째", "두 번째", "세 번째" 순서대로 출력됩니다
```

### 비동기(Asynchronous)

비동기는 작업의 완료를 기다리지 않고 다른 작업을 진행하는 방식입니다.

#### 정의
작업을 시작한 후 완료 여부와 상관없이 다음 작업을 진행하는 방식입니다.

#### 특성

- 작업이 완료되지 않아도 다음 작업 수행
- 작업 결과는 나중에 콜백, 프로미스, async/await 등으로 동기와 다르게 받음
- 여러 작업이 동시에 진행 가능
- 실행 순서가 보장되지 않을 수 있음

#### 예시

```javascript
console.log("시작");
setTimeout(() => {
  console.log("비동기 작업 완료");
}, 1000);
console.log("다음 작업 진행 중");
// "시작", "다음 작업 진행 중", "비동기 작업 완료" 순서로 출력됩니다
```

## Blocking과 Non-blocking

**제어권의 흐름과 실행 중 대기의 유무**에 대해서 정의한 패러다임입니다.
### Blocking

Blocking은 제어권(control)에 관한 개념으로, 호출한 함수가 제어권을 돌려받지 못하는 상태를 말합니다.

#### 정의
특정 작업이 완료될 때까지 프로그램의 제어권을 붙잡고 있는 상태입니다.

#### 특성

- 호출한 함수는 호출된 함수가 작업을 완료할 때까지 대기
- 해당 스레드는 다른 작업을 수행 불가능
- 리소스를 효율적으로 사용하지 못할 수 있음
- 주로 동기 방식과 함께 사용 (항상 그런 것은 아님)

#### 예시

```javascript
// Blocking I/O 예시 (Node.js)
const fs = require('fs');
const data = fs.readFileSync('file.txt', 'utf8'); // 이 줄에서 블로킹됨
console.log(data);
console.log('다음 작업'); // 파일 읽기가 완료된 후에만 실행됨
```

### Non-blocking

Non-blocking은 호출한 함수가 즉시 제어권을 돌려받는 상태를 말합니다.

#### 정의
작업 완료 여부와 상관없이 프로그램의 제어권을 즉시 반환하는 상태입니다.

#### 특성

- 호출한 함수는 작업 시작 후 즉시 제어권 회수
- 다른 작업을 계속 수행 가능
- 리소스를 효율적으로 사용 가능
- 주로 비동기 방식과 함께 사용 (항상 그런 건 아님)

#### 예시

```javascript
// Non-blocking I/O 예시 (Node.js)
const fs = require('fs');
fs.readFile('file.txt', 'utf8', (err, data) => {
  if (err) throw err;
  console.log(data); // 파일 읽기가 완료된 후에 실행
});
console.log('다음 작업'); // 파일 읽기를 기다리지 않고 즉시 실행
```

# 동기/비동기 + Blocking/Non-blocking 조합
  
| 조합                       | 설명                                            | 예시                                     |
| ------------------------ | --------------------------------------------- | -------------------------------------- |
| **Sync + Blocking**      | 호출자가 피호출자의 완료를 기다리며 제어권도 넘김                   | 전통적인 파일 읽기/쓰기                          |
| **Async + Non-blocking** | 호출자가 완료를 기다리지 않고, 제어권도 유지하며 완료 시 콜백/이벤트로 통보받음 | Node.js의 이벤트 루프, JavaScript의 `Promise` |
| **Sync + Non-blocking**  | 호출자가 완료를 직접 확인하지만 제어권은 유지                     | 폴링(Polling) 방식                         |
| **Async + Blocking**     | 호출자가 완료를 확인하지 않고, 제어권을 넘김                     | 거의 사용되지 않음                             |

이 두 개념은 서로 다르지만 같이 얘기되는 경우가 많아 자주 혼란을 일으킵니다. 이 두 개념의 조합으로 만들 수 있는 네 가지 패턴을 살펴봅시다.

## 조합 개념
### 1. 동기 + Blocking: 일이 끝날 때까지 꼼짝 못함

가장 흔한 조합으로, 작업이 끝날 때까지 다음 코드로 넘어가지 않고 대기하는 방식입니다. 마치 은행에서 창구 직원이 내 업무를 처리할 때까지 줄에서 기다리는 것과 같습니다.

```javascript
// 파일을 동기적으로 읽는 예시
const fs = require('fs');
const data = fs.readFileSync('big-file.txt'); // 여기서 멈춤!
console.log('파일 읽기 완료'); // 파일 읽기가 끝나야만 실행됨
```

#### 특징

- 코드가 순차적으로 실행되어 예측하기 쉬움
- 시간이 오래 걸리는 작업이 있으면 프로그램 전체가 멈춤
- 단일 스레드 환경에서는 UI가 응답하지 않는 현상 발생

### 2. 비동기 + Non-blocking: 작업은 맡겨두고 다른 일 하기

가장 유연한 조합으로, 작업을 시작한 후 결과를 기다리지 않고 바로 다음 코드로 넘어갑니다. 커피숍에서 주문 후 진동벨을 받고 자리에 가서 다른 일을 하다가, 벨이 울리면 커피를 받으러 가는 것과 비슷합니다.

```javascript
// 파일을 비동기적으로 읽는 예시
fs.readFile('big-file.txt', (err, data) => {
  // 파일 읽기가 완료되면 이 콜백 함수가 실행됨
  console.log('파일 읽기 완료');
});
console.log('다음 작업 진행 중'); // 즉시 실행됨
```

#### 특징

- 시간이 오래 걸리는 작업도 프로그램의 다른 부분을 막지 않음
- 콜백 함수나 Promise를 통해 작업 완료 시 처리할 로직 정의
- JavaScript의 주요 비동기 패턴, Node.js의 핵심 철학

### 3. 동기 + Non-blocking: 일하면서 계속 확인하기

작업을 시작하고 주기적으로 완료 여부를 확인하는 방식입니다. 음식을 주문한 후 계속 카운터를 쳐다보며 "내 음식 나왔나요?"라고 물어보는 것과 비슷합니다.

```javascript
let taskComplete = false;
const taskId = startBackgroundTask();

// 작업 완료 여부를 주기적으로 확인
function checkStatus() {
  const status = getTaskStatus(taskId); // 즉시 반환됨
  
  if (!status.complete) {
    console.log('아직 진행 중...');
    setTimeout(checkStatus, 1000); // 1초 후 다시 확인
  } else {
    console.log('작업 완료!');
    taskComplete = true;
  }
}

checkStatus();
console.log('다른 작업 수행 가능'); // 즉시 실행됨
```

#### 특징

- 폴링(polling) 방식으로 작업 상태 확인
- 메인 스레드가 차단되지 않아 다른 작업 가능
- 주기적인 상태 확인으로 인한 오버헤드 발생

### 4. 비동기 + Blocking: 한 곳에선 막히고 다른 곳에선 흐르는 물

비교적 드문 조합으로, 비동기 작업이 시작되었지만 특정 부분에서는 블로킹이 발생하는 상황입니다. 주로 멀티스레드 환경에서 발생하며, 한 스레드는 블로킹되어도 다른 스레드는 계속 실행됩니다.

```javascript
// Java 예시 (JavaScript는 단일 스레드라 직접적인 예시가 어려움)
new Thread(() => {
  // 비동기적으로 새 스레드 시작
  socketChannel.connect(serverAddress); // 블로킹 호출
  // 연결될 때까지 이 스레드는 블로킹됨
  console.log('연결 완료');
}).start();

// 메인 스레드는 계속 실행
console.log('다른 작업 수행 중');
```

#### 특징

- 특정 스레드나 컨텍스트에서만 블로킹이 발생
- 시스템 전체가 멈추지는 않음
- 비동기 I/O 작업에서 동기식 API를 호출할 때 발생 가능

## 언제 어떤 방식을 사용해야 할까?

각 패턴의 특징을 더 읽기 쉬운 형식으로 정리해보겠습니다.

### 동기 + Blocking

#### 적합한 사용 상황

- 간단한 스크립트나 유틸리티
- 순차적 실행이 필수적인 작업
- 즉각적인 결과가 필요한 경우
- 초기화 코드 또는 설정 단계
- CLI(Command Line Interface) 애플리케이션

#### 장점

- 코드 흐름 예측이 쉬움
- 디버깅이 간편함
- 에러 처리가 직관적
- 작업 순서 보장

#### 단점

- 시간이 오래 걸리는 작업 시 전체 프로그램이 멈춤
- 리소스 활용률이 낮음
- UI 애플리케이션에서 응답 불가 상태 유발
- 확장성 제한

#### 예시

```javascript
const fs = require('fs');
const data = fs.readFileSync('file.txt');
console.log(data);
// 파일 읽기가 완료된 후에만 다음 코드 실행
```

### 비동기 + Non-blocking

#### 적합한 사용 상황

- 웹 서버 및 API 서비스
- 대규모 동시성 처리가 필요한 애플리케이션
- 사용자 인터페이스(UI) 애플리케이션
- I/O 작업이 많은 서비스
- Event-driven 시스템

#### 장점

- 높은 처리량과 확장성
- 자원 효율적 사용
- UI 반응성 유지
- 동시에 여러 작업 처리 가능

#### 단점

- 코드 복잡도 증가
- 콜백 지옥 가능성
- 디버깅이 어려움
- 에러 처리가 복잡

#### 예시

```javascript
fs.readFile('file.txt', (err, data) => {
  if (err) throw err;
  console.log(data);
});
console.log('다음 작업 진행 중'); // 즉시 실행
```

### 동기 + Non-blocking

#### 적합한 사용 상황

- 상태 모니터링 시스템
- 게임 루프
- 실시간 데이터 처리
- 폴링이 필요한 시스템
- 프로세스 관리

#### 장점

- 지속적인 작업 상태 확인 가능
- 타이밍 제어 가능
- 메인 스레드 차단 방지
- 간단한 동시성 구현

#### 단점

- 불필요한 리소스 소모(CPU 사이클)
- 폴링 간격 설정의 어려움
- 즉각적인 반응이 어려움
- 복잡한 상태 관리 필요

#### 예시

```javascript
function checkStatus() {
  const status = getTaskStatus();
  if (!status.complete) {
    setTimeout(checkStatus, 1000);
  } else {
    console.log('완료!');
  }
}
checkStatus();
console.log('다른 작업 가능');
```

### 비동기 + Blocking

#### 적합한 사용 상황

- 멀티스레드 환경
- 특정 리소스에 대한 동기화 필요 시
- 저수준 I/O 작업
- 신호 처리(Signal handling)
- 병렬 계산의 동기화 지점

#### 장점

- 특정 스레드만 블로킹되고 시스템은 계속 작동
- 동기화 문제 해결에 유용
- 특정 작업 완료 보장
- 자원 공유 제어 가능

#### 단점

- 데드락 가능성
- 복잡한 동시성 관리
- 리소스 활용 비효율
- 스레드 관리 오버헤드

#### 예시

```java
// Java 예시
new Thread(() -> {
  // 비동기적 스레드 시작
  socket.connect(); // 블로킹 호출
  // 연결될 때까지 이 스레드만 블로킹
}).start();
// 메인 스레드는 계속 실행
```


이 네 가지 패턴을 요구사항에 맞게 적재적소에 활용하면 더 효율적이고 반응성 높은 프로그램을 만들 수 있습니다!@
