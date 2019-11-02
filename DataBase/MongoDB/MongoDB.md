# Mongo DB 정리

## NoSQL이란

* Non Relational Operational Database SQL
* 관계형 데이터베이스가 아닌 SQL
* 빅데이터와 클라우드 환경에서 기존 관계형 데이터베이스로만 처리시 문제점이 발생함. 해당 문제를 극복하기 위해 요구사항 발생.
  * 1989년 이후 퍼스널 컴퓨터가 보급되며 클라이언트/서버 환경의 개발 패러다임 등장
  * 21세기 통신환경과 하드웨어들의 발달로 인한 빅데이터 생산. 그로 인하여 다양한 운영프로그램들을 설치 및 사용하기위해 클라우드 컴퓨팅 환경 개발 패러다임 등장.

## NoSQL 장점

### 1. 클라우드 컴퓨팅 환경에 적합

* 대부분의 NoSQL은 오픈소스
* 하드웨어 확장에 유연한 대처가 가능
* RDBMS에 비해 저렴한 비용으로 분산,병렬 처리 가능

### 2. 유연한 데이터 모델

* 비정형 데이터 구조 설계로 설계 비용 감소
* 관계형 데이터베이스의 Relationship과 Join 구조를 Linking과 Embedded로 구현하여 성능이 빠름.

### 3. 빅데이터 처리에 효과적

* Memory Mapping 기능을 통해 IO가 빠름.
* 전형적인 OS와 Hardware에 구축 가능.
* 기존 RDB와 동일한 데이터 처리가 가능.

## NoSQL 종류

* MongoDB
* Casandra
* HBASE
* CouchDB
* Riak
* Redis

# MongoDB란

* JSON Type 데이터 저장 구조 제공
* Sharding(분산)/Replica(복제) 기능 제공
  * 장애 발생시 안전하게 데이터 보관 및 운영 관리
* MapReduce(분산/병렬처리) 기능 제공
  * 빅 데이터에 대한 빠른 추출 가능
* CRUD 위주의 다중 트랜잭션 처리도 가능
* Memory Mapping 기술을 기반한 Big Data 처리에 탁월한 성능 제공
  * 관계형 데이터베이스보다 3배~10배 이상의 성능이 보장될 수 있는 이유

## MongoDB 용어

|RDB|MongoDB|설명|
|:---|:---|:---|
|TABLE|Collection|컬렉션|
|ROW|Document|도큐멘트|
|COLUMN|FIELD|필드|
|PRIMARY_KEY|OBJECT_ID Field|오브젝트 ID 필드|
|RELATIONSHIP|EMBEDDED & LINK|임베디드 & 링크|

# Collection

* RDB 기준으로 본다면 Table
* 논리적, 물리적 구조를 비교해 본다면 관리하는 방법이 많이 다르다.
  * RDB
    * 기본적인 분석,설계 정규화를 통한 스키마 설계가 선행되어야함
  * NoSQL
    * 기본적인 DB 구조 설계만 진행해도 Collection 생성 가능함
    * 비정형 데이터 구조

* Gapped Collection
  * 최초 제한된 크기로 생성된 공간 내에서 만 데이터를 저장 가능
  * 공간을 모두 사용한다면 처음으로 돌아가서 기존 공간을 재사용함

* Non Gapped Collection
  * RDB의 테이블처럼 디스크 공간이 허용하는 범위 내에서 데이터를 저장할 수 있음

## INSET & UPDATE & REMOVE

|SQL|MongoDB|
|:---|:---|
|Create Table emp(no number(3))|db.createCollection({"emp"})|
|INSERT INTO emp VALUES(3,5)|db.emp.insert({empno:3, ename:5})|
|UPDATE emp SET ename='test' WHERE empno=1|db.emp.update({empno:1}, {$set:{ename:'test'}})|
|DELETE FROM emp WHERE deptno=10|db.emp.remove({deptno:10})|

* INSERT
  * Collection에 하나의 Document를 최초 저장할 때 사용.

