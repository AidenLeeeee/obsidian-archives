
## Code Smell

```javascript
// 넥타이 하나를 구매하면 넥타이 클립을 무료로 증정하는 코드
function freeTieClip(cart) {
	var hasTie = false;
	var hasTieClip = false;
	for (var i = 0; i < car.length; i++) {
		var item = cart[i]
		if (item.name === "tie") hasTie = true;
		if (item.name === "tie clip") hasTieClip = true;
	}
	if (hasTie && !hasTieClip) {
		var tieClip = makeItem("tie clip", 0);
		return addItem(cart, tieClip);
	}
	return cart;
}
```

- 이 코드는 설계되지 않은 코드이다.
- 이 코드는 첫 번째 계층형 설계 패턴인 직접 구현을 따르지 않고 있다.
- `freeTieClip()` 함수는 마케팅에 관련된 함수이지만, 장바구니가 배열이라는 사실을 알고 있을 필요가 없다.


## Solution

### 1. 장바구니가 해야 할 동작 정의

> 장바구니가 해야 할 동작을 아래와 같이 정리

  - 제품 추가하기
  - 제품 삭제하기
  - 장바구니에 제품이 있는지 확인하기
  - 합계 계산하기
  - 장바구니 비우기
  - 제품 이름으로 가격 설정하기
  - 세금 계산하기
  - 무료 배송이 되는지 확인하기

### 2. 제품이 있는지 확인하는 함수

> 장바구니 안에 제품이 있는지 확인하는 함수가 있다면, 저수준의 반복문을 직접 쓰지 않았을 것이다.
> 저수준의 코드는 추출해야 할 가능성이 높다.

```javascript
// 제품이 있는지 확인하는 함수
function isInCart(cart, name) {
	for (var i = 0; i < cart.length; i++) {
		if (cart[i].name === name) return true;
	}
	return false;
}

// 넥타이 하나를 구매하면 넥타이 클립을 무료로 증정하는 코드
function freeTieClip(cart) {
	var hasTie = isInCart(cart, "tie");
	var hasTieClip = isInCart(cart, "tie clip");
	if (hasTie && !hasTieClip) {
		var tieClip = makeItem("tie clip", 0);
		return addItem(cart, tieClip);
	}
	return cart;
}
```

- 개선된 함수는 짧고 명확하다.
- 비슷한 구체화 수준에서 작동하고 있어 가독성이 높다.

### 3. 함수 호출을 시각화

> 함수에서 사용하는 다른 함수와 언어 기능을 호출 그래프(Call graph)로 그릴 수 있다.
> `makeItem` 과 `addItem` 은 직접 만든 함수이기 때문에 언어에서 제공하는 배열 인덱스, 반복문 기능보다 높은 추상화 수준을 가지고 있다.

**기존의 호출 그래프**
- `freeTieClip()`
	- `makeItem()`
	- `addItem()`
		- `array index`
		- `for loop`

### 4. 비슷한 추상화 계층에 있는 함수를 호출

> 서로 다른 추상화 단계에 있는 기능을 사용하면 직접 구현 패턴이 아니다.
> 개선된 함수처럼 호출하는 함수들이 서로 비슷한 계층에 있다면 직접 구현했다고 할 수 있다.

**개선된 호출 그래프**
- `freeTieClip()`
	- `isInCart()`
	- `makeItem()`
	- `addItem()`

### 5. 같은 계층에 있는 함수는 같은 목적을 가진다

> 각 계층은 추상화 수준이 다르다. 그래서 어떤 계층에 있는 함수를 읽거나 고칠 때 낮은 수준의 구체적인 내용은 신경쓰지 않아도 된다.
> 예를 들면 '장바구니 비즈니스 규칙' 계층에 있는 함수를 사용할 때, 장바구니가 배열로 구현되어 있다는 것과 같은 구체적인 내용을 신경쓰지 않아도 되는 것과 같다.

**각 계층의 목적**
- 장바구니 비즈니스 규칙
- 일반적인 비즈니스 규칙
- 장바구니 기본 동작
- 제품에 대한 기본 동작
- 카피-온-라이트 동작
- 자바스크립트 언어 지원 기능

위와 같이 나누어진 계층의 목적은 각 계층에 있는 함수들의 목적과 같다.


## Summary

- 계층형 설계는 코드를 **추상화 계층**으로 구성한다. 각 계층을 살펴볼 때 다른 계층의 구체적인 내용을 알 필요가 없다.
- 문제 해결을 위한 함수를 구현할 때 어떤 구체화 단계로 사용할지 결정하는 것이 중요하다.
- 함수 이름, 본문, 호출 그래프 등은 함수가 어떤 계층에 속할지 알려주는 단서와 같다.
	- 함수 이름은 의도를 알려준다. 비슷한 목적의 이름을 가진 함수를 함께 묶을 수 있다.
	- 함수 본문은 중요한 세부 사항을 알려준다. 결국 함수가 어떤 계층 구조에 있어야 하는지 알려준다.
	- 호출 그래프로 구현이 직접적이지 않다는 것을 알 수 있다. 함수를 호출하는 화살표가 다양한 길이를 가지고 있다면 직접 구현되어 있지 않다는 신호이다.