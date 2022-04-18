# Part 2. 실무에서 쉽게 써보는 Kafka: 클러스터 구축부터 MSA 환경에서 활용까지

## Apache Kafka 설치

[Apache Kafka](https://kafka.apache.org/downloads)

실습은 Scala 2.13  - kafka\_2.13-3.1.0.tgz 로 진행

```java
// Apache Kafka down
$ wget <https://dlcdn.apache.org/kafka/3.1.0/kafka_2.13-3.1.0.tgz>

// 압축 해제
$ tar zxvf kafka_2.13-3.1.0.tgz kafka_2.13-3.1.0/

// zookeeper 실행
$ bin/zookeeper-server-start.sh config/zookeeper.properties
// zookeepr 실행 확인.
$ ./ bin/zookeeper-shell.sh localhost:2181

// broker를 띄우기 위한 설정
$ vi config/server.properties

31 line '#' 제거및
listeners = 127.0.0.1:9092 기입 후 저장.

// broker 실행
$ bin/kafka-server-start.sh config/server.properties

// topic 생성
```
