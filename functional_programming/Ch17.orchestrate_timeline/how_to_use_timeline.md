## 요약: 타임라인 사용하기

#### 1. 타임라인 수를 줄이기

스레드나 비동기 호출, 서버 요청을 줄여 적은 타임라인을 만들면 시스템이 단순해진다.

#### 2. 타임라인 길이를 줄이기

타임라인에 있는 액션을 줄인다.
액션을 계산으로 바꾼다.
암묵적인 입력과 출력을 없앤다.

#### 3. 공유 자원을 없애기

공유하는 자원을 줄인다.
자원을 공유하지 않는 타임라인은 순서 문제가 생기지 않는다.
가능하다면 단일 스레드에서 공유 자원에 접근한다.

#### 4. 동시성 기본형으로 자원을 공유하기

자원을 안전하지 않게 공유한다면 큐나 락을 사용해 안전하게 공유할 수 있는 방법으로 바꾼다.

#### 5. 동시성 기본형으로 조율하기

타임라인을 조율하기 위해 Promise 나 Cut 과 같은 것으로 액션에 순서나 반복을 제한한다.