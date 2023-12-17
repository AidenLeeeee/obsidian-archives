### Pattern 1. 직접 구현

> 직접 구현된 함수를 읽을 때, 함수 시그니처가 나타내고 있는 문제를 함수 본문의 적절한 구체화 수준에서 해결.
> 너무 구체적이라면, Code smell 🤮


### Pattern 2. 추상화 벽

> 어떤 계층은 중요한 세부 구현을 감추고 인터페이스를 제공.
> 인터페이스를 사용하여 코드를 작성하면, 고수준의 추상화 단계만 생각하면 되기 때문에 높은 차원으로 고민 가능.


### Pattern 3. 작은 인터페이스

> 시스템이 커질수록 비즈니스 개념을 나타내는 중요한 인터페이스는 작고 강력한 동작으로 구성하는 것이 좋다.
> 다른 기능들도 직간접적으로 최소한의 인터페이스를 유지하면서 정의한다.


### Pattern 4. 편리한 계층

> 계층형 설계 패턴은 개발자의 요구를 만족시키면서도 비즈니스 문제를 해결할 수 있어야 한다.
> 소프트웨어를 더 빠르고 고품질로 제공하는 데 도움이 되는 계층을 고민하고 작성하는데 시간을 투자해야 한다.