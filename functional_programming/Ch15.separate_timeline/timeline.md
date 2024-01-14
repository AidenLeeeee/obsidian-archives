### 3 Steps for timeline diagram

1. 액션을 확인한다.
2. 각 액션을 그린다.
3. 단순화한다.


### 예시

```javascript
saveUserAjax(user, function() {
	 setUserLoadingDOM(false);
});
setUserLoadingDOM(true);
saveDocumentAjax(document, function() {
	setDocLoadingDOM(false);
});
setDocLoadingDOM(true);
```

`setUserLoadingDOM(false)` 는 콜백 안에 있다. 콜백은 비동기로 실행되기 때문에 요청이 끝나는 시점에 언젠가 실행된다.
비동기 콜백이기 때문에 새로운 타임라인이 필요하다.
그리고 콜백이 ajax 함수 뒤에 실행된다는 사실을 점선으로 표시한다.
요청을 보내기 전에 응답을 받을 수는 없기 때문에 점선으로 순서를 표시해야 한다.

`setUserLoadingDOM(true)` 는 콜백이 아니므로 원래 타임라인에서 실행된다.

`saveDocumentAjax()` 는 콜백 밖에 있으므로 원래 타임라인에서 실행된다.

`setDocLoadingDOM(false)` 는 콜백 안에 있는 코드이기 때문에 새로운 타임라인에 그린다.
이 콜백은 미래에 응답이 도착하는 시점에 실행된다.
네트워크 상황을 예측할 수 없어서 언제 실행될지 알 수 없다.

`setDocLoadingDOM(true)` 는 콜백 밖에 있으므로 원래 타임라인에서 실행된다.

![](../../assets/images/timeline_diagram.png)