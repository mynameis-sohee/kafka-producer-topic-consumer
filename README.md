### docker-compose 실행
docker-compose up -d


### 확인
docker ps


### 토픽 생성 (토픽: 데이터 저장 공간) (주키퍼 직접 통신 : 아키텍처의 복잡도를 높임 -> 부트스트랩 서버 옵션을 사용)
docker-compose exec broker kafka-topics --create --topic example-topic --bootstrap-server broker:9092 --replication-factor 1 --partitions 1 


### 카프카 브로커 CLI 접속
docker-compose exec broker bash


### 프로듀서 생성 (프로듀서: 데이터 생성)
docker-compose exec broker bash
kafka-console-producer --topic example-topic --broker-list broker:9092
"""
* key:value 형식의 option 추가:
        --property parse.key=true\
        --property key.separator=":"
    입력 예시 : key1:value1
"""


### 카프카 브로커 CLI 접속
docker-compose exec broker bash


### 컨슈머 생성 (컨슈머: 데이터 받음) [브로커 CLI 접속 후]
kafka-console-consumer --topic example-topic --bootstrap-server broker:9092


### 토픽 내 데이터 확인 [브로커 CLI 접속 후]
kafka-console-consumer --topic example-topic --bootstrap-server broker:9092  --from-beginning 

"""
* key:value 형식의 option 추가:
        --property parse.key=true\
        --property key.separator=":"
    출력 예시 : key1:value1
"""

### 토픽 상세 조회 [브로커 CLI 접속 후]
kafka-topics --bootstrap-server broker:9092 --describe --topic example-topic


---------------------------
# MySQL

### MySQL CLI 접속
docker exec -it mysql bash
mysql -uroot -ptest

### DB 생성 및 생성한 DB 사용
CREATE DATABASE IF NOT EXISTS connect_test;
USE connect_test;

### 테이블 CREATE (예시)
CREATE TABLE IF NOT EXISTS test (
  id serial NOT NULL PRIMARY KEY,
  name varchar(100),
  email varchar(200),
  department varchar(200),
  modified timestamp default CURRENT_TIMESTAMP NOT NULL,
  INDEX `modified_index` (`modified`)
);

### 데이터 INSERT (예시)
INSERT INTO test (name, email, department) VALUES ('alice', 'alice@abc.com', 'engineering');
INSERT INTO test (name, email, department) VALUES ('bob', 'bob@abc.com', 'sales');
