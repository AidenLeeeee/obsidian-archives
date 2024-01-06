### 문제

> filter() 와 map() 은 모두 새로운 배열을 만들어 반환한다.
> 함수가 호출될 때마다 새로운 배열이 생기기 때문에 크기가 클 수도 있고, 비효율적일 수 있다.
> 현대의 가비지 컬렉터(garbage-collector) 는 생성된 배열이 참조되지 않을 때 빠르게 처리하기 때문에 대부분의 경우에서 문제가 되지 않지만, 때로는 위와 같은 문제를 쉽게 최적화할 수 있다.


### 스트림 결합 (Stream fusion)

map() 과 filter(), reduce() 등의 체인을 최적화하는 것을 스트림 결합(stream fusion) 이라고 부른다.

**예시**
```javascript
const names = map(customers, getFullName);
const nameLengths = map(names, stringLength);
```

위 예시의 경우, 값 하나에 두 번의 map() 을 사용하고 있다.

**예시**
```javascript
const nameLengths = map(customers, function(customer) {
	return stringLength(getFullName(customer));
})
```

위 예시는 두 동작을 하나로 조합하여 한 번의 map() 으로 동일한 결과를 반환한다.
이 때는 가비지 컬렉션이 필요없다.
filter() 와 reduce() 도 동일하게 최적화할 수 있다.

**단점**
스트림 결합을 통해 두 번의 반복 작업을 하나의 반복으로 최적화할 수 있었지만, 이와 같은 방법은 병목이 생겼을 경우 사용하는 것이 좋다.
수많은 작업을 스트립 결합할 경우, 가독성이 낮아질 수 있기 때문이다.

