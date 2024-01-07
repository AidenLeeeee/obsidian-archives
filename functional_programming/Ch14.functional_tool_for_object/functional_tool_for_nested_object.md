### 예시 (리팩터링 전)

> 앞서 update() 함수를 통해, 특정 객체의 특정 컬럼에 동일한 동작을 수행하는 여러 코드의 중복을 제거할 수 있었다.

```javascript
function update(object, key, modifyFn) {
	const value = object[key];
	const newValue = modifyFn(value);
	const newObject = objectSet(object, key, newValue);
	return newObject;
}
```

하지만 위 update() 함수는 n 단계로 중첩되어 있는 모든 객체에서 정상적으로 동작하지는 않는다.

```javascript
const shirt = {
	name: "shirt",
	price: 13,
	options: {
		color: "blue",
		size: 3
	}
}

function incrementSize(item) {
	const options = item.options;
	const newOptions = update(options, 'size', increment);
	const newItem = objectSet(item, 'options', newOptions);
	return newItem;
}

function update(object, key, modifyFn) {
	const value = object[key];
	const newValue = modifyFn(value);
	const newObject = objectSet(object, key, newValue);
	return newObject;
}

function increment(value) {
	return value + 1;
}
```

위와 같이 두 단계로 중첩되어 있는 객체의 특정 컬럼을 변경할 경우,
그 중첩 단계 수만큼 조회와 설정의 반복이 발생하게 된다.

```javascript
function incrementSize(item) {
	return update(item, 'options', function(options) {
		return update(options, 'size', increment);
	});
}

function update(object, key, modifyFn) {
	const value = object[key];
	const newValue = modifyFn(value);
	const newObject = objectSet(object, key, newValue);
	return newObject;
}

function increment(value) {
	return value + 1;
}
```

결국 중첩 단계 수만큼 update 함수는 콜백으로 전달되어야 하며, 중첩 단계가 많아질수록 콜백이 많아져 가독성은 떨어지게 된다.

### 예시 (리팩터링 후)

```javascript
function incrementSize(item) {
	return updateNestedTwice(item, 'options', 'size', function(size) {
		return size + 1;
	});
}

function updateNestedTwice(object, key1, key2, modifyFn) {
	return update(object, key1, function(value1) {
		return update(value1, key2, modifyFn);
	});
}

function update(object, key, modifyFn) {
	const value = object[key];
	const newValue = modifyFn(value);
	const newObject = objectSet(object, key, newValue);
	return newObject;
}
```

update() 함수를 두 번 호출하는 로직을 updateNestedTwice() 함수로 만들어주었다.
이제 updateNestedTwice() 함수를 사용하면, 두 번 중첩된 객체에 대한 작업을 손쉽게 구현할 수 있다.

하지만 updateNestedTwice() 함수 역시 두 번 중첩된 객체에만 적용이 가능하다는 한계가 있으며,
중첩 단계에 따라 인자의 개수도 늘어나게 된다는 단점이 존재한다.

### 예시 (두 번째 리팩터링 후)

```javascript
function nestedUpdate(object, keys, modifyFn) {
	if (keys.length === 0) return modifyFn(object);
	const key1 = keys[0];
	const restOfKeys = drop_first(keys);
	return update(object, key1, function(value1) {
		return nestedUpdate(value1, restOfKeys, modifyFn);
	});
}
```

nestedUpdate() 함수는 0을 포함해 중첩된 깊이에 상관없이 활용할 수 있다.
재귀 함수(Recursive Function) 이기 때문에 자신을 참조하고 있다.