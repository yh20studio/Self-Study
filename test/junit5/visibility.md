# visibility

## ❓관련 문제

- [자동차 경주 미션 junit5 피드백](https://github.com/woowacourse/java-racingcar/pull/393#r809927135)

<br>

## 📝 공부 내용


Class and method visibility

Test classes, test methods, and lifecycle methods are not required to be public, but they must not be private.

It is generally recommended to omit the public modifier for test classes, test methods, and lifecycle methods unless there is a technical reason for doing so – for example, when a test class is extended by a test class in another package. Another technical reason for making classes and methods public is to simplify testing on the module path when using the Java Module System.


테스트 클래스, 테스트 메서드, 라이프사이클 메서드는 접근제어자를 꼭 public으로 선언 안해줘도 된다.
단, private으로는 선언하면 안된다. 추천되는 방법은 publicd을 생략하고 default로 사용하는 것이다

<br>

## 👨‍💻 해결 방법

- [수정 Commit](https://github.com/woowacourse/java-racingcar/pull/393/commits/4d758cf878f81541d4b9b35ac830ad6ea7a615d9)

<br>

## 📎 참고

- [JUnit 5 User Guide](https://junit.org/junit5/docs/current/user-guide/)