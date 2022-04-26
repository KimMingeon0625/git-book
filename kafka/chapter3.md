---
description: 카프카 기본 개념 설명
---

# Chapter3

## 카프카 브로커/클러스터/주키퍼

데이터를 안전하게 보관하고 처리하기 위해 3대 이상의 브로커 서버를 1개의 클러스터로 묶어서 운영.

#### 데이터 저장, 전송

* 프로듀서가 요청한 토픽의 파티션에 데이터 저장.(파일 시스템).&#x20;
* 컨슈머가 요청하면 파티션에 저장된 데이터 전달.

```
// EC2
// 파일 시스템에 저장된 데이터
$ ls /tmp/kafka-logs

// hello.kafka topic의 0번 파티션 데이터
$ ls /tmp/kafka-logs/hello.kafka-0
```

*   카프카는 데이터를 파일 시스템에 저장한다.

    페이지 캐시(메모리 영역)를 사용하여 디스크 입출력 속도 향상.

    한번 읽은 파일의 내용은 메모리의 페이지 캐시 영역에 저장. 동일한 파일 접근은 메모리에서 읽음.

#### 데이터 복제, 싱크

* 카프카의 데이터 복제는 파티션 단위.
* 복제된 파티션은 리더(leader)와 팔로워(Follower)로 구성.

{% hint style="info" %}
리더 파티션 : 프로듀서 또는 컨슈머와 직접 통신하는 파티션.

팔로워 파티션 : 나머지 복제 데이터를 가지고 있는 파티션.
{% endhint %}

* 복제(replica) : 리더 파티션의 오프셋을 확인하여 자신의 오프셋과 차이가 나는 경우 리더 파티션으로 부터 데이터를 가져오는 과정.
* 리더 파티션이 사용할 수 없는 경우 팔로워 파티션 중 하나가 지위를 넘겨받는다.

#### 컨트롤러(Controller)

* 클러스터의 다수 브로커 중  한 대가 컨트롤러의 역할을 한다.
* 컨트롤러는 다른 브로커들의 상태를 체크하고 브로커가 클러스터에서 빠지는 경우 해당 브로커에 존재하는 리더 파티션을 재분배한다.
* 컨트롤러 역할의 브로커에 장애가 생기는 경우 다른 브로커가 컨트롤러 역할을 한다.

#### 데이터 삭제

* 컨슈머가 데이터를 가져가도 토픽의 데이터는 삭제되지 않는다.
* 컨슈머나 프로듀서가 데이터 삭제를 요청 할 수 없다.(브로커만 데이터 삭제 가능)

{% hint style="info" %}
로그 세그먼트(log segment) : 데이터 삭제의 파일 단위

세그먼트 파일이 닫히는 기본값 : 1GB

닫힌 세그먼트 파일 삭제 관련 옵션 : log.retention.bytes or log.retension.ms

닫힌 세그먼트 파일 체크 간격 옵션 : log.retention.check.interval.ms
{% endhint %}



#### 컨슈머 오프셋 저장

* 파티션의 어느 레드까지 가져갔는지 확인하기 위해 오프셋을 커밋.
* 커밋한 오프셋은 --consumer\_offsets 토픽에 저장



#### 코디네이터(Coordinator)

{% hint style="info" %}
코디네이터 : 컨슈머 그룹의 상태를 체크하고 파티션을 컨슈머와 매칭되도록 분배하는 역할.

리밸런스(rebalance) : 파티션을 컨슈머로 재할당.
{% endhint %}

* 클러스터의 다수 브로커 중 한 대는 코디네이터의 역할을 수행
* 컨슈머가 컨슈머 그룹에서 빠지면 매칭되지 않은 파티션을 정상 동작하는 컨슈머로 할당하여 끊임없이 데이터 처리.



#### 주키퍼

* 카프카의 메타데이터를 관리하는데 사용

