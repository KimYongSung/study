# junit4 vs junit5 vs spock

## junit4

### 장점

* spring 진영에서 기본적으로 사용했었다. 
  * springboot 2.2 이전 버전은 junit4를 기본으로 사용한다.
* junit4 사용을 위해서 별도의 ide나 빌드툴 설정이 필요하지 않다.
* 오랜시간 사용해왔고, 현재도 사용 중이며 레퍼런스가 많이 있다.

### 단점

* 메소드명에 한글이 가능하지만, Spock or junit5 처럼 자유로운 형식으로 사용은 할 수 없다.
* BDD 기반 테스트 작성시 spock 처럼 직관적인 코드 작성은 개발자 개인의 노력이 필요하다.
* spock이나 junit5 와 달리 테스트 결과를 핸들링할 수 있는 추상화 포인트가 없다. ( 더 찾아볼께요.. )
* mock이 필요한 경우 mockito를 사용해야한다.

## junit5

### 장점

* spring 진영에서 기본적으로 사용한다.
  * springboot 2.2 부터 junit4 대신 junit5를 사용한다. 
* junut4 에서 많은 [기능](https://junit.org/junit5/docs/current/user-guide/#writing-tests-classes-and-methods)들이 추가되었다.
  * @DisplayName
  * 람다를 지원하는 Assert 메소드
  * 특정 환경 및 커스터마이징 가능한 테스트 [Disabling](https://junit.org/junit5/docs/current/user-guide/#writing-tests-disabling) 지원
  * 등등.. 위에 나열한 기능들 + @ 로 많은 기능들을 제공한다.
* Spock의 extension 기능과 유사한 테스트 전후로 [확장가능한 기능](https://junit.org/junit5/docs/current/user-guide/#extensions-execution-order-wrapping-behavior)이 존재한다. ( 더 확인이 필요.. )
* [병렬로 테스트 실행](https://junit.org/junit5/docs/current/user-guide/#writing-tests-parallel-execution)이 가능하여 테스트에 소요되는 시간이 감소될 것으로 기대된다. ( 이것도 더 확인이.. )
  
### 단점

* springboot 2.2 이전에는 의존성 추가 작업이 필요하다.
* mock이 필요한 경우 mockito를 사용해야한다.
* BDD 기반 테스트 작성시 spock 처럼 직관적인 코드 작성은 개발자 개인의 노력이 필요하다.

## spock

### 장점

* 블록기반으로 직관적인 BDD 기반의 테스트 코드 작성을 지원한다.
* groovy 만의 간결한 문법과 spock의 내장된 기능을 통해서 mockito 보다 간편한 mock 객체 생성이 가능하다.
* extension 기능을 통하여 테스트 전후의 결과값 핸들링이 가능하다.

### 단점

* 초기 환경 구성이 필요하다.
  * groovy 컴파일 설정.
  * spock 의존성 설정
* groovy를 먼저 컴파일 하다보니 lombok 과 같은 라이브러리 사용이 어렵다.
