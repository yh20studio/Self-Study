## @ParameterizedTest

### ❓관련 문제

TEST 코드가 반복되지만 매개변수에 따라서 결과 값만 달지는 경우 중복을 줄이자.

[--> 자동차 경주 미션 ParameterizedTest 피드백 1](https://github.com/woowacourse/java-racingcar/pull/327#discussion_r805305180)

[--> 자동차 경주 미션 ParameterizedTest 피드백 2](https://github.com/woowacourse/java-racingcar/pull/393#discussion_r810495941)

### 📝 공부 내용

#### @ParameterizedTest의 name

name의 인자값을 통해서 Test시 나타나는 displayName을 매개변수에 따라서 적용할 수 있다.

``` java
@ParameterizedTest(name = "{index}: 예외: {1}")
@MethodSource("provideCarNameAndErrorMessage")
void constructor_exception_test(String carName, String errorMessage) {
    assertThatThrownBy(() -> new CarName(carName))
        .hasMessageContaining(errorMessage);
}
```

#### @ValueSource

ParameterizedTest의 간단한 매개변수 값을 설정할 때 사용한다. strings, ints, booleans 등 원시 변수와 string을 사용할 수 있다. 구분은 ","로 구분하여 지정한다.

``` java
@ParameterizedTest
@ValueSource(strings = {"name1", "name1,name2"})
void judgeWinners_test(String inputNames) {}
```

#### @NullSource

ParameterizedTest의 null을 매개변수로 하는 Test를 진행할 수 있다.

``` java
@ParameterizedTest
@NullSource
void sum_null_empty_test(String input) {
    int inputResult = Calculator.sum(input);
    assertThat(inputResult).isZero();
}
```

#### @MethodSource

좀 더 다양한 매개변수를 받아서 Test를 진행할 때 사용하기 좋다.

``` java
@ParameterizedTest(name = "{index}: 예외: {1}")
@MethodSource("provideCarNameAndErrorMessage")
void constructor_exception_test(String carName, String errorMessage) {
    assertThatThrownBy(() -> new CarName(carName))
        .hasMessageContaining(errorMessage);
}

private static Stream<Arguments> provideCarNameAndErrorMessage() {
    return Stream.of(
        Arguments.of("", ErrorMessage.CAR_NAME_EMPTY.getMessage()),
        Arguments.of("abcdef", ErrorMessage.CAR_NAME_TOO_LONG.getMessage())
    );
}
```

### 👨‍💻 해결 방법

@ParameterizedTest
[--> 관련 Commit](https://github.com/woowacourse/java-racingcar/pull/327/commits/5534b28146329d84a00cff52b1df5dd792dc9d0f)

@MethodSource
[--> 관련 Commit](https://github.com/woowacourse/java-racingcar/pull/393/commits/fc08b1165af7bd1c8b382680162a9670976fc802)