```
// 로컬 주키퍼 기본 포트 : 2181
$ bim/zookeeper-shell.sh my-kafka:2181

Connecting to my-kafka:2181
Welcome to ZooKeeper!
JLine support is disabled

WATCHER::

WatchedEvent state:SyncConnected type:None path:null

// root znode의 하위 znode 확인
ls /
[admin, brokers, cluster, config, consumers, controller, controller_epoch, isr_change_notification, latest_producer_id_block, log_dir_event_notification, zookeeper]

// 카프카 브로커에 대한 정보 (보안 규칙, jmx port, host, 등등)
get /brokers/ids/0
{"listener_security_protocol_map":{"PLAINTEXT":"PLAINTEXT"},"endpoints":["PLAINTEXT://52.14.108.146:9092"],"jmx_port":-1,"host":"52.14.108.146","timestamp":"1637036405828","port":9092,"version":4}

// 컨트롤러 정보
get /controller
{"version":1,"brokerid":0,"timestamp":"1637036406085"}

// 카프카에 저장된 토픽 정보
ls /brokers/topics
[__consumer_offsets, hello.kafka, hello.kafka.2, verify-test]
```

*   카프카 클러스터로 묶인 브로커들은 동일한 경로의 주키퍼 경로로 선언해야

    같은 카프카 브로커 묶음이 된다.

{% hint style="info" %}
znode : 주키퍼에서 사용되는 저장 단위(znode간 계층 구조로, 트리 구조를 가질 수 있다.)

주키퍼에서 다수의 카프카 클러스터 사용은 서로 다른 znode에 카프카 클러스터들을 설정.
{% endhint %}



## 토픽과 파티션

{% hint style="info" %}
토픽(Topic) : 카프카에서 데이터를 구분하기 위해 사용하는 단위.(1개 이상의 파티션을 소유)

레코드(Record) : 파티션에 저장된 데이터.
{% endhint %}

* 파티션은 병렬처리의 핵심으로, 같은 그룹의 컨슈머들이 레코드를 병렬로 처리 할 수 있도록 매칭된다.
* 파티션은 큐(queue)와 비슷한 구조이지만, 데이터를 가져가도(pop) 삭제되지 않는다.

#### 토픽 이름 제약 조건

* 빈 문자열은 지원하지 않는다.
* 마침표 하나(.) 또는 마침표 둘(..)로 생성될 수 없다.
* 길이는 249자 미만.
* 영어 대소문자, 숫자, 마침표(.), 언더바(\_), 하이픈(-) 조합으로 생성 가능.
* 카프카 내부 로직 관리 목적으로 사용되는 2개 토픽(\_\_consumer\_offsets, \_\_transaction\_state)과 동일한 이름으로 생성 불가.
*   카프카 내부적으로 사용하는 로직 때문에 이름에 마침표와 언더바가 동시에 들어가면 안된다.

    (생성은 가능하지만 이슈발생을 유발. 사용 시 warning 메시지 발생)
*   이미 생성된 이름의 마침표를 언더바로 바꾸거나 언더바를 마침표로 바꾼 경우 신규 토픽 이름과 동일하다면 생성 할 수 없다.

    ex) to.pic 의 토픽이 있는 경우, to\_pic 생성 불가.

#### 토픽 작명 템플릿과 예시

*   <환경>.<팀명>.<애플리케이션-명>.<메시지-타입>

    ex) prd.marketing-team.sms-platform.json
*   <프로젝트-명>.<서비스-명>.<환경>.<이벤트-명>

    ex) commerce.payment.prd.notification
*   <환경>.<서비스-명>.\<JIRA-번호>.<메시지-타입>

    ex) dev.email-sender.jira-1234.email-vo-custom
*   <카프카-클러스터-명>.<환경>.<서비스-명>.<메시지-타입>

    ex) aws-kafka.live.marketing-platform.json

## 레코드

{% hint style="info" %}
레코드 : 타임스템프, 메시지 키, 메시지 값, 오프셋으로 구성.
{% endhint %}

* 브로커에 한번 적재된 레코드는 수정할 수 없고 로그 리텐션 기간 또는 용량에 따라서만 삭제.
*   타임스템프는 브로커 기준 유닉스 시간이 설정.

    프로듀서가 레코드를 생성할 때 임의의 타임스템프 값 설정 가능.

    카프카 0.10.0.0 버전 이상에서만 타임 스탬프 사용 가능.
* 메시지 키를 사용하면, 해쉬값을 토대로 파티션을 지정.(동일한 메시지 키라면 동일 파티션)
* 데이터의 송수신은 직렬화 - 역직렬화 필요.
* 오프셋은 카프카 컨슈머가 데이터를 가져갈때 사용.

## 카프카 클라이언트

* 클러스터에 명령을 내리거나 데이터를 송수신.

### 프로듀서 API

데이터의 시작점 : 프로듀서
