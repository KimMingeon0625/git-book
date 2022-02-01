---
description: 카프카 빠르게 시작해보기
---

# Chapter 2

## 실습용 카프카 브로커 설치

### AWS EC2 인스턴스 발급 및 보안 설정

#### EC2 Dashboard

![(우측상단)인스턴스 시작](<../.gitbook/assets/image (9) (1) (1) (1) (1) (1).png>)

#### AMI 선택 화면

![프리 티어 사용가능한 Amazon Linex 2 AMI 선택](<../.gitbook/assets/image (2).png>)

#### 인스턴스 유형 선택

![찬가지로 프리티어가 가능한 인스턴스를 선택](<../.gitbook/assets/image (11) (1) (1) (1) (1) (1).png>)

#### 인스턴스 시작 및 검토

![마지막 확인 절차. 확인 뒤 시작하기](<../.gitbook/assets/image (15) (1) (1) (1) (1) (1) (1).png>)

#### 서버 접속을 위한 키 페어 설정

![키 페어 름 test-kafka-server-key / 키 페어 다운로드](<../.gitbook/assets/image (7) (1) (1).png>)

#### 보안 그룹 규칙

![](<../.gitbook/assets/image (2) (1) (1).png>)

* 로컬 터미널

```
// pem 파일 권한 변경(read only)
chmod 400 test-kafka-server-key.pem

// 접속
ssh -i test-kafka-server-key.pem ec2-user@인스턴스 주
```

### 자바 설치

```
// 설치
sudo yum install -y java-1.8.0-openjdk-devel.x86_64

// 확인
java -version
```

### 주키퍼/카프카 브로커 실행

```
// install
wget https://archive.apache.org/dist/kafka/2.5.0/kafka_2.12-2.5.0.tgz

// 압축 해제
tar xvf kafka_2.12-2.5.0.tgz

cd kafka_2.12-2.5.0.tgz
```

#### 카프카 브로커 힙/메모리 설정

* 카프카 브로커는 레코드의 내용은 페이지 캐시로 시스템 메모리를 사용하고 나머지 객체들은 힙 메모리에 저장하여 사용한다. (일반적으로 카프카 브로커를 운영할 때 힙 메모리를 5GB 이상으로 설정 X)
* Defatult 힙 메모리 - 카프카 브로커 = 1G, 주키퍼 = 512MB
* 실습용 인스턴스는 1GB 메모리를 가지고 있으므로 1.5GB 메모리를 담아내지 못한다. 이를 해결하기 위해 메모리 설정이 필요. ( 메모리가 넉넉하다면 지정하지 않아도 된다.)

```
// 메모리 설정
export KAFKA_HEAP_OPTS="-Xmx400m -Xms400m"

// 확인
echo $KAFKA_HEAP_OPTS
```

* 터미널에서 사용자가 입력한 KAFKA\_HEAP\_OPTS 환경변수는 터미널 세션이 종료되면 초기화되어 재사용이 불가. 이를 위해 \~/.bashrc 파일에 환경변수 선언문을 넣을 필요가 있다.

```
// 편집기 실행
$ vi ~/.vashrc

// 아래와 같이 설정
# .bashrc

# Source global definitions
if [ -f /etc/bashrc ]; then
        . /etc/bashrc
fi

# Uncomment the following line if you don't like systemctl's auto-paging feature:
# export SYSTEMD_PAGER=

# User specific aliases and functions
export KAFKA_HEAP_OPTS="-Xmx400m -Xms400m"
// 여기까지 파일 끝.

// 적용
$ source ~/.bashrc
$ echo $KAFKA_HEAP_OPTS
-Xmx400m -Xms400m

```

#### 카프카 브로커 실행 옵션 설정

config/server.properties : 카프카 브로커가 클러스터 운영에 필요한 옵션.

* 실습용 카프카 브로커를 실행할 것이므로 advertised.listener만 설정. (하단 코드 (3)에 해당)
* advertised.listener는 카프카 클라이언트 또는 커맨드 라인 툴을 브로커와 연결할 때 사용.
* 이미 실행되고 있는 카프카 브로커의 설정을 변경하고 싶다면 브로커를 재시작해야 한다.

