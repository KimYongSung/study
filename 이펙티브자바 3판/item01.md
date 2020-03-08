# 아이템1. 생성자 대신 정적 팩터리 메서드를 고려하라

정적 팩터리 메서드는 프로그래머가 꼭 알아둬야할 기법으로 평상시 자주 사용하고 있습니다.

```java
// Integer 클래스
public static Integer valueOf(int i) {
    if (i >= IntegerCache.low && i <= IntegerCache.high)
        return IntegerCache.cache[i + (-IntegerCache.low)];
    return new Integer(i);
}

// boolean 클래스
public static Boolean valueOf(boolean b) {
    return (b ? TRUE : FALSE);
}

```

정적 팩터리 메서드는 아래와 같은 장점이 있습니다.

1. 이름을 가질 수 있다.
2. 호출될 때마다 인스턴스를 새로 생성하지는 않아도 된다.
3. 반환 타입의 하위 타입 객체를 반환할 수 있는 능력이 있다.
4. 입력 매개변수에 따라 매번 다른 클래스의 객체를 반환할 수 있다.
5. 정적 팩터리 메서드를 작성하는 시점에는 반환할 객체의 클래스가 존재하지 않아도 된다.

위의 장점을 SOLID 관점에서 본다면, ``단일 책임 원칙(SRP) , 개방 폐쇠 원칙 (OCP), 리스코프 치환 원칙 (LSP), 의존관계 역전 원칙(DIP)`` 가 적절히 혼합된 방식이라고 생각합니다.

## 이름을 가질 수 있다

개발자가 특정 클래스를 생성시에 생성자 내부의 소스코드나 클래스의 맴버변수를 직접 확인해야 해당 생성자가 가지는 의미를 이해할 수 있습니다.

* 하나의 메소드는 ``이름에 근거한 하나의 책임을 가지며 이를 단일책임원칙(OCP)``으로 볼 수 있다고 생각합니다.

> 책에서 설명하는 BigInteger.probablePrime와 Spring의 ResponseEntity 에서 ok 메소드

```java
public static BigInteger probablePrime(int bitLength, Random rnd) {
    if (bitLength < 2) {
        throw new ArithmeticException("bitLength < 2");
    } else {
        return bitLength < 95 ? smallPrime(bitLength, 100, rnd) : largePrime(bitLength, 100, rnd);
    }
}

public static <T> ResponseEntity<T> ok(T body) {
    BodyBuilder builder = ok();
    return builder.body(body);
}
```

아래 클래스는 String type의 code와 message를 멤버변수로 가지고 있는 Response객체로 ErrorCode 과 Stirng 타입의 생성자를 각각 가지고 있습니다.

여기서 error, validationError, success 이름을 가지는 정정 팩터리 메서드들은 이름으로 의도를 파악할 수 있습니다.

```java
public class Response {

    private String code;

    private String message;

    protected Response(ErrorCode errorCode) {
        this.code = errorCode.getCode();
        this.message = errorCode.getMessage();
    }

    private Response(String validMsg) {
        this.code = ErrorCode.CD_S001.getCode();
        this.message = validMsg;
    }

    /**
     * API 실패 응답 생성
     * @param errorCode 결과코드
     * @return
     */
    public static Response error(ErrorCode errorCode){
        return new Response(errorCode);
    }

    /**
     * 파라미터 유효성 실패 에러
     *
     * @param result
     * @return
     */
    public static Response validationError(BindingResult result) {
        return Objects.isNull(result) ? new Response(ErrorCode.CD_S001) : new Response(result.getFieldError().getDefaultMessage());
    }
    /**
     * API 성공 응답 생성
     * @return
     */
    public static Response success(){
        return new Response(ErrorCode.CD_0000);
    }
}
```

## 호출될 때마다 인스턴스를 새로 생성하지는 않아도 된다

불변 클래스는 인스턴스를 미리 만들어 놓거나, 캐싱하여 재활용하는 식으로 불필요한 객체 생성을 피할 수 있다.

```java
// Integer.java

public static Integer valueOf(int i) {
    if (i >= IntegerCache.low && i <= IntegerCache.high)
        return IntegerCache.cache[i + (-IntegerCache.low)];
    return new Integer(i);
}
```

```java
// Boolean.java

public static final Boolean TRUE = new Boolean(true);

public static final Boolean FALSE = new Boolean(false);

...

public static Boolean valueOf(boolean b) {
    return (b ? TRUE : FALSE);
}
```

