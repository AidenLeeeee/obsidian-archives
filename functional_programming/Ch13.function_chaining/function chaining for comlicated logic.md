### 문제

> map, filter, reduce 와 같은 함수형 도구를 활용하여 반복과 함께 다양한 작업을 구현할 수 있다.
> 하지만 계산이 복잡해지면 함수형 도구 하나로는 구현할 수 없다.
> 복잡한 계산을 단계 별로 분리하고, 각 단계를 함수형 도구를 활용해 구현한 뒤, 이 단계들을 조합하여 복잡한 작업을 구현할 수 있다.

### 예시

모든 고객 중에서 3개 이상을 주문한 고객을 우수 고객으로 정의하고, 각각의 우수 고객들의 구매 중 가장 비싼 구매 건에 대해 반환하는 예시.

각 단계에서의 작업
1. 우수 고객(3개 이상 구매)를 필터링 (filter)
2. 우수 고객을 가장 비싼 구매로 변환 (map)

```javascript
function biggestPuchasesBestCustomers(customers) {
	// 단계 1. 우수 고객(3개 이상 구매)를 필터링
	const bestCustomers = filter(customers, function(customer) {
		return customer.puchases.length >= 3;
	})

	// 단계 2. 우수 고객을 가장 비싼 구매로 변환
	const biggestPurchases = map(bestCustomers, function(customer) {
		return reduce(customer.purchases, { total: 0 }, function(biggestSoFar, purchase) {
			if (biggestSoFar.total > purchase.total) return biggestSoFar;
			else return purchase;
		})
	})

	return biggestPurchases;
}
```

**위 함수는 우수 고객을 필터링하고, 각 우수 고객의 구매 중 가장 비싼 구매 건을 반환하는 함수이다.**
그러나 콜백이 여러 개 중첩되어 함수가 너무 커졌고, 때문에 가독성이 좋지 않다.

### 해결 1. 작업을 분리하기

배열에서 가장 큰 값을 찾는 `maxKey()` 함수를 구현하여, 관련 작업을 분리할 수 있다.

```javascript
function biggestPuchasesBestCustomers(customers) {
	// 단계 1. 우수 고객(3개 이상 구매)를 필터링
	const bestCustomers = filter(customers, function(customer) {
		return customer.puchases.length >= 3;
	});

	// 단계 2. 우수 고객을 가장 비싼 구매로 변환
	const biggestPurchases = map(bestCustomers, function(customer) {
		return maxKey(customer.purchases, { total: 0 }, function(purchase) {
			return purchase.total;
		});
	});

	return biggestPurchases;
}

// 배열에서 가장 큰 값을 찾는 함수
function maxKey(array, init, f) {
	return reduce(array, init, function(biggestSoFar, element) {
		if (f(biggestSoFar) > f(element)) return biggestSoFar;
		else return element;
	});
}
```

**특정 작업을 새로운 함수로 분리하면서, 콜백 함수를 줄이고 가독성을 높일 수 있었다.**

### 해결 2. 콜백을 분리하기

인라인으로 정의한 콜백 함수를 분리하여 구현하고, 재사용성 및 가독성을 높일 수 있다.

```javascript
function biggestPurchasesBestCustomers(customers) {
	const bestCustomers = filter(customers, isBestCustomer);
	const biggestPurchases = map(bestCustomers, getBiggestPurchase);
	return biggestPurchases;
}

// 콜백 1. 우수 고객 여부를 반환하는 함수
function isBestCustomer(customer) {
	return customer.purchases.length >= 3;
}

// 콜백 2. 고객의 구매 중 가장 비싼 구매 건을 반환하는 함수
function getBiggestPurchase(customer) {
	return maxKey(customer.purchases, { total: 0 }, getPurchaseTotal);
}

// 콜백 3. 인자로 받은 구매 내역의 금액을 반환하는 함수
function getPurchaseTotal(purchase) {
	return purchase.total;
}

// 배열에서 가장 큰 값을 찾는 함수
function maxKey(array, init, f) {
	return reduce(array, init, function(biggestSoFar, element) {
		if (f(biggestSoFar) > f(element)) return biggestSoFar;
		else return element;
	});
}
```
