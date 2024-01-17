반응형 아키텍처를 위한 이벤트 핸들러를 구현하기 위해 **상태 모델**을 만들어야 한다.
상태는 애플리케이션에서 중요한 부분을 차지하는데, 이는 웹 애플리케이션은 물론 함수형 프로그램에서도 마찬가지이다.

## Value cell 구현

특정 변수의 상태를 일급 함수로 만들어 볼 수 있다.
아래는 변경 가능한 값을 일급 함수로 만드는 코드 예제이다.

```javascript
function ValueCell(initialValue) {
	let currentValue = initialValue;
	return {
		val: function() {
			return currentValue;
		},
		update: function(f) {
			const oldValue = currentValue;
			const newValue = f(oldValue);
			currentValue = newValue;
		}
	};
}
```

ValueCell 에는 값 하나와 두 개의 동작이 있다.
값을 읽는 `val()` 동작과 현재 값을 바꾸는 `update()` 동작이다.

이제 ValueCell 은 변경 가능한 상태를 나타내는 새로운 기본형이다.

## Value cell 을 반응형으로 만들기

이제 상태가 바뀔 때 특정 행위가 이루어지도록 만들어야 한다.

ValueCell 코드에 **감시자(Watcher)** 개념을 추가할 수 있다.
감시자는 상태가 바뀔 때마다 실행되는 핸들러 함수이다.

아래는 기존의 ValueCell 코드에 감시자 개념을 적용한 새로운 코드이다.

```javascript
function ValueCell(initialValue) {
	let currentValue = initialValue;
	const watchers = [];
	
	return {
		val: function() {
			return currentValue;
		},
		update: function(f) {
			const oldValue = currentValue;
			const newValue = f(oldValue);
			
			if (oldValue !== newValue) {
				currentValue = newValue;
				forEach(watchers, function(watcher) {
					watcher(newValue);
				});
			}
		},
		addWatcher: function(f) {
			watchers.push(f);
		}
	};
}
```

감시자를 활용해 상태가 변경되었을 때의 실행될 행위를 지정할 수 있다.

> **감시자**는 아래와 같은 이름으로 불리기도 한다.
> - 이벤트 핸들러(Event Handler)
> - 옵저버(Observer)
> - 콜백(Callback)
> - 리스너(Listener)

## Value cell 을 일관되게 유지하기

- 올바른 값으로 초기화한다.
- `update()` 에는 계산을 전달한다.
- 계산은 올바른 값이 주어졌다면 올바른 값을 리턴해야 한다.