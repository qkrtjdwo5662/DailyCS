# private 메서드에 @Transactional 선언하면 트랜잭션이 동작할까?

태그: Jpa

### 결론

- @Transactional은 private 메서드에서는 동작하지 않는다.

### 이유

- 스프링에서 @Transactional은 AOP 기반으로 동작한다.
- 좀 더 자세하게 설명하면 프록시 객체를 만들어서 메서드 호출을 가로채는 방식으로 트랜잭션을 처리함
- 여기서 조건이 몇개 있음
    - 스프링의 프록시 방식은 외부에서 호출되는 public 메서드만 가로챌 수 있다.
    - 자기 자신의 내부 호출의 경우 프록시가 끼어들 수 없어 트랜잭션이 적용되지 않는다.
    - private, protected, default 메서드의 경우 AOP 적용 대상이 아니기에 @Transactional이 무시된다.
    - 하지만 좀 더 정확하게 얘기하면 JDK Dynamic Proxy의 경우에 해당하는 것임
    - CGLIB 방식 경우 인터페이스를 구현하지 않는 클래스를 상속하여 프록시를 생성하고, private를 제외한 public, protected, default 메서드에 AOP 적용 가능
- 스프링은 대부분 인터페이스 기반으로 서비스 계층을 설계하기 때문에 AOP도 JDK Dynamic Proxy 방식으로 적용된다고 볼 수 있음

### 예시

```java
@Service
public class UserService {

    @Transactional
    private void saveUserPrivate(User user) {
        userRepository.save(user);
    }

    public void savePublic(User user) {
        saveUserPrivate(user);
    }
}
```

- 위의 두 메서드는 @Transactional이 적용되지 않는다.
- saveUserPrivate의 경우 private로 선언된 메서드이기에 동작하지 않음
- savePublic의 경우 @Transactional을 선언하지 않음

```java
@Service
public class UserService {

    @Transactional
    public void saveUser(User user) {
        userRepository.save(user);
    }
}
```

- 이렇게 코드를 수정해야 @Transactional이 적용된다.