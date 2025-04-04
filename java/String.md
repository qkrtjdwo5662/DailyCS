# String 클래스

태그: Java

### String class

문자를 다루는 대표적 타입에는 char, String 2가지가 있음

char[]을 통해서 여러 문자를 나열할 수 있지만 이 방식이 불편해서 좀 더 편리한 방법의 String 클래스가 생김

```java
String s1 = "Hello";
String s2 = new String("Hello");
```

- String 클래스를 사용해서 문자열 생성 방법은 이렇게 두가지가 있음

String 클래스를 사용하기 때문에 char[], byte[] 등의 배열은 감추고 charAt, indexOf등 문자열을 편리하게 사용할 수 있는 메서드를 사용가능함

String 클래스는 객체다

객체이기 때문에 참조 값을 갖게 됨

하지만 String 변수는 +로 문자열을 합칠수 있음

이는 편의상 +연산을 java에서 제공해주는 것임

String에서 비교하는 것을 보게 되면

new String()을 통해 새로운 객체를 만드는 경우 == 비교하면 참조값을 비교하기 때문에 같은 문자열을 갖는다고 하더라도 false를 반환한다. 따라서 문자열을 비교할때는 String 클래스 내에서 오버라이드 되어있는 equals 메서드를 활용해서 비교하면 됨

하지만,

String s1 = “123”, String s2 = “123”

이와 같은 경우는 두개의 문자열을 ==으로 비교하면 true를 반환한다.

이는 자바에서 메모리 효율성과 성능 최적화를 위해 문자열 풀을 사용하기 때문임

문자열 풀은 힙영역을 사용하며 자바가 실행되는 시점에 해당 클래스에 문자열 리터럴이 있으면 문자열 풀에 String 인스턴스를 미리 만들어 놓음 이때, 같은 문자열이 있으면 만들지 않음

문자열에서 기존 문자가 있는지 여부는 해시를 사용하기 때문에 O(1)만에 매우 빠르게 찾을 수 있음

문자열 풀이 있지만 문자열 비교는 equals를 통해서 비교하는 것이 맞음

### String은 불변객체

String은 불변객체로 생성 후 내부 문자열의 값을 변경할 수는 없다.

따라서 String클래스의 메서드를 사용하면 반환값이 모두 String임을 알 수 있는데,

불변객체이기 때문에 새로운 값을 할당한 객체를 생성해서 반환해줌

왜 불변객체일까?

- 불변객체가 있는 이유는 사이드 이펙트 문제이기 때문임
- 아까 언급했던 문자열 풀에 있는 String 인스턴스를 참조하는 다른 문자열들이 참조하고 있는 문자열이 변경되는 경우 참조하는 모든 문자열의 값이 바뀌는 문제가 발생함

String은 불변객체이기에 문자열 더하기 연산을 수행하는 것 등을 지양하는 것임

근데 사실 정확하게 얘기하면 가변객체인 StringBuilder객체를 생성해서 append 합치고 마지막에 toString으로 불변객체를 바꾸긴함

→ 즉, 자바 버전에 바뀜에 따라 문자열 연산에 대한 최적화도 구현이 되어있긴하다

### StringBuilder

그럼에도 불구하고 StringBuilder를 쓰는것이 유용할때가 있음

일단 앞서 말했듯 StringBuilder는 가변객체임

반복문 연산을 수행할때는 StringBuilder 객체 하나 생성해놓고 반복문에서 append 메서드 사용하면 보다 빠르게 문자열 연산을 수행할 수 있음

또한 StringBuilder는 메서드 체이닝이 구현되어있음

메서드 체이닝은 해당 메서드의 연산이 끝나고 자기 자신 객체를 리턴하여 메서드를 체이닝 형식으로 붙여서 쓸 수 있는데 StringBuilder는 이것이 구현되어있다.