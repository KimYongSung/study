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

## 이름을 가질 수 있다

개발자가 특정 클래스를 생성시에 생성자 내부의 소스코드나 클래스의 맴버변수를 직접 확인해야 해당 생성자가 가지는 의미를 이해할 수 있습니다.

> 책에서 설명하는 BigInteger.probablePrime과 Spring의 ResponseEntity 에서 ok 메소드

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

여기서 error, validationError, success 이름을 가지는 정정 팩터리 메서드들은 메소드명으로 의도를 파악할 수 있습니다.

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
