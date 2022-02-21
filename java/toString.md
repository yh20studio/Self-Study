## toString()

### 관련 문제

ErrorMessage를 관리하는 Enum에서 해당 메세지를 String으로 반환하고 싶을 때 toString()을 활용해서 Enum의 상수 값을 꺼내서 사용했다.

[>> 자동차 경주 미션 toString() 피드백](https://github.com/woowacourse/java-racingcar/pull/393#discussion_r809924380)

### 공부 내용

#### 일반적인 toString() 사용하는 이유

- 사용하려는 객체에 toString()을 재정의하면 로깅 및 디버깅 시 개발자에게 좀 더 유익한 정보를 전해줄 수 있습니다.


- ![](../../../Desktop/스크린샷 2022-02-22 오전 12.32.07.png)

#### 재정의 시 규칙

- 재정의할 때 가장 중요한 건 객체 스스로를 완벽히 설명하는 문자열이어야 한다는 것입니다. 보통은 객체가 가진 주요 정보를 모두 반환하는 것이 좋습니다.

#### toString() 사용이 적절하지 않은 경우

- 사용자에게 보여지기 위한 값을 출력하는 용도로 사용하는 것은 적절하지 않다.
    - 이미 재정의 한 toString()이 있다면 출력 형식 변화로 인하여 메서드를 변경할 때, toString()을 사용하는 모든 객체들이 영향을 받게 된다.

#### toString() In Enum

- Enum에서는 toString()은 Enum의 이름 값(name)을 리턴하는 것으로 이미 정의되어있다.
- Enum에서는 대부분의 경우에 toString()은 name()와 같은 역할을 한다.
- Enum의 이름값을 통해서 해당 Enum 객체를 생성하는데 많이 사용한다.
    - Ex> ```WeekDay.valueOf(WeekDay.MONDAY.name())```

#### name() vs toString()

- 많은 개발자들이 name()보다는 toString()에 대한 직관성이 높아서 toString()을 사용하라고 추천한다.
- 둘의 차이점은 name()은 final 메서드이기 때문에 재정의하는 것이 불가능하다. 하지만 toString()은 가능하고 name()으로 나타낼 수 없는 형식의 로그를 보이고 싶을 때 사용한다.
    - 예시로 name()값이 APPLE_IN_BAG 값을 반환한다면, 사용자가 읽기 편하도록 APPLE IN BAG으로 toString()을 활용해서 나타낼 수 있다.

### 해결 방법

toString()이 아닌 getMessage()로 메세지 값을 꺼내서 사용하는 방식이 더 적절하다고 생각하여 변경했습니다.

[>> 관련 Commit](https://github.com/woowacourse/java-racingcar/pull/393/commits/7555c038659267c4695afaf57be8609b0afcb940)

### 참고

이펙티브 자바: [아이템 12] toString을 항상 재정의하라

[What is the difference between `Enum.name()` and `Enum.toString()`? ](https://stackoverflow.com/questions/18031125/what-is-the-difference-between-enum-name-and-enum-tostring)
