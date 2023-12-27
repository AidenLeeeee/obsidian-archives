### 함수를 리턴하는 함수

> 함수를 리턴하는 함수는 함수 팩토리(factory) 와 같다.
> 자동으로 정형화된 코드를 함수로 만들 수 있다.

### 예시 코드

- **예외가 발생했을 때 로그를 생성하는 함수**
```javascript
// 고차 함수 (함수를 리턴하는 함수)
function wrapLogging(f) {
	return function (...args) {
		try {
			return f(args);
		} catch (error) {
			logger.error(error);
		}
	}
}

// 사용 예시
const buyItemWithLogging = wrapLogging(buyItem);
buyItemWithLogging(items);
```

- **숫자를 증가시키는 함수**
```javascript
// 고차 함수 (함수를 리턴하는 함수)
function makeAdder(numToBeAdded) {
	return function(num) {
		return numToBeAdded + num;
	}
}

// 사용 예시 1
const incrementInventory = makeAdder(1);
incrementInventory(inventory);

// 사용 예시 2
const plus10 = makeAdder(10);
plus10(12);
```

### 요약

> 고차 함수로 패턴이나 원칙을 코드로 만들 수 있다.
> 고차 함수는 한 번 정의하고 필요한 곳에 여러 번 사용할 수 있다.
> 고차 함수로 함수를 리턴하는 함수를 만들 수 있다. 리턴 받은 함수는 변수에 할당해 이름이 있는 일반 함수처럼 쓸 수 있다.
> 고차 함수는 많은 중복 코드를 없애 주지만, 가독성을 해칠 수도 있다.