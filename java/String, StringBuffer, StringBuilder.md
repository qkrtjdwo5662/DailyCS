# String, StringBuffer, StringBuilder

태그: Java

### String 특징

- new 연산을 통해 생성된 인스턴스의 메모리 공간은 변하지 않는다. → 불변객체
- Garbage Collector로 제거가 되어야한다.
- 불변객체이기에 문자열 연산 시 객체를 새로이 만든다.
- 객체를 새로 만든다는 점에서 멀티 스레드에서 동기화를 신경 쓸 필요가 없어진다.
- 메모리 누수의 문제점은 있다.

### StringBuffer, StringBuilder

- new 연산으로 클래스를 한 번만 만든다.
- 문자열 연산 시 객체를 새로 만드는 것이 아닌 크기를 증가 시킨다.
- StringBuffer와 StringBuilder의 내부 메서드는 동일하다.
- 다만 차이점은 StringBuffer는 동기화가 적용된 Thread-Safe하며 StringBuilder는 Thread-Safe하지 않다.
- 따라서 StringBuffer는 문자열 연산이 많은 멀티 스레드 환경에 적합함