```
// 편집기 실행
vi config/server.properties

============= Server Basics =============

 Licensed to the Apache Software Foundation (ASF) under one or more
# contributor license agreements.  See the NOTICE file distributed with
# this work for additional information regarding copyright ownership.
# The ASF licenses this file to You under the Apache License, Version 2.0
# (the "License"); you may not use this file except in compliance with
# the License.  You may obtain a copy of the License at
#
#    http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

# see kafka.server.KafkaConfig for additional details and defaults

############################# Server Basics #############################

# The id of the broker. This must be set to a unique integer for each broker.
broker.id=0 // =============== (1) ===============

############################# Socket Server Settings #############################

# The address the socket server listens on. It will get the value returned from
# java.net.InetAddress.getCanonicalHostName() if not configured.
#   FORMAT:
#     listeners = listener_name://host_name:port
#   EXAMPLE:
#     listeners = PLAINTEXT://your.host.name:9092
#listeners=PLAINTEXT://:9092 // =============== (2) ===============

# Hostname and port the broker will advertise to producers and consumers. If not set,
# it uses the value for "listeners" if configured.  Otherwise, it will use the value
# returned from java.net.InetAddress.getCanonicalHostName().
advertised.listeners=PLAINTEXT://[퍼블릭 IP]:9092 // =============== (3) ===============

# Maps listener names to security protocols, the default is for them to be the same. See the config documentation for more details
#listener.security.protocol.map=PLAINTEXT:PLAINTEXT,SSL:SSL,SASL_PLAINTEXT:
SASL_PLAINTEXT,SASL_SSL:SASL_SSL // =============== (4) ===============

# The number of threads that the server uses for receiving requests from the network and sending responses to the network
num.network.threads=3 // =============== (5) ===============
                                                                                                                               1,1           Top

# The number of threads that the server uses for processing requests, which may include disk I/O
num.io.threads=8 // =============== (6) ===============

# The send buffer (SO_SNDBUF) used by the socket server
socket.send.buffer.bytes=102400

# The receive buffer (SO_RCVBUF) used by the socket server
socket.receive.buffer.bytes=102400

# The maximum size of a request that the socket server will accept (protection against OOM)
socket.request.max.bytes=104857600


############################# Log Basics #############################

# A comma separated list of directories under which to store log files
log.dirs=/tmp/kafka-logs // =============== (7) ===============

# The default number of log partitions per topic. More partitions allow greater
# parallelism for consumption, but this will also result in more files across
# the brokers.
num.partitions=1 // =============== (8) ===============

# The number of threads per data directory to be used for log recovery at startup and flushing at shutdown.
# This value is recommended to be increased for installations with data dirs located in RAID array.
num.recovery.threads.per.data.dir=1

############################# Internal Topic Settings  #############################
# The replication factor for the group metadata internal topics "__consumer_offsets" and "__transaction_state"
# For anything other than development testing, a value greater than 1 is recommended to ensure availability such as 3.
offsets.topic.replication.factor=1
transaction.state.log.replication.factor=1
transaction.state.log.min.isr=1

############################# Log Flush Policy #############################

# Messages are immediately written to the filesystem but by default we only fsync() to sync
# the OS cache lazily. The following configurations control the flush of data to disk.
# There are a few important trade-offs here:
#    1. Durability: Unflushed data may be lost if you are not using replication.
#    2. Latency: Very large flush intervals may lead to latency spikes when the flush does occur as there will be a lot of data to flush.
#    3. Throughput: The flush is generally the most expensive operation, and a small flush interval may lead to excessive seeks.
# The settings below allow one to configure the flush policy to flush data after a period of time or
# every N messages (or both). This can be done globally and overridden on a per-topic basis.

# The number of messages to accept before forcing a flush of data to disk
#log.flush.interval.messages=10000

# The maximum amount of time a message can sit in a log before we force a flush
#log.flush.interval.ms=1000

############################# Log Retention Policy #############################

# The following configurations control the disposal of log segments. The policy can
# be set to delete segments after a period of time, or after a given size has accumulated.
# A segment will be deleted whenever *either* of these criteria are met. Deletion always happens
# from the end of the log.

# The minimum age of a log file to be eligible for deletion due to age
log.retention.hours=168 // =============== (9) ===============

# A size-based retention policy for logs. Segments are pruned from the log unless the remaining
# segments drop below log.retention.bytes. Functions independently of log.retention.hours.
#log.retention.bytes=1073741824

# The maximum size of a log segment file. When this size is reached a new log segment will be created.
log.segment.bytes=1073741824 // =============== (10) ===============

# The interval at which log segments are checked to see if they can be deleted according
# to the retention policies
log.retention.check.interval.ms=300000 // =============== (11) ===============

############################# Zookeeper #############################

# Zookeeper connection string (see zookeeper docs for details).
# This is a comma separated host:port pairs, each corresponding to a zk
# server. e.g. "127.0.0.1:3000,127.0.0.1:3001,127.0.0.1:3002".
# You can also append an optional chroot string to the urls to specify the
# root directory for all kafka znodes.
zookeeper.connect=localhost:2181 // =============== (12) ===============

# Timeout in ms for connecting to zookeeper
zookeeper.connection.timeout.ms=18000 // =============== (13) ===============
                                                                                                                               121,1         89%
                                                                                                                        76,1          44%
############################# Group Coordinator Settings #############################

# The following configuration specifies the time, in milliseconds, that the GroupCoordinator will delay the initial consumer rebalance.
# The rebalance will be further delayed by the value of group.initial.rebalance.delay.ms as new members join the group, up to a maximum of max.poll.interval.ms.
# The default value for this is 3 seconds.
# We override this to 0 here as it makes for a better out-of-the-box experience for development and testing.
# However, in production environments the default value of 3 seconds is more suitable as this will help to avoid unnecessary, and potentially expensive, rebalances during application startup.
group.initial.rebalance.delay.ms=0

```

