
1. **파싱과 AST 생성**
	- HTML 파서가 `<script>` 태그를 만나면 JavaScript 코드를 파싱
	- 토큰화(Tokenizing)를 거쳐 추상 구문 트리(AST) 생성
	- 구문 오류가 있다면 이 단계에서 발견됨
2. **바이트코드 생성**
	- AST를 바이트코드로 변환
	- V8 엔진의 경우 Ignition 인터프리터가 이 작업 수행
3. **실행 컨텍스트 생성**
	- 전역 실행 컨텍스트가 생성되고 콜 스택에 푸시
	- 함수 호출 시마다 새로운 실행 컨텍스트가 생성
	- 각 컨텍스트는 `Variable Environment`, `Lexical Environment`, `This Binding`을 포함
4. **코드 실행** 
	- 생성된 실행 컨텍스트의 렉시컬 환경에서 변수/함수 선언 처리 (호이스팅)
	- 코드가 한 줄씩 실행됨
	- 이벤트 루프를 통한 비동기 작업 처리
5. **최적화 (JIT Compilation)**
- 자주 실행되는 코드는 머신 코드로 컴파일
- V8 엔진의 경우 TurboFan이 이 작업 수행
- 실행 중에도 계속해서 최적화/탈최적화가 발생

이 모든 과정은 단일 스레드인 메인 스레드에서 일어나며, Web API와 이벤트 루프를 통해 비동기 작업을 처리합니다.
### 실행 과정

1. **동기 코드 실행**
   - 코드가 콜스택에 들어가서 순차적으로 실행
   - 이때 비동기 함수(setTimeout, Promise 등)를 만나면 WebAPI로 보냄

2. **비동기 작업 처리**
   - WebAPI에서 해당 작업 실행 (타이머, HTTP 요청 등)
   - 작업 완료 후 해당 콜백을 큐로 이동
   ```javascript
   console.log('1');
   setTimeout(() => console.log('2'), 0);
   Promise.resolve().then(() => console.log('3'));
   console.log('4');
   // 출력: 1, 4, 3, 2
   ```

3. **태스크 큐 구분**
   - 마이크로태스크 큐: Promise, queueMicrotask, process.nextTick(Node.js)
   - 매크로태스크 큐: setTimeout, setInterval, setImmediate, requestAnimationFrame

4. **이벤트 루프 처리 순서**
   1. 콜스택이 비었는지 확인
   2. 비었다면 마이크로태스크 큐 모두 실행
   3. 매크로태스크 큐에서 하나 실행
   4. 다시 1번으로 반복

이렇게 비동기 작업들이 적절한 큐에 배치되어 이벤트 루프에 의해 처리되는 것입니다.