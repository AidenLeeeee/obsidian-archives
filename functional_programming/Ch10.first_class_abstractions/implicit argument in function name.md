## Implicit argument in function name

> **함수 이름에 있는 암묵적 인자**는 함수 본문에서 사용하는 어떤 값이 함수 이름에 나타나는 것으로, 코드의 냄새(Code Smell) 이 된다.

### 특징

- 거의 똑같이 구현된 함수가 있다.
- 함수 이름이 구현의 차이를 만든다.

```javascript
function setPriceByName(cart, name, price) {
	var item = cart[name];
	var newItem = objectSet(item, 'price', price);
	vat newCart = objectSet(cart, name, newItem);
	return newCart;
}
```

## Refactor: Express implicit argument

> **암묵적 인자를 드러내기** 리팩터링은 암묵적 인자가 일급 값이 되도록 함수에 인자를 추가하는 방법이다.
> 이렇게 하면 잠재적 중복을 없애고 코드의 목적을 더 잘 표현할 수 있다.


### 단계

- 함수 이름에 있는 암묵적 인자를 확인한다.
- 명시적인 인자를 추가한다.
- 함수 본문에 하드 코딩된 값을 새로운 인자로 바꾼다.
- 함수를 호출하는 곳을 고친다.

```javascript
function setFieldByName(cart, name, field, value) {
	var item = cart[name];
	var newItem = objectSet(item, field, value);
	var newCart = objectSet(cart, name, newItem);
	return newCart;
}
```


## Refactor: Replace body with callback

> 언어의 문법 중 어떤 문법은 일급(First-class)이 아니다.
> **함수 본문을 콜백으로 바꾸기** 리팩터링으로 함수 본문에 어떤 부분(비슷한 함수에 있는 서로 다른 부분)을 콜백으로 바꾼다.
> 이렇게 하면 일급 함수로 어떤 함수에 동작을 전달할 수 있다.


### 단계

- 함수 본문에서 바꿀 부분의 앞부분과 뒷부분을 확인한다.
- 리팩터링 할 코드를 함수로 빼낸다.
- 빼낸 함수의 인자로 넘길 부분을 또 다른 함수로 빼낸다.