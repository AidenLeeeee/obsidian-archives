직접 구현, 추상화 벽, 작은 인터페이스의 세 가지 패턴이 가장 이상적인 계층 구성을 만드는 방법에 대해 설명하고 있다면,
**편리한 계층 패턴**은 조금 더 현실적이고 실용적인 측면을 다루고 있다.

**편리한 계층 패턴**은 언제 패턴을 적용하고 또 언제 멈춰야 하는지 실용적인 방법을 알려준다.

## Solution

### 1. 설계를 멈춰야 할 때

> 만약 작업하는 코드가 편리하다고 느낀다면 설계는 조금 멈춰도 된다.

현재 작업 중인 코드가 편리하다면 반복문은 감싸지 않고 그대로 두고 호출 화살표가 조금 길어지거나 계층이 다른 계층과 섞어도 그대로 둘 수 있다.

### 2. 설계를 시작해야 할 때

> 만약 구체적인 것을 너무 많이 알아야 하거나, 코드가 지저분하다고 느껴진다면 다시 패턴을 적용하여야 한다.

어떤 코드도 이상적인 모습에 도달할 수 없다.
언제나 설계와 새로운 기능의 필요성 사이 어느 지점에 머물게 된다.

### 3. 계층으로 보는 비기능적 요구사항

> 호출 그래프의 구조를 통해 소프트웨어의 비기능적 요구사항을 알아낼 수 있다.
> 비기능적 요구사항이란 유지보수성, 테스트성, 재사용성과 같은 특성들로, 소프트웨어 설계를 해야 하는 중요한 이유이다.

**1. 유지보수성**
- 요구 사항이 바뀌었을 때 가장 쉽게 고칠 수 있는 코드는 어떤 코드인가?

**2. 테스트성**
- 어떤 것을 테스트하는 것이 가장 중요한가?

**3. 재사용성**
- 어떤 함수가 재사용하기 좋은가?

## Summary

**그래프의 가장 위에 있는 코드가 고치기 가장 쉽다**
- 가장 상위 계층에 있는 코드가 고치기 쉽다.
- 가장 상위 계층에 있는 코드는 아무 곳에서도 호출하는 곳이 없기 때문에 다른 코드에 영향을 주지 않고 변경할 수 있다.
- 하위 계층에 있는 코드는 그 위에 너무 많은 코드가 만들어져 있기 때문에 상대적으로 고치기 어렵다.
- 따라서 직접 구현 패턴처럼 함수를 추출해 더 낮은 계층으로 이동시키거나, 작은 인터페이스 패턴처럼 더 높은 계층에 함수를 추가하는 일은 모두 변경 가능성을 생각해 그에 따라 판단하여야 한다.

**아래에 있는 코드는 테스트가 중요하다**
- 테스트를 작성하는 데는 오랜 시간이 소요되며, 비즈니스를 위해 효율적으로 움직여야 한다.
- 코드를 잘 작성하고 있다면, 자주 바뀌는 코드는 위로 올리고 더 안정적인 코드는 아래에 둘 것이다.
- 따라서 상위 계층에 있는 코드가 자주 바뀌면 해당 코드의 테스트 코드도 바뀐 행동에 맞게 고쳐주어야 한다.
- 반대로 하위 계층에 있는 코드는 자주 바뀌지 않기 때문에 테스트 코드도 자주 고칠 필요가 없다.
- 패턴을 사용하여 설계하면, 테스트 가능성에 맞춰 코드를 계층화할 수 있다.
- 하위 계층으로 코드를 추출하거나 상위 계층에 함수를 만드는 일은 테스트의 가치를 결정한다.

**아래에 있는 코드가 재사용하기 더 좋다**
- 계층형 구조를 만들면 자연스럽게 재사용성이 좋아진다.
- 낮은 계층으로 함수를 추출하면 재사용할 가능성이 많아진다.
- 낮은 계층은 재사용하기 더 좋다.