1.  실행하는 카프카 브로커의 번호.

    단 하나뿐인(unique) 번호로 설정(동일한 id를 가질 경우 비정상적인 동작이 발생 할 수 있다.)
2.  카프카 브로커가 통신을 위해 열어둘 인터페이스 IP,port,프로토콜 설정.

    (설정하지 않는 경우 모든 IP와 port에서 접속 가능.)
3.  카프카 클라이언트 또는 카프카 커맨드 라인 툴에서 접속할 때 사용하는 IP와 port 정보.

    (인스턴스를 생성할 때 발급받은 퍼블릭 IPv4값을 IP에 넣고, port에는 카프카 기본 포트인 9092.)
4. SASL\_SSL,SASL\_PLAIN 보안 설정 시 프로토콜 매핑을 위한 설정.
5. 네트워크를 통한 처리를 할 때 사용할 네트워크 스레드 개수 설정이다.
6. 카프카 브로커 내부에서 사용할 스레드 개수.
7.  통신을 통해 가져온 데이터를 파일로 저장할 디렉토리 위치.

    (디렉토리가 없는 경우 오류가 발생할 수 있으므로 브로커 실행 전에 디렉토리 생성 여부를 확인한다.)
8.  파티션 개수를 명시하지 않고 토픽을 생성할 때 기본 설정되는 파티션 개수.

    (파티션 개수가 많아지면 병렬처리 데이터양이 늘어난다.)
9.  카프카 브로커가 저장한 파일이 삭제되기까지 걸리는 시간을 설정.

    가장 작은 단위를 기준으로 하므로 log.retention.hours보다는 log.retention.ms 값을 설정하여 운영하는 것을 추천. log.rentention.ms 값을 -1로 설정하는 경우 삭제는 이루어지지 않는다.
10. 카프카 브로커가 저장할 파일의 최대 크기를 지정. (이 크기를 채운 경우 새로운 파일 생성.)
11. 카프카 브로커가 저장한 파일을 삭제하기 위해 체크하는 간격 지정.
12. 카프카 브로커와 연동할 주키퍼의 IP와 port를 설정. 실습을 위해 EC2 인스턴스에 주키퍼와 카프카 브로커를 동시에 실행하므로 localhost:2181로 설정한다.
13. 주키퍼의 세션 타임아웃 시간을 지정.



#### 주키퍼 실행

주키퍼

* 분산 코디네이션 서비스 제공
* 카프카의 클러스터 설정 리더 정보
* 컨트롤러 정보

보통 3개이상의 서버로 구성한다.

1대만 실행하는 주키퍼를 'Qucik-and-dirty single-node'라 칭한다. (테스트용으로만 사용)

```
$ bin/zookeeper-server-start.sh -daemon config/zookeeper.properties
$ jps -vm

// 이상이 있는 경우 daemon을 제거. 로그를 확인하자.
// jps : JVM 프로세스 상태를 보는 도구.
```



#### 카프카 브로커 실행 및 로그 확인

```
$ bin/kafka-server-start.sh -daemon config/server.properties
$ jps -m

$ tail -f logs/server.log
```



### 로컬 컴퓨터에서 카프카와 통신 확인

