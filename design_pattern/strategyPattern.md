# 인터페이스를 이용해서 TEST를 더 잘할 수 있게 만드는 방법

<br>

## ❓관련 문제

자동차의 움직임에 대한 메서드를 테스트하고 싶은데.. 자동차는 움직일 때 Random 값을 받아서 움직이고 있다.
이때 자동차의 움직임을 테스트하기 위해서는 Random값을 조절해야하는가? 어떻게 진행 해야 할까?

- [자동차 경주 미션 인터페이스 분리 피드백](https://github.com/woowacourse/java-racingcar/pull/327#discussion_r805303802)
- [자동차 경주 미션 자동차 move() 테스트 피드백](https://github.com/woowacourse/java-racingcar/pull/327#discussion_r805304533)

<br>

## 📝 공부 내용

### 전략 패턴이란?
실행 중에 알고리즘을 선택할 수 있게 하는 행위 소프트웨어 디자인 패턴이다.
즉, 컴파일 타임에서는 인터페이스를 매개변수로 받기 때문에 해당 구현체를 자세하게 모르지만
런타임에서 해당 매개변수에 들어가는 인터페이스의 구현체를 통해서 실제 작동할 알고리즘이 선택되는 패턴이다.


### 테스트 하고 싶은 메서드

``` java
public boolean move() {
    if (isMoveForward(generateRandomNumber())) {
        position.increase();
        return true;
     }
     return false;
   }
}
```
여기서 generateRandomNumber()은 항상 Random 값을 가지고 오기 때문에 Test할 때 값을 예측할 수 
없어서 Test가 불가능하다. 이에 전략패턴을 활용해서 인터페이스로 분리해 보았다.

<br>

### interface

``` java
public interface NumberGenerator {

     int generate();
}
```

### 항상 움직이는 결과 값을 주는 인터페이스 구현체

``` java

public class MovableNumberGenerator implements NumberGenerator {

     private static final int EXCLUSIVE_BOUND_CORRECTION_VALUE = 1;
     private static final int MINIMUM = 4;
     private static final int MAXIMUM = 9;

     private static final Random RANDOM = new Random();

     @Override
     public int generate() {
         int randomNumber =
                 RANDOM.nextInt(MAXIMUM - MINIMUM + EXCLUSIVE_BOUND_CORRECTION_VALUE) + MINIMUM;
         validateRange(randomNumber);
         return randomNumber;
     }

     private static void validateRange(int randomNumber) {
         if (randomNumber > MAXIMUM || randomNumber < MINIMUM) {
             throw new IllegalArgumentException(ErrorMessage.RANGE_OVER.toString());
         }
     }
}
```

### 항상 움직이지 않는 값을 주는 인터페이스 구현체

``` java
public class UnMovableNumberGenerator implements NumberGenerator {

     private static final int EXCLUSIVE_BOUND_CORRECTION_VALUE = 1;
     private static final int MINIMUM = 0;
     private static final int MAXIMUM = 3;

     private static final Random RANDOM = new Random();

     @Override
     public int generate() {
         int randomNumber =
                 RANDOM.nextInt(MAXIMUM - MINIMUM + EXCLUSIVE_BOUND_CORRECTION_VALUE) + MINIMUM;
         validateRange(randomNumber);
         return randomNumber;
     }

     private static void validateRange(int randomNumber) {
         if (randomNumber > MAXIMUM || randomNumber < MINIMUM) {
             throw new IllegalArgumentException(ErrorMessage.RANGE_OVER.toString());
         }
     }
}
```

<br>

### 변경된 코드

이렇게 인터페이스를 매개변수로 받는 방식으로 코드를 수정한다면 실제 구현되는 프로덕션 코드에서는 
랜덤 숫자를 제공하는 인터페이스 구현체인 `RandomNumberGenerator` 객체로 생성된 숫자 넘겨주면 평소와 같이 그대로
랜덤 값을 통해서 자동차가 움직이게 된다.

``` java

public boolean move(int number) {
    if (isMoveForward(number) {
        position.increase();
        return true;
     }
     return false;
   }
}
```

하지만 Test 코드를 작성할 때는 `MovableNumberGenerator`, `UnMovableNumberGenerator`로 생성된 숫자를
매개변수 값에 넣어주게 되면 항상 움직이는 결과와, 항상 움직이지 않는 결과를 예측할 수 있게 된다.
이를 이용해서 자동차의 움직임을 테스트 할 수 있게 된다.

``` java
@DisplayName("move() 움직임 테스트")
@Test
public void move_with_movableNumber_test() throws Exception {
     String name = "name1";
     CarName carName = new CarName(name);
     Position zeroPosition = new Position(0);
     Car car = new Car(carName, zeroPosition);
     NumberGenerator numberGenerator = new MovableNumberGenerator();
     Car movedCar = car.move(numberGenerator.generate());
     assertThat(movedCar.compareTo(car)).isGreaterThan(0);
}

@DisplayName("move() 정지 테스트")
@Test
public void move_with_unMovableNumber_test() throws Exception {
     String name = "name1";
     CarName carName = new CarName(name);
     Position zeroPosition = new Position(0);
     Car car = new Car(carName, zeroPosition);
     NumberGenerator numberGenerator = new UnMovableNumberGenerator();
     Car movedCar = car.move(numberGenerator.generate());
     assertThat(movedCar.compareTo(car)).isEqualTo(0);
}
```

<br>

## 👨‍💻 해결 방법

- [전략 패턴 수정 Commit](https://github.com/woowacourse/java-racingcar/pull/327/commits/a622f99dfb6514e3dcb84a98f626d44788dae729)
- [전략 패턴을 활용한 Test 수정 Commit](https://github.com/woowacourse/java-racingcar/pull/327/commits/a3e68399b61ee87392f027023d2691266db4a3f6)

<br>

## 📎 참고

[인터페이스를 분리하여 테스트하기 좋은 메서드로 만들기](https://tecoble.techcourse.co.kr/post/2020-05-17-appropriate_method_for_test_by_interface/)