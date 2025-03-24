# 중첩 클래스

태그: Java

중첩 클래스의 종류에는 크게는 2가지, 작게는 4가지로 나눌 수 있음

- 정적 중첩 클래스
- 내부 클래스
    - 내부 클래스
    - 지역 클래스
    - 익명 클래스

중첩 클래스의 종류는 선언 위치에 따라 달라짐

- 정적 중첩 클래스는 정적 변수와 같은 위치에 static으로 선언
- 내부 클래스는 인스턴스 변수와 같은 위치
- 지역 클래스는 지역 변수와 같은 위치

정적 중첩 클래스와 내부 클래스의 차이는 바깥 클래스를 구성하고 있는 요소인지 아닌지에 차이가 있다.

좀 더 쉽게 설명하면 정적 중첩 클래스는 위치상으로는 바깥 클래스 안에 있지만 바깥 클래스와는 관계가 없음

이와 다르게 내부 클래스는 바깥 클래스의 내부에 있으면서 바깥 클래스를 구성하는 요소를 의미한다.

### 중첩 클래스의 사용

- 특정 클래스가 다른 하나의 클래스에서 사용되는 상황에서 사용 → 논리적 그룹화
- 둘 클래스 간 긴말한 연결요소가 있는 경우에 사용
- 여러 클래스가 중첩 클래스를 사용한다면 중첩 클래스로 만들면 안된다.

### 정적 중첩 클래스

- static이 붙음
- 바깥 클래스의 인스턴스에 소속되지 않음
- 정적 중첩 클래스는 바깥 클래스의 클래스 멤버에는 접근할 수 있지만 인스턴스 멤버에는 접근할 수 없음
- 클래스 멤버는 static 키워드가 붙은 클래스 안의 정적 변수로서 해당 클래스가 로드될때 초기화가 되므로 정적 중첩 클래스에서 사용이 가능하다.
- 인스턴스 멤버는 static 키워드가 붙지 않은 클래스 안의 정적 변수로서 해당 클래스가 생성이 되어야만 인스턴스 멤버가 초기화된다. 따라서 정적 충첩 클래스에서 사용할 수 없는 것임

### 내부 클래스

- 내부 클래스는 바깥 클래스의 인스턴스를 이루는 요소가 된다.
- 즉, 내부 클래스는 바깥 클래스의 인스턴스에 소속된다.
- 이로 인해 정적 중첩 클래스는 바깥 클래스의 인스턴스 멤버에 접근할 수 없었지만 내부 클래스에서는 바깥 클래스의 인스턴스 멤버에 접근할 수 있음
- 바깥 클래스가 생성되서 초기화 될때 내부 클래스도 생성이 되어 초기화가 되기때문에 어떤 인스턴스에서 생성된 내부 클래스인지 명확하게 결정이 되기 때문에 인스턴스 멤버에 접근할 수 있다.

### 지역 클래스

- 지역 클래스는 내부 클래스의 한 종류임
- 그렇기에 바깥 클래스의 인스턴스 멤버에 접근할 수 있음
- 지역 클래스는 지역 변수처럼 코드 블럭 안에서 정의됨

```java
class Outer {
		public void process() {
		
				int localVar = 0; // 지역 변수
				
				class Local {} // 지역 클래스
				
				Local local = new Local();
		}
}
```

- 위의 형식으로 보이듯 지역 변수 선언하는 것 처럼 지역 클래스를 선언해서 사용한다.
- 당연하겠지만 지역 클래스는 지역 변수에 접근 가능함

### 지역 클래스 지역 변수 캡처

- 지역 클래스에서 중요한 개념은 지역 변수 캡처가 있음
- 클래스 변수는 프로그램 종료까지 존재하고, 인스턴스 변수는 인스턴스가 존재하는 기간까지 존재하고 지역변수는 메서드 호출이 끝나면 사라진다.
- 이 개념을 토대로 밑의 예시 코드를 보면

```java
package nested.local;

import java.lang.reflect.Field;

public class LocalOuterV3 {
    private int outInstanceVar = 3;

    public Printer process(int paramVar){

        int localVar = 1; // 지역 변수는 스택영역이 종료되는 순간 함께 제거된다.

        class LocalPrinter implements Printer{
            int value = 0;

            @Override
            public void print(){
                System.out.println(value);

                // 인스턴스는 지역변수보다 더 오래 살아 남는다.
                System.out.println(localVar);
                System.out.println(paramVar);
                System.out.println(outInstanceVar);
            }
        }

        LocalPrinter printer = new LocalPrinter();
        return printer;
    }

    public static void main(String[] args) {
        LocalOuterV3 localOuterV1 = new LocalOuterV3();
        Printer printer = localOuterV1.process(2);
        printer.print();
    }
}

```

- 여기서 보면 process()는 Printer의 타입을 반환하는데
- LocalPrinter의 print 메서드를 process()안에서 실행하는 것이 아닌 process() 메서드가 종료되고 main()메서드에서 실행이 됨
- 즉, paramVar와 localVar는 매개변수와 지역변수로서 process() 메서드에서 호출과 함께 사라졌어야하는데 코드 돌려보면 잘 동작하는 것이 확인됨
- 이건 지역변수의 캡처로 인해 가능한 것이다.
- 지역 클래스의 인스턴스를 생성하는 시점에 필요한 지역 변수를 복사해서 생성한 인스턴에 함께 넣어두는데 이 과정이 변수 캡처라고 함
- 이 변수 캡처로 인해서 지역 클래스에 있는 지역변수와 매개변수는 도중에 값을 바꿀 수 없음
- 이게 사실상 final 이라고 한다.
- 캡처 변수의 값을 바꾸게 되면 캡처한 변수의 값도 바꿔야하고 예상치 못한 문제가 발생할 수 있기 때문에 자바에서 final로 선언한 것 처럼 컴파일 오류를 발생하게 한다.

### 익명 클래스

- 이름이 없는 지역 클래스를 의미함
- 상위 타입을 상속 또는 구현하면서 바로 생성함
- 주로 특정 상위 타입을 간단히 구현해서 일회성으로 사용할 때 유용하다.
- 익명 클래스를 통해 지역 클래스의 선언과 생성을 한번에 할 수 있음

```java
package nested.anonymous;

import nested.local.Printer;

public class AnonymousOuter {
    private int outInstanceVar = 3;

    public void process(int paramVar){

        int localVar = 1;

        Printer printer = new Printer(){
            // 인터페이스를 생성하는 것이 아니고,
            // Printer 인터페이스를 구현하는 익명 클래스를 생성하는 것임
            int value = 0;

            @Override
            public void print(){
                System.out.println(value);
                System.out.println(localVar);
                System.out.println(paramVar);
                System.out.println(outInstanceVar);
            }
        };

        printer.print();
        System.out.println(printer.getClass());
    }

    public static void main(String[] args) {
        AnonymousOuter outer = new AnonymousOuter();
        outer.process(2);
    }
}

```

- 마치 인터페이스를 생성하는 것 처럼 보이지만 인터페이스를 구현한 익명 클래스를 생성하는 것임
- 익명 클래스의 특징은 이름을 가지지 않기 때문에 생성자를 가질 수 없고 기본 생성자만 사용된다.
- 익명 클래스는 단 한번만 인스턴스를 생성할 수 있음. 여러번 생성이 필요한 경우에는 지역 클래스로