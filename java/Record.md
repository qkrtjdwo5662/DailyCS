# Record 클래스

태그: Java

### Record 클래스

- Record 클래스는 자바 16버전 이상부터 사용되는 것이 권장됨
- 불변성을 기본으로 하는 객체이다.
- 기본 클래스와 달리 모든 필드가 final로 되어있음
- DTO 자체가  데이터를 전달하는 객체로 사용되는 경우 변경될 필요가 없는 경우가 많다
- 그래서 Record 클래스를 사용하면 데이터만 담겠다라는 의미전달이 명확해짐
- 그리고 getter, equals, hashCode, toString 메서드를 자동으로 생성하여 중복 코드를 줄일 수 있다.
- @RequestBody, @ResponseBody로 바로 사용할 수 있어 컨트롤러에서 DTO로 사용하기 좋다.
- 즉, 값을 생성시점에 한번 주입받고 생성자를 통해 인스턴스를 생성하고 변경이 불가능하므로 안정성이 좋다.

### Record 단점

- 하지만 단점도 존재한다.
- 불변이기에 setter가 없어 값을 변경하는 것이 불가능하다.
- 따라서 단순 데이터 전달 용으로 많이 쓰기 때문에 비즈니스 로직을 처리하는 상황에서는 일반적인 클래스 사용하는 것이 낫다.

### 예시

```java
public class MemberDto {

	private final String name;
	private final String email;
	private final int age;

	public MemberDto(String name, String email, int age) {
		this.name = name;
		this.email = email;
		this.age = age;
	}

	public String getName() {
		return name;
	}
	
	public String getEamil() { 
		return email;
	}
	
	public int getAge() {
		return age;
	}
}
```

- 기존 DTO 클래스

```java
public record MemberDto(String name, String email, int age) {}
```

- record 클래스

이렇게 보면 getter나 필드 선언 없이 매우 간단해진 것을 볼 수 있음 실제로

```java
public record MemberDto(String name, String email, int age) {

    // 메서드 추가는 가능
    public String maskedEmail() {
        return email.replaceAll("(?<=.{2}).(?=.*@)", "*");
    }

    // 필드 변경은 불가능 (컴파일 오류 발생)
    public void changeName(String newName) {
        this.name = newName; 
    }
    
}
```

- 위와 같이 메서드는 추가 가능하지만 필드 변경 메서드가 들어가면 컴파일 오류가 발생함