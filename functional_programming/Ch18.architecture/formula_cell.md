어떤 셀은 다른 셀의 값을 최신으로 반영하기 위해 파생될 수 있다.

FormulaCell 로 이미 있는 셀에서 파생한 셀을 만들 수 있다.
FormulaCell 은 다른 셀의 변화가 감지되면 값을 다시 계산한다.

## Formula cell 구현

```javascript
function FormulaCell(upstreamCell, f) {
	const myCell = ValueCell(f(upstreamCell.val()));
	upstreamCell.addWatcher(function(newUpstreamValue) {
		myCell.update(function(currentValue) {
			return f(newUpstreamValue);
		});
	});
	return {
		val: myCell.val,
		addWatcher: myCell.addWatcher
	};
}
```

FormulaCell 은 값을 직접 바꿀 수 없다.
감시하던 상위(Upstream) 셀 값이 바뀌면 FormulaCell 값이 바뀐다.
상위 셀이 바뀌면 상위 값을 가지고 셀 값을 다시 계산한다.
FormulaCell 에는 값을 바꾸는 기능은 없지만 FormulaCell 을 감시할 수 있다.