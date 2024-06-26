# Optional

- Java 8에서 소개 
- Null을 처리하기 위해서 새로운 방법을 제공하는 클래스 
- Reference type은 모두 Null을 가질 수 있지만 이를 좀 더 명시적으로 다루기 위해 Optional을 사용

<br> 

## Optional 내부 구조 

- Reference type을 감싸고 있는 Wrapper 클래스
- Primitive type은 사용할 수 없음
- Optional Class 내부의 value 필드에 Reference type을 저장

```java
@jdk.internal.ValueBased
public final class Optional<T> {
    /**
     * If non-null, the value; if null, indicates no value is present
     */
    private final T value;
}
```

- value를 가져오기 위해서는 **.get()** 메서드를 사용

```java
public T get() {
    if (value == null) {
        throw new NoSuchElementException("No value present");
    }
    return value;
}
```

<br>
<br>

## Optional 메서드의 사용 방법

- 빈 Optional 객체 생성

```java 
Optional emptyOptional = Optional.empty();
```

- Reference type으로부터 Optional 객체 생성

```java
String name = "Hello";
Optional<String> opt = Optional.of(name);
```

<br>
<br>

## Optional을 확인하는 메서드 

### isPresent() 

Optional 객체가 비어있지 않은 경우에만 true를 반환  

- 내부 구현

```java
public boolean isPresent() {
    return value != null;
}
```

- 사용 예시

```java
Optional<String> userName = Optional.ofNullable(null);
if(!userName.isPresent()) {
  throw new IllegalArgumentException("Username is null");
}
userName.get(); // NoSuchElementException 발생하지 않음
```

<br>

### isEmpty()

Optional 객체가 비어있는 경우에만 true를 반환

- 내부 구현

```java
public boolean isEmpty() {
    return value == null;
}
```

- 사용 예시 

```java
Date data = cacheRepository.findById(1L);
if(chache.isEmpty()) {
  data = jpaRepository.findById(1L);
}
```

<br>

### orElse(T other)

Optional 객체가 비어있는 경우에 대체할 값을 지정할 수 있음

- 내부 구현 

```java
Optional<String> userName = Optional.ofNullable(null);
String name = userName.orElse("홍길동");
```

- 사용 예시 

```java
User convertToEntity(UserRequest request) {
  User user = new User();
  user.setName(request.getName().orElse("홍길동"));
  user.setAge(request.getAge().orElse(20));
  return user;
}
```

<br>

### orElseThrow(Supplier<? extends X> exceptionSupplier)

Optional 객체가 비어있는 경우에 예외를 발생시킬 수 있음

- 내부 구현 

```java
public T orElseThrow() {
    if (value == null) {
        throw new NoSuchElementException("No value present");
    }
    return value;
}
```

- 사용 예시 

```java
User user = userRepository.findById(1L)
  .orElseThrow(
    () -> new ApiException("User not found")
);
```

<br>






