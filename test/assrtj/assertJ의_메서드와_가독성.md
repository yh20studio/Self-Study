# assertJ의 메서드와 가독성

## ❓관련 문제

- [자동차 경주 미션 assertj 가독성 피드백1](https://github.com/woowacourse/java-racingcar/pull/393#r809930638)
- [자동차 경주 미션 assertj 가독성 피드백2](https://github.com/woowacourse/java-racingcar/pull/393#r809933362)
- [자동차 경주 미션 assertj 가독성 피드백3](https://github.com/woowacourse/java-racingcar/pull/393#r809935061)

<br>

## 📝 공부 내용

항상 검증과정에서 isEqualTo()만 사용해왔지만, 좀 더 가독성을 높일 수 있는 assrtj의 메서드들이 있기에 
찾아 보았다.

다음은 모두 테스트를 통과하는 코드를 나타냈다.

``` java
@Test
void assertJ_Test() {
    // 슷자 0 테스트
    assertThat(0)
            .isEqualTo(0)
            .isZero()
            .isEven();
    // 슷자 1 테스트
    assertThat(1)
            .isGreaterThan(0)
            .isGreaterThanOrEqualTo(1)
            .isPositive()
            .isNotNegative()
            .isNotZero()
            .isOdd()
            .isOne();
    // 슷자 -1 테스트
    assertThat(-1)
            .isLessThan(0)
            .isLessThanOrEqualTo(-1)
            .isNegative()
            .isNotPositive()
            .isNotZero()
            .isBetween(-2, 0);
    // 문자열 테스트
    assertThat("Hello world") 
            .isNotEmpty() // 비어 않은 있는 경우
            .contains("Hello") // 포함 하는 경우
            .doesNotContain("Hi") // 포함 하지 않는 경우
            .startsWith("Hel") // 해당 문자열로 시작
            .endsWith("ld")// 해당 문자열로 끝난다
            .isEqualTo("Hello world");
}
```
<br>

핵심은 같은 결과를 나타내지만 표현법에 있어서 정말 다양하다는 뜻이다. 이 테스트들 모두 그냥 
`isEqualTo()`를 사용한다면 손 쉽게 작성할 수 있지만 테스트 코드를 읽는 사람 입장에서 모두 다
다르게 느껴질 수 있는 코드라고 생각한다. 테스트 코드를 만들 때 의도하는 의미가 있다면 최대한
의미가 담길 수 있도록 메서드를 선택해서 작성할 수 있어야 하지 않을까..?

추가적으로 특별히 의도하거나 의미부여할 것이 없다면 간단하게 표현할 수 있도록 하자. 예를 들면
`isEqualTo(0)` 보단 `isZero()`가 훨씬 읽기 편할 것이다.

<br>

## 👨‍💻 해결 방법

- [수정 Commit](https://github.com/woowacourse/java-racingcar/pull/393/commits/e67ecba07676a4e698310cf8e7902a7ccf00ef8f)

<br>

## 📎 참고

- [AssertJ](https://assertj.github.io/doc/)
