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
		const cart = queue_items.shift();

		worker(cart, function() {
			working = false;
			runNext();
		});
	}

	return function(cart) {
		queue_items.push(cart);
		setTimeout(runNext, 0);
	};
}

function calc_cart_worker(cart, done) {
	calc_cart_total(cart, function(total) {
		update_total_dom(total);
		done(total);
	});
}

const update_total_queue = Queue(calc_cart_worker);
```