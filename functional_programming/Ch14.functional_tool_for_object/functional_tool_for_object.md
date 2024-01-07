### 예시 (리팩터링 전)

```javascript
function incrementQuantity(item) {
	const quantity = item['quantity'];
	const newQuantity = quantity + 1;
	const newItem = ObjectSet(item, 'quantity', newQuantity);
	return newItem;
}
```

위 예시 코드는 특정 객체의 quantity 를 1 증가시키는 단순한 동작의 함수이다.
그러나 아래와 같은 문제점을 가지고 있다.

- 함수 이름에 있는 암묵적 인자
	- quantity 와 같이, 함수 이름에 있는 일부분을 함수 본문에서 사용하고 있다.
	- increment 와 같이, 함수 이름에 있는 동작을 함수 본문에서 사용하고 있다.

### 예시 (리팩터링 후)

```javascript
function incrementField(item, field) {
	return update(item, field, function(value) {
		return value + 1;
	})
}

function update(object, key, modifyFn) {
	const value = object[key];
	const newValue = modifyFn(value);
	const newObject = objectSet(object, key, newValue);
	return newObject;
}
```

위 예시 코드는 기존 예시 코드의 문제점을 해결한 코드이다.
아래와 같은 리팩터링 기법을 사용했다.

- 암묵적 인자를 드러내기
	- 함수 이름에 있던 Quantity 를 인자로 드러내면서, Field 를 받아 변경할 수 있도록 수정하였다.
- 함수 본문을 콜백으로 바꾸기
	- 함수 이름에 있던 increment 와 같은 특정 동작을 콜백으로 변경하여 update 라는 고차함수에서 사용하도록 수정하였다.

이제 update() 를 활용하면, **어떤 객체의 특정 컬럼에 동일한 동작을 수행하는 코드의 중복**을 제거할 수 있다.

### 활용

```javascript
function halveField(item, field) {
	return update(item, field, function(value) {
		return value / 2;
	})
}

function decrementField(item, field) {
	return update(item, field, function(value) {
		return value - 1;
	})
}

function doubleField(item, field) {
	return update(item, field, function(value) {
		return value * 2;
	})
}
```
