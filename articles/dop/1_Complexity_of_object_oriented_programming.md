# 1 복잡한 OOP
이번 단원에서는 이런걸 알아본다.
- 시스템을 복잡하게 만드는 OOP
- OOP 시스템은 왜 이해하기 어려운지
- 오브젝트에 코드와 데이터를 섞는 비용

이번 장에서는 OOP가 복잡해지는 이유를 살펴봅니다.
OOP의 구문이나 의미가 프로그램을 복잡하게 만드는게 아닙니다. State와 state를 수정/접근하는 method로 이뤄진 오브젝트들로 프로그램을 짜나가는 근본 방식에서 비롯합니다.

수년간 OOP생태계는 새로운 기능(e.g., anonymous classes, anonymous funcion)을 추가하고 간단한 인터페이스를 제공하는 프레임워크를 개발해 복잡성을 완화시켰습니다. (e.g., Spring ans Jackson in Java)
프레임워크는 내부적으로 reflection, custom annotation과 같은 고급기능에 의존합니다.

