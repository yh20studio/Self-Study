## hasMessageContaining()

<br>

### ❓관련 문제

- [자동차 경주 미션 hasMessageContaining() 피드백](https://github.com/woowacourse/java-racingcar/pull/327#discussion_r805305535)
- [로또 미션 hasMessageContaining() 피드백](https://github.com/woowacourse/java-lotto/pull/364#r813878330)

<br>

### 📝 공부 내용

#### AssertJ 예시 코드

AssertJ 3.X와 Java 8 이상의 환경에서 Exception 테스트를 할 때 가이드는 다음과 같다.

``` java
// 이 두가지 방법중에서 더 직관적인 이 방법을 사용하기로 한다.
@Test
public void testException() {
   assertThatThrownBy(() -> { throw new Exception("boom!"); })
   .isInstanceOf(Exception.class)
   .hasMessageContaining("boom");
} 

@Test
public void testException() {
   assertThatExceptionOfType(IOException.class).isThrownBy(() -> { throw new IOException("boom!"); })
       .withMessage("%s!", "boom")
       .withMessageContaining("boom")
       .withNoCause();
}
```

여기서 알 수 있는 점은 `isInstanceOf()` 와 `hasMessageContaining()`의 사용이다.

#### isInstanceOf()
해당 코드를 통해서 기대되는 Exception의 Class를 확인할 수 있는 코드이다. 현재 위의 코드에서는 Exception.class으로 선언되어 있지만
좀 더 자세한 검증을 위해서는 다양한 Exception 구현체를 사용할 수 있다.

#### hasMessageContaining()
단순히 Exception의 종류만 판단하고 Test를 종료할 수 있지만, 그렇다면 정말 의도한 부분에서 Exception이 발생했는지
혹은 다른 부분에서 같은 종류의 Exception이 발생한 것인지는 판단할 수 없다. 
무엇이 되던 Exception이 일어났기에 Test는 통과하게 되니..

따라서 hasMessageContaining()으로 정확히 내가 의도한 Exception이 발생하는지 판단할 수 있는 것이다.
특별한 상황이 아니라면 hasMessageContaining()까지 사용해서 Exception에 대한 Test를 명확히 하자.

추가적으로 isInstanceOf()을 먼저 사용하고 hasMessageContaining()을 그 다음에 검증하는 것을 잊지말자.

<br>

### 👨‍💻 해결 방법

- [자동차 경주 미션 hasMessageContaining() 수정 Commit](https://github.com/woowacourse/java-racingcar/pull/327/commits/d5e1cc8b4e604d44445e00bf0c4b4ef715c44feb)
- [로또 미션 hasMessageContaining() 수정 Commit](https://github.com/woowacourse/java-lotto/pull/364/commits/3b5d6042f5dc93f5ef1e7d84b58b726f2932190b)

<br>

### 📎 참고

[AssertJ Core website](https://joel-costigliola.github.io/assertj/assertj-core-features-highlight.html)