* UPDATE
  * 하나의 Document에서 특정필드만 수정하는 경우 사용.
  * 빠른 수정이 요구되는 경우 가장 적합한 방법

* SAVE
  * 하나의 Document에서 특정필드만 변경하더라고 Document 전체를 변경하는 방법.

## DataType 종류

* OBJECT_ID 타입
![Object_ID](https://miro.medium.com/max/602/1*89a4srDnVRrKkuxjQqZzuw.png)
  * RDB의 ROWID와 유사한 데이터 속성.
  * 유일값으로 구성된다.
  * Default Object ID 와는 별개로 사용자가 직접 부여 가능.

* JSON 타입

|TypeName|TypeNumber|
|:---|:---|
|Double|1|
|String|2|
|Object|3|
|Array|4|
|Binary data|5|
|Object id|7|
|Boolean|8|
|Date|9|
|Null|10|
|Regular expression|11|
|JavaScript code|13|
|Symbol|14|
|JavaScript code with scope|15|
|23-bit integer|16|
|Timestamp|17|
|64-bit integer|18|
|Min key|255|
|Max key|127|

* Date와 Timestamp 타입
  * 싱글노드에서 Timestamp 타입은 64bit 값으로 저장 및 2개의 필드로 구성된 값이 리턴됨
  * Millisecond 값을 사용자가  직접 생성할 때 사용 가능.

* Sequence Number 타입
  * 기능을 제공하지는 않지만, 함수를 이용하여 직접 생성 가능

## 연산자 (Operator) 종류

* [공식 사이트 참조](https://docs.mongodb.com/manual/reference/operator/)

## 빅데이터 추출/분석

* [Aggreation FrameWork](https://docs.mongodb.com/manual/aggregation/) 함수
  * 빅데이터 추출에 최적화 됨.
  * MongoDB 2.2부터 지원
  * [MapReduce](https://m.blog.naver.com/PostView.nhn?blogId=jevida&logNo=140199795866&proxyReferer=https%3A%2F%2Fwww.google.com%2F)를 이용한 JavaScript로 생성되어 있음.
  * MongoDB의 MapReduce를 사용하여 빠른 성능을 보장.
  * MongoDB 내의 데이터만 처리 가능.

* MongoDB의 MapReduce
  * MongoDB의 Mao함수와 Reduce 함수를 javaScript 형태로 제공함.
  * 컬렉션 내의 데이터를 빠르게 읽고 가공 처리 가능

* Hadoop MapReduce와 MongoDB 연동
  * 기존에 Hadoop System이 도입된 경우 재활용할때 사용.

## [MapReduce](https://docs.mongodb.com/manual/core/map-reduce/) 기능

* 구글에서 대용량 데이터 처리 및 집합을 위해 만들어 졌으며 비공유 구조로 연결된 여러개의 노드에서 병렬 처리 방식으로 대용량 데이터를 처리 가능.
* Map과 Reduce 함수 만드로 병렬 프로그래밍이 가능.

## JavaScript 함수

* 비 저장형 javaScript
  * mongoDB에 영원히 저장되지 않음.
  * 필요에 따라 작성 및 호출가능
  * 호출은 db.eval 함수를 통하여 호출함.

* 저장형 javaScript
  * 컴파일된 실행코드를 미리 저장함.
  * 필요할 때 마다 호출하여 실행.
  * 호출은 db.eval 함수를 통하여 호출함.

# Lock 정책

* 1.x 버전
  * 글로벌-락 (Global lock)

* 2.2 버전
  * 데이터베이스-락 (Database Lock)

* 3.2 버전
  * 도큐멘트-락 (Document lock)
    * RDB의 Row Level Lock과 동일한 수준의 락

## Isolation

* RDB
  * Read Commited 를 기본 제공
  * 사용자가 변경전 데이터를 볼 수 있다.

* NoSQL
  * 빠른 입출력을 위한 Auto Commit를 동반함.
  * Auto Commit 기반의 고립화 정책을 Read Uncommited 이라고 한다.
    * 대부분의 NoSQL이 해당 정책을 기반으로 만들어졌다.
  * 빅데이터의 입출력시 Read Uncommited이 Read Commited보다 유리

# Transaction

## Two Phase Commits

* 4.2 이전 버전은 [Two Phase Commits](https://docs.mongodb.com/v3.6/tutorial/perform-two-phase-commits/)을 활용하여 RDB처럼 Commit 문, RollBack문과 유사한 방법으로 제어 가능.

* 특정 Collection의 데이터가 변경될 정보를 제2의 Collection에 저장하여 원자성과 일관성을 보장하는 방법

> 처리 순서
> 1. 데이터 입력시 STATE 필드에 트랜잭션 제어를 위해 initial로 Flag 설정
> 2. STATE가 initial 상태인 트랜잭션을 검색한 다음 pending 상태로 변경 후 수정할 데이터 변경 처리
> 3. 트랜잭션 종료를 위해 STATE 필드의 Flag를 committed 으로 변경 및 데이터 변경처리
> 4. 최종적으로 stste 필드를 done 으로 변경 처리

## Two Phase Commits

* 기본적인 방식은 Two Phase Commits과 동일함
* 제2의 Collection에서 보관하고 있는 변경 정보를 기준으로 다시 원복처리 진행.

> 처리 순서
> 1. 데이터 입력시 STATE 필드에 트랜잭션 제어를 위해 initial로 Flag 설정
> 2. STATE가 initial 상태인 트랜잭션을 검색한 다음 pending 상태로 변경 후 수정할 데이터 변경 처리
> 3. 트랜잭션 롤백을 위해 STATE 필드의 Flag를 canceling 으로 변경 및 데이터 변경처리
> 4. 최종적으로 stste 필드를 cancelled 으로 변경 처리

# 인덱스

* MongoDB는 RDB와 유사한 인덱스 구조를 제공함.

## 인덱스 재 구성과 삭제

```text
> db.emp.dropIndexes();
> db.emp.dropIndex({ eno : -1})
> db.runCommand({dropIndexes:'emp', index:{eno:-1}})
```
```text
> db.emp.reIndex()
> db.runCommand({reIndex : 'emp'})
```
* MongoDB에서 Index의 **대소문자는 엄격히 구분**됩니다.
* Document를 Update할 때 해당 IndexKey 만 변경되지만 변경되는 Document 크기가 기존 EXTENT 공간 크기보다 큰 경우에는 더 큰 EXTENT 공간으로 마이그레이션이 될 수 있기 때문에 성능 저하 현상이 발생할 수 있음 (충분한 EXTENT 크기로 설계 해야함)

> EXTENT?
> * MongoDB는 대용량 데이터를 HDD에 쉽게 저장할 수 있는 단위로 레코드들을 그룹핑grouping한다.이를 익스텐드extent라고 한다
> * 익스텐드들을 이용하여 MongoDB는 HDD에 저장될 파일과 삭제된 레코드를 관리한다.
> * 자료 구조 관점에서 보면 연결되어 있는 레코드들의 헤더 역할을 수행하는 것

* sort() 절과 limit() 절은 함께 사용하는 것이 성능에 도움이 됨.
* [Balance*Tree](https://hyungjoon6876.github.io/jlog/2018/07/20/btree.html)를 이용하여 데이터의 빠른 검색이 가능함.
  * 입력과 삭제가 빈번하게 발생하는 경우 성능을 보장하지 않음
  * 이 때 인덱스 구조를 재구성하여 해결할 수 있음.

## 인덱스 종류

* 공식 사이트 참조 (https://docs.mongodb.com/manual/indexes/)

### Sparse 인덱스

* 검색 대상 필드의 값이 전체 걸렉션에서 차지하는 밀도가 낮은 경우에 생성하면 유리한 인덱스 타입
* Full Collection 검색보다 조건을 만족하는 Document로 만 Sparse 인덱스를 생성 하고 검색하여 빠른 성능을 기대 가능
* 데이터 양과 밀도가 낮은 경우에 IO를 줄일 수 있어서 유리하다

### Partial 인덱스

* 검색 방법이 특정 컬럼의 조건을 만족하는 데이터 + 다시 특정 값만 가진 데이터를 추가로 조건 검색 하는 경우에 사용
* 위의 경우 B*Tree 인덱스에 비해서 현저히 적은 공간으로 인덱스 생성 가능
* 좁은 범위의 인덱스만 검색할 수 있어서 성능이 좋다.

### Background 인덱스

* 빅데이터 환경에서 인덱스를 생성시 크기가 엄청큰 대용량의 인덱스가 생성될 수 있다.
  * 생성시에 사용되는 많은 시스템 자원
* 백그라운드로 실행시 가장 시스템 자원을 활용하여 인덱스 생성 및 재구성함
  * 빠른 성능 보장
  * 시스템 자원이 충분하지 않은 서버 환경시
  * 빅 데이터 컬렉션에 대한 효율적인 인덱스 생성시

### Covered 인덱스

* 빅데이터 환경에서 일반적인 인덱스로 조회시 성능 지연 발생 가능
* 위에 상황에서 조건을 만족하는 Covered 필드만으로 인덱스를 생성하고 검색시 보다 빠르게 검색이 가능

### GeoSpatial 인덱스

* 좌표에 의해 구성된 2차원 구조로 하나의 Collection에 2D Index를 생성 가능
* 현재 좌표 위치를 기준으로 가장 가깝게 등록된 맛 집 좌표를 검색 가능.

#### GeoSpatial 연산자 종류

* $near 비교 연산자
  * 해당 좌표를 기준으로 가장 가까운 좌표를 검색시 사용

* $center 비교 연산자
  * 해당 좌표를 기준으로 가장 가까운 원형 좌표 검색시 사용

* $box 비교 연산자
  * 해당 좌표를 기준으로 가장 가까운 Box형 좌표를 검색시 사용

* $polygon 비교 연산자
  * 해당 좌표를 기준으로 가장 가까운 다면형 좌표를 검색시 사용

* $centerSphere 비교 연산자
  * 해당 좌표를 기준으로 3 Mile 내의 좌표를 검색하고 반드시 $within 연산자와 함께 검색해야함.

* $nearSphere 비교 연산자
  * 해당 좌표 기준으로 3 Mile 내의 좌표를 검색. 중복 조건을 만족하는 document가 여러 개인 경우 모두 리턴

### GeoMetry 인덱스

* geoJSON은 직선 또는 곡선의 교차에 의하여 이루어지는 추상적인 구조나 다각형과 같은 기하학 구조를 일컫는 말이며 이러한 구조에 만들어지는 인덱스 구조를 geometry 인덱스라고 함.
* MongoDB 2.4 부터 제공됨.
* 위치 정보 기반의 데이터 저장과 검색이 용이함.

#### Point Type

* 해당 좌표 기준으로 특정 거리 이내의 포인터 좌표로 등록된 위치를 검색

#### LineString Type

* 출발 지점과 도착 지점까지 갈 수 있는 선분 정보(포인터 좌표 기준)를 기준으로 조회

#### PolygonType

* 특정좌표로 구획된 다각형의 지리 정보로 검색시 사용

# Data Modeling

* RDB의 ERD 처럼 CD(Collection Diagram) 을 작독.

> 모델링 작업 절차
>
> 1. 개념적 데이터 모델링 : 비즈니스 영역으로부터 데이터를 수집하는 단계
> 2. 논리적 데이터 모델링 : Document 구조에 맞게 분석, 설계하는 단계
> 3. 물리적 데이터 모델링 : MongoDB 물리적 구조에 맞게 설계하는 단계

## 설계 주요 특징

* MongoDB는 데이터와 프로세스 모두가 설계의 중심
  * 단순하게 컬렉션을 생성하고 데이터를 저장하는 것 만으로는 효율적인 운영과 관리에 문제가 발생할 수 있음
  * 기업의 업무적 프로세스와 데이터 모두를 설계의 중심으로 둠으로써 유연한 데이터 구조를 설계할 수 있고 관리할 수 있어야함

### Rich Document 구조

* 비정규화된 구조
  * RDB는 중복을 제거하고 무결성을 보장하기 위해 정규화된 구조로 설계
* 여러대의 서버를 중심으로 구축 및 확장, 복제하는 ScaleOutp 중심의 관리기법을 지향
* RDB와 NoSQL을 상호 보완적인 역할로 사용한다면 시너지 효과를 극대화 가능

### 중첩 구조 (Embedded Document)

* RDB는 Entity간의 관계(Relationship) 를 중심으로 데이터의 무결성을 보장
  * JOIN을 유발시킴으로써 코딩양 증가
  * 검색 성능 저하시키는 요인 제공
* NoSQL은 중첩구조로 설계가 가능하여 불필요한 JOIN을 최소화 가능

### 논-스키마 구조

* 기본적으로 Schema가 없기 때문에 유연한 데이터 구조를 설계 가능

### 비정형 데이터 구조

* 기존의 업무영역에서 처리하기 힘들었던 비정형 데이터에 대한 저장과 관리가 용이하다
  * 이미지, 동영상, 음성 등

### 유연한 서버 구조

* RDB에 비해 빠른 쓰기와 읽기가 가능한 유연성 있는 아키텍처 구조를 제공
  * RDB는 DBMS 전용 메모리 영역을 할당받아서 사용함.
    * 다수의 사용자가 소량의 데이터를 처리하는 트랜잭션 위주의 업무 환경
  * NoSQL은 유연한 시스템 자원 할당 기술 사용함.
    * 초당 10만건 이상의 빅 데이터가 발생하는 비즈니스 환경

## 설계 기준

### 1. 데이터 조작은 어떻게 수행하는가

* 필드별로 데이터의 양, IO 발생 비율 등등 고려
  * 매일 참조되는 필드와 일년에 한번 참조되는 필드가 같은 Collection내에 존재시 시스템 자원을 낭비하게됨

### 2. Access Pattern은 어떤가

* IO 발생 빈도
  * 빅데이터 기준으로 Collection 분리 설계시 고려 사항
    * 여러 개의 Collection에 쓰기 작업시
    * 여러 개의 Collection에 읽기 작업시
* 데이터의 Life cycle

### 3. SCHEME 설계 시 고려 사항

* 데이터는 업무적 성격에 따라서 다양한 다양성을 가지고 있다.
* 다양성에 맞는 가장 적절한 데이터 구조를 설계할 수 있어야 한다.
  * MongoDB 데이터 저장 기술에 대한 명확힌 이해가 필요함.

## 설계 패턴

* 강한 참조
  * 중첩테이블(Nested Table)을 활용
  * 데이터 저장에 제한이 없다
* 약한 참조
  * 배열을 활용
  * 데이터 저장에 제한이 있다.

### 1. Rich Document

* 장점
  * Query가 단순해진다.
  * Join을 실행할 필요가 없어서 빠른 성능이 보장됨
  * 데이터 보안에 효과적

* 단점
  * Embedded 되는 도큐멘트의 크기는 최대 16MB에서 가능
  * Embedded 되는 도큐멘트가 조재하지 않는 컬렉션에는 부적합

* 약한 참조를 가지는 구조에서 Rich Document로 설계는 성능상 불이익

#### Embedded Document

* RDB는 정규화되어 데이터 검색시 JOIN 이 필요한 경우가 많다
* 필드에 Document 정보가 내장되어 한번의 조회로 다른 정보들을 조회가 가능

#### Extent Document

* Collection에 먼저 저장 후 Update 처리
* 먼저 저장 후 Document를 업데이트 하기 때문에 확장형 도큐멘트라고 표현함.

### 2. Link

* RDB는 PK,FK 로 연관관계 설정
* ObjectId를 통해 연결고리를 설정
  * 논리적으로는 유사하지만 물리적으로는 다른 데이터 구조를 가진다.