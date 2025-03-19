# Enum

태그: Java

### ENUM이 생긴 이유

- 문자열의 비교로만 로직을 처리하게 되면 안정성이 부족함
- 즉, 컴파일 시점에 오류를 찾는 것이 아닌 런타임 상황에서 오류를 찾아 치명적인 시스템 문제가 발생할 수 있음
- 데이터의 일관성과(소문자, 대문자) 타입 안정성(오타)의 이유로 인해 Enum이 탄생했다

### Enum

- Enum은 클래스이다.
- 근데 열거형을 쉽게 처리할 수 있는 몇가지 제약 조건이 구현된 클래스라고 보면됨
- Enum은 다음 클래스와 유사하다고 볼 수 있음

```java
public class ClassGrade {
    public static final ClassGrade BASIC = new ClassGrade();
    public static final ClassGrade GOLD = new ClassGrade();
    public static final ClassGrade DIAMOND = new ClassGrade();

    private ClassGrade(){};
}
```

- 위와 같이 각각의 상수들에 새로운 인스턴스를 생성하여 할당한다.
- 그리고 private으로 생성자 생성을 막아 인스턴스 참조값을 비교해도 문제가 없도록 구현되어있음

```java
public enum Grade {
    BASIC, GOLD, DIAMOND
}

```

- 위의 클래스와 밑의 enum은 거의 유사한 구조라고 보면된다.
- 비교해보면 알겠지만 enum 타입으로 매우 편하게 열거형 타입을 사용할 수 있음을 알 수 있음

### Enum의 장점

- 타입의 안정성 향상 : 열거형은 사전에 정의된 상수들로만 구성됨. 따라서 유효하지 않은 값이 입력될 가능성이 없어 사전에 오류를 방지한다.
- 간결성 및 일관성 : 열거형을 사용하면 코드가 매우 간결해지고 데이터의 일관성이 보장된다.
- 확장성 : 새로운 타입을 추가하고 싶으면 Enum에 새로운 상수를 추가만 하면 된다.

### Enum 활용

- 앞서 말했듯 enum은 클래스이다.
- 따라서 클래스 처럼 필요에따라 로직을 구현할 수 도 있음

```java
public enum AuthGrade {
    GUEST(1, "손님"),
    LOGIN(2, "로그인 회원"),
    ADMIN(3, "관리자");

    private final int level;

    private final String description;

    AuthGrade(int level, String description){
        this.level = level;
        this.description = description;
    }

    public int getLevel(){
        return level;
    }

    public String getDescription(){
        return description;
    }

}

```

- 이 예시를 보면 알 수 있듯 enum에 필요한 필드를 추가하고 생성자를 통해 필드에 값을 저장할수 있음
- 외부에서 생성자를 통해 해당 열거형을 생성할 수 없게 기본이 private로 구현되어있음
- 또한 필요한 메서드를 구현해서 내부 로직을 캡슐화해서 감출수도 있음