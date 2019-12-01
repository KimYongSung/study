# AWS 와 Docker

## AWS 장단점

* 장점
  * 탄력적인 웹 규모 컴퓨팅
  * 다양한 Command(API) 제공
  * 유연한 클라우드 호스팅 서비스
  * 통합
  * 안전성
  * 보안

* 단점
  * 베어 메탈 성능을 원할때
  * 웹페이지가 몇개뿐일때
  * 솔루션에 적합
  * 가격

## AWS 종류

### Server

* EC2 ( Elastic Compute Cloud)
* Lambda
  * 서버가 필요 없이 코드를 올려서 실행가능
  * 코드가 실행되야 과금이 됨
* VPC ( Virtual Private Cloud )

### Storage

* S3
* EBS (Elastic Block Store)

### DataBase

* RDS
  * 관리형 관계형 데이터베이스
  * Amazon Ourora, Oracle, MySQL, maria 등등
  
* DynamoDB
  * NoSQL

* RedShift
* AuroraDB
  * mysql 및 postrageSql

### Management

* Cloud Watch

### Analysis

* Kinesis

## Docker 장점

* 실행시점에 상관없이 구성 시점을 고를 수 있음
  * 버전 관리 차원에서 동시에 뱁포가 가능
  * 눈송이 서버 : 서버가 많아질때 각 서버별로 버전이 상이함
    * 이미지를 똑같이 컨테이너에 배포하여 해결 가능

* 개발 프로그램 설치와 삭제가 용이
  * java, mysql, oracle, elk, nginx 등등

* 실행 소스 일관성, 유연성
  * 초기 인프라 환경 설정은 복잡하고 어려운, 쉽고 일관성이 있게 만들어 줌

* 이미지 용량이 크게 줄어듬
  * 리눅스 컨테이너를 사용

* 여러군데 배포할 수 있는 확장성
