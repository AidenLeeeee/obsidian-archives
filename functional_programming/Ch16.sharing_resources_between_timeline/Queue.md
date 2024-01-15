## 동시성 기본형 만들기

자바스크립트에는 큐(Queue)가 존재하지 않는다.
따라서, 여러 액션을 하나의 타임라인에서 순서대로 실행하도록 하는 타임라인에서의 큐를 사용하기 위해서는 직접 구현하여야 한다.

```javascript
function Queue(worker) {
	const queue_items = [];
	let working = false;

	function runNext() {
		if (working) return;
		if (queue_items.length === 0) return;
		working = true;
		const item = queue_items.shift();

		worker(item.data, function() {
			working = false;
			runNext();
		});
	}

	return function(data) {
		queue_items.push(data);
		setTimeout(runNext, 0);
	};
}
```

위에서 구현된 Queue 는 아래와 같이 사용할 수 있다.

```javascript
function calc_cart_worker(cart, done) {
	calc_cart_total(cart, function(total) {
		update_total_dom(total);
		done(total);
	});
}

const update_total_queue = Queue(calc_cart_worker);
```

구현된 Queue() 는 액션의 **순서 보장**을 가능하게 해주는 도구이다.

Queue() 라는 말 대신 linearize() 라고 할 수도 있다.
Queue 가 액션 호출을 순서대로 만들기 때문이다.

Queue 는 동시성 기본형이다.
여러 타임라인을 올바르게 동작하도록 만드는 재사용 가능한 코드이다.
동시성 기본형은 방법은 다르지만 모두 실행 가능한 순서를 제한하면서 동작한다.
기대하지 않는 순서를 없애면 코드가 기대한 순서로 동작한다는 것을 보장할 수 있다.

이제 Queue 를 사용해 DOM 업데이트를 같은 타임라인에서 하도록 만들었기 때문에 순서 문제가 생기지 않는다.
제품 추가 버튼을 누르는 순서대로 DOM 업데이트가 실행된다.