```
// wsl ubuntu 환경 : java가 없는 경우
sudo apt -y install openjdk-8-jdk

$ curl https://archive.apache.org/dist/kafka/2.5.0/kafka_2.12-2.5.0.tgz --output kafka.tgz

$ tar -xvf kafka.tgz

$ cd kafka_2.12-2.5.0

$ bin/kafka-broker-api-versions.sh --bootstrap-server [퍼블릭 IP]:9092
```

#### 테스트 편의를 위한 hosts 설정

```
$ vi /etc/hosts
[IP] my-kafka
```



## 카프카 커맨드 라인 툴



### Kafka-topics.sh

{% hint style="success" %}
Topic 생성 2가지

1. Consumer, Producer 가 Broker에 생성되지 않은 Topic에 대해 데이터를 요청할 때.
2. 'Command line tool' 명시적으로 Topic 생성.

Topic마다 처리 하는 데이터의 특성이 다르기 때문에 2번을 추천.
{% endhint %}



#### 토픽 생성

```
// 기본적인 토픽 생성
$ bin/kafka-topics.sh \
--create \
--bootstrap-server my-kafka:9092 \
--topic hello.kafka 

// 옵션을 활용한 토픽 생성
$ bin/kafka-topics.sh \
--create \
--bootstrap-server my-kafka:9092 \
--partitions 3 \
--replication-factor 1 \
--config retention.ms=172800000 \
--topic hello.kafka.2 \
```

option

* partitions : 파티션의 개수 (최소 = 1개, default = server.properties의 num.partitions)
* replication-factor : 레플리카 개수 (최소 = 1개 / 2인 경우 (원본 1개 +복제품 1개))
* config rentention.ms : 토픽 데이터 유지 기간
*

#### 토픽 리스트 조회

```
$ bin/kafka-topics.sh --bootstrap-server my-kafka:9092 --list
```

#### 토픽 상세 조회

```
$ bin/kafka-topics.sh --bootstrap-server my-kafka:9092 --describe --topic hello.kafka.2
```

#### 토픽 옵션 수정

```
// 토픽관련 옵션은 topics 와 configs에 위치.

// 파티션 추가 (기존 3개)
$ bin/kafka-topics.sh --bootstrap-server my-kafka:9092 \
--topic hello.kafka \
--alter \
--partitions 4

// 조회
$ bin/kafka-topics.sh --bootstrap-server my-kafka:9092 \
--topic hello.kafka \
--describe


// 토픽 데이터 저장 기간 변경
$ bin/kafka-configs.sh --bootstrap-server my-kafka:9092 \
--entity-type topics \
--entity-name hello.kafka \
--alter --add-config retention.ms=86400000

// 조회
$ bin/kafka-configs.sh --bootstrap-server my-kafka:9092 \
--entity-type topics \
--entity-name hello.kafka \
--describe
```



### Kafka-console-producer.sh

```
// 메시지 키 없이 메시지 값만 전송
$ bin/kafka-console-producer.sh --bootstrap-server my-kafka:9092 \
--topic hello.kafka
>hello
>kafka
>0
>1
>2
>3
>4
>5

// 메시지 키를 가지는 레코드 전송
$ bin/kafka-console-producer.sh --bootstrap-server my-kafka:9092 \
--topic hello.kafka \
--property "parse.key=true" \
--property "key.separator=:"
>key1:no1
>key2:no2
>key3:no3


```

* kafka-console-producer.sh로 전송하는 레코드 값은 UTF-8을 기반으로 Byte로 변환되고 ByteArraySerializer로만 직렬화 된다. (텍스트 목적으로 문자열만 전송 가능)
* parse.key = true -> 메시지 키 추가 가능
* key.separator 원하는 separator 선언 가능.
* 구분자 없이 입력할 경우 kafkaException 발생과 종료.

### Kafka-console-consumer.sh

```
$ bin/kafka-console-consumer.sh --bootstrap-server my-kafka:9092 \
--topic hello.kafka \
--from-beginning

$ bin/kafka-console-consumer.sh --bootstrap-server my-kafka:9092 \
--topic hello.kafka \
--property print.key=true \
--property key.separator="-" \
--group hello-group \
--from-beginning

```

* \--from-beginning : 토픽에 저장된 가장 처음 데이터부터 출력.
* print.key : 메시지 키 확인.
* key.seperator : 미설정의 경우 tab delimieter(\t)로 설정되어 출력.&#x20;
*   \--group 옵션을 통해 신규 컨슈머 그룹(consumer group)을 생성.

    <mark style="color:green;">**컨슈머 그룹을 통해 가져간 토픽의 메시지는  커밋.(레코드의 오프셋 번호를 브로커에 저장)**</mark>

    <mark style="color:green;">**커밋 정보는 \_\_consumer\_offsets 이름의 내부 토픽에 저장.**</mark>