* 같은 객체가 자주 사용되는 상황이라면 성능을 상당히 끌어올려 준다.
* 반복되는 요청에 같은 객체를 반환하는 식으로 정적 팩터리 방식의 클래스는 언제 어느 인스턴스를 살아 있게 할지를 철저히 통제할 수 있다.
  * 인스턴스 통제 클래스라고 함.
  * 싱글턴으로 객체를 생성 가능.
  * 인스턴스화 불가 클래스로 생성 가능.
    * 생성자가 protected, privete 으로 외부에 공개되지 않는다.
  * 불변값 클래스에서 동치인 인스터스가 단 하나뿐임을 보장가능.

## 반환 타입의 하위 타입 객체를 반활할 수 있는 능력이 있다

객체의 클래스를 자유롭게 선택할 수 있게 하는 엄청난 유연성을 선물한다.

* java.util.Collections를 통하여 Collection 프레임워크 기능들을 핸들링 가능하다.
* 클라이언트는 인터페이스만으로 객체를 다루게 된다.
  * 클라이언트는 실제 구현체의 정보를 알 수 없기 때문에 구현체가 변경되어도 실제 인터페이스를 활용하는 로직은 변경되지 않는다. 이때 ``리스코프 치환 법칙(LSP)``에 성립된다고 볼 수 있다고 생각합니다.

> collections의 unmodifiableList 메소드를 통하여 수정이 불가능한 list 생성 기능 및 여러 기능들을 제공함.

```java
public static <T> List<T> unmodifiableList(List<? extends T> list) {
    return (list instanceof RandomAccess ?
            new UnmodifiableRandomAccessList<>(list) :
            new UnmodifiableList<>(list));
}
```

## 입력 매개변수에 따라 매번 다른 클래스의 객체를 반환할 수 있다

* [팩토리 메서드 패턴](https://ko.wikipedia.org/wiki/%ED%8C%A9%ED%86%A0%EB%A6%AC_%EB%A9%94%EC%84%9C%EB%93%9C_%ED%8C%A8%ED%84%B4)으로 생각해도 되지 않을까?? 생각함.
* 클라이언트는 인터페이스만을 활용하며 실제 구현체 정보는 알 수 없다.
  * 특정 파라미터에 맞는 구현체를 변경하여도 클라이언트는 그 정보를 몰라도 상관없다.
* 실제 객체의 생성을 정적 팩터리 메서드가 관리하므로 의존관계 역전 원칙 (DIP)이 성립된다고 생각합니다.

## 정적 팩터리 메서드를 작성하는 시점에는 반환할 객체의 클래스가 존재하지 않아도 된다

* 서비스 제공자 프레임워크를 만드는 근간이 된다.
  * 대표적으로는 JDBC가 존재함.

# 단점

단점으로는 아래 두가지가 존재합니다.

1. 상속을 하려면 public이나 protected 생성자가 필요하니 정적 팩터리 메서드만 제공하면 하위 클래스를 만들 수 없다.

2. 정적 팩터리 메서드는 프로그래머가 찾기 어렵다.

## 상속을 하려면 public이나 proected 생성자가 필요하니 정적 팩터리 메서드만 제공하면 하위 클래스를 만들 수 없다

* 컬렉션 프레임워크의 유틸리티 구현 클래스들을 상속할 수 없다는 이야기로, 상속보다는 컴포지션을 사용하도록 유도하고 불변타입으로 만들려면 이 제약을 지켜야 한다는 점에서 장점으로 받아들일 수도 있다.

## 정적 팩터리 메서드는 프로그래머가 찾기 어렵다

* 메서드 이름을 널리 알려진 규약을 따라 짓는 식으로 문제를 완화해줘야 한다.

### 메서드 이름 규약

* from : 매개변수를 하나 받아서 해당 타입의 인스턴스를 반환하는 형변환 메서드

```java
Date d = Date.from(instant);
```

* of : 여러 매개변수를 받아 적합한 타입의 인스턴스를 반환하는 집계 메서드

```java
Set<Rank> faceCards = EnumSet.of(JACK, QUEEN, KING);
```

* valueOf : from과 of의 더 자세한 버전

```java
BigInteger prime = BigInteger.valueOf(Integer.MAX_VALUE);
```

* instance 혹은 getInstance : 매개변수로 명시한 인스턴스를 반환하지만, 같은 인스턴스임을 보장하지는 않는다.

```java
StackWalker luke = StackWalker.getInstance(options);
```

* create 혹은 newInstance : instance 혹은 getInstance와 같지만, 매번 새로운 인스턴스를 생성해 반환함을 보장한다.

```java
Object newArray = Array.newInstance(classObject, arrayLen);
```

* getType : getInstance와 같으나, 생성할 클래스가 아닌 다른 클래스에 팩터리 메서드를 정의할 때 쓴다. "Type"은 팩터리 메서드가 반환할 객체 타입이다.

```java
BufferedReader br = Files.newBufferedReader(path);
```

* type : getType과 newType의 간결한 버전

```java
List<Complaint> litany = Collections.list(legacyLitany);
```
