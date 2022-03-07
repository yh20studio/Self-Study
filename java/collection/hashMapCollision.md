# HashMap은 해시 충돌이 자주 발생한다면 시간 복잡도가 O(logN)에 근접하게 된다?

## ❓관련 문제

[이펙티브 자바 스터디 아이템 37: ordinal 인덱싱 대신 EnumMap을 사용하라](https://github.com/woowacourse-study/2022-effective-java/pull/20)

Key의 Hash값을 계산해서 테이블의 인덱스로 사용하기에
노드의 Key가 일치할때까지 연결리스트 또는 트리를 탐색한다.
따라서 HashMap은 해시 충돌이 자주 발생한다면 시간 복잡도가 O(logN)에 근접하게 된다.

위 발표 자료에서 Collision이 발생 시 시간 복잡도가 왜 O(logN)인지 궁금합니다!

<br>

## 📝 공부 내용

### 우선 **HashMap의 작동 원리**부터 설명해 드리겠습니다.

HashMap은 기본적으로 각 객체의 hashCode() 메서드가 반환하는 값을 사용하며 결과 값의 자료형은 int 입니다. 하지만 32비트 정수 자료형으로는 완전한 자료 해시 함수를 만들 수 없습니다. 논리적으로 생성 가능한 객체의 수가 2^32보다 많을 수 있기 때문이며, 또한 모든 HashMap 객체에서 접근의 시간복잡도가 O(1)임을 보장하기 위해 랜덤 접근이 가능하게 하려면 원소가 2^32인 배열을 모든 HashMap이 가지고 있어야 하기 때문에 어렵습니다.

따라서 HashMap 에서는 메모리를 절약하기 위하여 실제 해시 함수의 표현 정수 범위보다 작은 M개의 원소가 있는 배열만을 사용합니다.

``` java
int index = X.hashCode() % M; 
```

이런 방식으로 2^32개의 인덱스가 아닌 M개의 인덱스를 사용하게 됩니다. 따라서 원래는 달라야할 해시 코드이지만 인덱스를 M개로 줄이면서 1/M의 확률로 같은 해시 인덱스를 사용할 수도 있게 됩니다.

이때 같은 인덱스를 가지는 해시 충돌이 발생하게 됩니다.
Java HashMap에서는 해시 충돌이 나는 데이터들을 구분하기 위해서 Separate Channing을 사용하여 이들을 관리 및 구분하고 있습니다.

### 그렇다면 Separate Channing은 어떻게 구현 되어 있을까?

Java HashMap은 그동안 JDK를 업그레이드 하면서 HashMap의 성능을 높이기 위해서 변화를 거쳐왔습니다. 이때 Java 8에서 중요한 변화가 일어났습니다. Java HashMap에 탐색에 대한 성능을 높이기 위해서 기존에 Java 2부터 Java 7까지 사용하던 인덱스가 겹치는 객체들을 구분하기 위해서 링크드 리스트를 사용했었는데, Java 8부터는 데이터의 개수가 많아지면 링크드 리스트 대신 트리를 사용하도록 변경되었습니다.

### 그렇다면 log(N) 값이 왜 나오는 것일까? 트리?

위에서 언급했던 트리가 성능을 높이는 역할을 하게되는데 이때 사용하는 트리는 Red-Black Tree입니다. Red-Black Tree는 Java Collections Framework의 TreeMap과 구현이 거의 같고 트리 순회 시 사용하는 대소 판단 기준은 원래 가지고 있던 고유의 해시 함수 값입니다.


### Red-Black Tree

Red-Black Tree는 이진탐색트리 중에서도 balanced binary search tree 형태를 가지고 있습니다.

Red-Black Tree가 balanced binary search tree 형태를 가질 수 있게 되는 원인은
Red-Black Tree는 다음과 같은 조건을 만족해야 하기 때문입니다.

- Root Property : 루트노드의 색깔은 검정(Black)이다.
- External Property : 모든 external node들은 검정(Black)이다.
- Internal Property : 빨강(Red)노드의 자식은 검정(Black)이다.
- Depth Property : 모든 리프노드에서 Black Depth는 같다.

이러한 조건을 갖추면서 트리를 생성 및 업데이트 하기 때문에 Red-Black Tree의 트리 높이는 log(n)으로 형성 될 수 있습니다.
따라서 Red-Black Tree에서 값을 찾는 연산은 트리의 높이가 최대 log(n)이기 때문에 O(logn)의 시간 복잡도를 가지게 됩니다.

[Red-Black Tree의 좀 더 자세한 설명](https://zeddios.tistory.com/237)

### 결론

HashMap에서 해시 인덱스가 겹치는 데이터가 8개 보다 많아지면 링크드 리스트 대신 트리를 이용하도록 구현되어 있다고 합니다. 따라서 데이터가 많아지고 해시 인덱스가 충돌하는 데이터가 많아지게 된다면 HashMap의 get 메서드의 시간 복잡도가 O(logn)으로 생각할 수 있게 됩니다.

<br>

## 📎 참고

[Java HashMap은 어떻게 동작하는가?](https://d2.naver.com/helloworld/831311)