* 파티션의 순서를 보장하고 싶다면 파티션 1개로 구성된 토픽을 생성하는 방법을 추천.

### Kafka-consumer-groups.sh

```
// 생성된 컨슈머 그룹 리스트
$ bin/kafka-consumer-groups.sh --bootstrap-server my-kafka:9092 --list

// 컨슈머 그룹의 상세 내용
$ bin/kafka-consumer-groups.sh --bootstrap-server my-kafka:9092 \
--group hello-group \
--describe

// 컨슈머 그룹 상세 내용에 대한 출력
Consumer group 'hello-group' has no active members.

GROUP           TOPIC           PARTITION  CURRENT-OFFSET  LOG-END-OFFSET  LAG             CONSUMER-ID     HOST            CLIENT-ID
hello-group     hello.kafka     0          7               7               0               -               -               -
```

*   group / topic / partition : 조회한 group이 마지막으로 커밋한 토픽과 파티션.

    \-> hello-group 이름의 컨슈머 그룹이 hello.kafka 토픽의 0번 파티션의 레코드가 마지막으로 커밋.
* current-offset : 컨슈머 그룹이 가져간 토픽의 파티션에 가장 최신 오프셋.
*   log-end-offset : 해당 컨슈머 그룹의 컨슈머가 어느 오프셋까지 커밋했는지 나타냄.

    \-> current-offset은 log-end-offset과 같거나 작은값이다.
* 렉(lag)은 컨슈머 그룹이 토픽의 파티션에 있는 데이터를 가져가는데 발생하는 지연.
*   consumer-id : 컨슈머의 토픽(파티션) 할당을 카프카 내부적으로 구분하기 위한 id.

    \-> client-id + uuid(universally unique identifier) 값을 붙여서 자동 할당되어 유니크한 값으로 설정.
* host : 컨슈머가 동작하는 host명.
* client-id : 컨슈머에 할당된 id. (사용자 지정 가능 -> 지정하지 않을 시 자동 생성)

컨슈머 그룹의 상세정보를 통해

1. 컨슈머 그룹의 중복 여부.
2.  운영하는 컨슈머의 렉을 확인하여 컨슈머 상태 최적화.

    \-> consumer의 lag이 증가하는 경우, producer가 topic에 data를 적재하는 속도에 비해 consumer의 처리량이 느리다는 증거.

### Kafka-verifiable-producer,consumer.sh

카프카 클러스터 설치가 완료된 이후 토픽에 데이터를 전송하여 간단한 네트워크 통신 테스트에 사용.

```
// 데이터 전송
$ bin/kafka-verifiable-producer.sh --bootstrap-server my-kafka:9092 \
--max-message 10 \
--topic verify-test

// 전송한 데이터 확인
$ bin/kafka-verifiable-consumer.sh --bootstrap-server my-kafka:9092 \
--topic verify-test \
--group-id test-group

// 출력된 데이터
{"timestamp":1637043498474,"name":"startup_complete"}
{"timestamp":1637043500155,"name":"partitions_assigned","partitions":[{"topic":"verify-test","partition":0}]}
{"timestamp":1637043501146,"name":"records_consumed","count":10,"partitions":[{"topic":"verify-test","partition":0,"count":10,"minOffset":0,"maxOffset":9}]}
{"timestamp":1637043501336,"name":"offsets_committed","offsets":[{"topic":"verify-test","partition":0,"offset":10}],"success":true}
```

### Kafka-delete-records.sh

적재된 토픽의 데이터 삭제.

```
$ vi delete-topic.json
{"partitions": [{"topic": "test", "partition": 0, "offset": 50}], "version":1 }

$ bin/kafka-delete-records.sh --bootstrap-server my-kafka:9092 \
--offset-json-file delete-topic.json
```

*   삭제하고자 하는 데이터에 대한 정보를 파일로 저장해서 사용.

    \-> delete-topic.json 파일로 생성. (토픽, 파티션, 오프셋 정보가 들어가야 한다.)

#### 주의 : 토픽의 특정 레코드 하나만 삭제되는 것이 아닌, 파티션 내의 가장 오래된 오프셋부터 지정한 오프셋까지 삭제됨. ( 토픽의 파티션에 지정된 특정 데이터만 삭제할 수 없다.)
