## Immutable Object

### ❓ 관련 문제

"도메인은 불변성을 유지하는 것이 좋다"

[>> 자동차 경주 미션 불변 유지 피드백](https://github.com/woowacourse/java-racingcar/pull/327#discussion_r805304408)
### 📝 공부 내용

#### 객체를 불변으로 만드는 방법

- 단순히 변수에 final 만 붙인다고 해서 객체가 불변으로 되는 것은 아니였다.
- 인스턴스 변수를 private final로 선언한다.
- setter를 통해서 값을 변경하지 못하도록 한다.
- 참조 객체가 있다면 해당 객체도 불변을 유지해야 한다.
- List, Set, Map 과 같은 가변 객체를 사용할 경우 final을 선언해도 add()와 같은 메서드는 작동하기 때문에 방어적인 복사를 사용해야 한다.

#### 불변 객체의 장점

- Thread-Safe하여 병렬 프로그래밍에 유용하며, 동기화를 고려하지 않아도 된다.
- 부수 효과(Side Effect)를 피해 오류 가능성을 최소화할 수 있다.
- 다른 사람이 작성한 함수를 예측 가능하며 안전하게 사용하도록 신뢰도가 올라간다.

#### 불변 객체의 단점
- 객체가 가지는 값마다 새로운 객체를 계속 생성하기 때문에 성능저하를 발생시킬 수 있습니다.

### 👨‍💻 해결 방법

자동차의 위치 상태를 관리하는 Position 객체를 1개 생성하여 변화시키는 방법을 사용하기 보단, 자동차의 위치가 변할 때 마다 Position 객체를 새로 생성하는 방식으로 문제를 해결했다.

``` java
private static final int MOVE_ONE_TIME=1;
public Position increase(){
    return new Position(position+MOVE_ONE_TIME);
}
```

[>> 관련 Commit](https://github.com/woowacourse/java-racingcar/pull/327/commits/aa0bd350e96109e350501077db914d44277c7603)

### 참고

이펙티브 자바: [아이템 17] 변경 가능성을 최소화하다.
