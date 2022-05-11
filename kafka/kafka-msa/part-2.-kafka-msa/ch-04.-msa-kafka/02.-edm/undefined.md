# 실습 코드



## 실습 코드

### **examples.txt**

```jsx
# mysql 설치
brew install mysql

# pip3 및 dependency 설치
curl <https://bootstrap.pypa.io/get-pip.py> -o get-pip.py
python3 get-pip.py
pip3 --version
pip3 install -r requirements.txt

# node js 설치 및 버전 확인
brew install node
npm --version
node --version
npm install mysql express

# /etc/hosts에 kafka1 추가

# 이벤트 채널 토픽 생성
./create_topics.py

# 인벤토리 테이블 생성
CREATE TABLE inventory (
    id int(10) NOT NULL AUTO_INCREMENT PRIMARY KEY,
    name char(30) NOT NULL,
    price int(10) NOT NULL,
    quantity int(10) NOT NULL
) ENGINE=InnoDB DEFAULT CHARSET=utf8;

CREATE TABLE inventory_history (
    id int(10) NOT NULL AUTO_INCREMENT PRIMARY KEY,
    transaction_id char(36) NOT NULL, // 사용자가 정상적인 주문을 넣었을 때, 발급
    inventory_id int(10) NOT NULL,
    quantity int(10) NOT NULL
) ENGINE=InnoDB DEFAULT CHARSET=utf8;

# 아이템 입력
INSERT INTO inventory(name, price, quantity) VALUES('cloth', 50000, 100);
INSERT INTO inventory(name, price, quantity) VALUES('cap', 30000, 100);
INSERT INTO inventory(name, price, quantity) VALUES('sunglasses', 25000, 100);
INSERT INTO inventory(name, price, quantity) VALUES('necklace', 150000, 100);
INSERT INTO inventory(name, price, quantity) VALUES('earring', 15000, 100);

# 주문 서비스 up
./order_service.py 

# 인벤토리 서비스 up
node inventory_service.js

# 인벤토리 서비스 컨슈머 up
./inventory_consumer.py

# 결제 서비스 컨슈머 up
./payment_consumer.py

# 주문 서비스 컨슈머 up
./order_consumer.py

// JSON
# 주문 요청
curl -v -XPOST <http://localhost:8080/v1/order> -H'Content-Type: application/json' \\
--data-binary @- << EOF
{
    "order": {
        "user_id": "user",
        "name": "Hong Gil Dong",
        "addr": "29, Hansil-ro, Dalseo-gu, Daegu, Republic of Korea",
        "tel": "01012341234",
        "email": "hong@gil.com"
    },
    "inventory": {
        "id": 2, 
        "quantity": 5
    }
} 
EOF

# 주문현황 조회
curl -v -XGET <http://localhost:8080/v1/order?transaction_id=${TRANSACTION_ID}&user_id=user>
```

### **create\_topics.py**

```jsx
#!/usr/bin/env python3
from confluent_kafka.admin import AdminClient, NewTopic

conf = {'bootstrap.servers': 'kafka1:19092'}
admin = AdminClient(conf=conf)
admin.create_topics([NewTopic('order', 1, 1), NewTopic('inventory', 1, 1), NewTopic('payment', 1, 1)])
print(admin.list_topics().topics)
```

### **docker-compose-edm.yml**

```jsx
version: '3'
services:
  zookeeper-1:
    hostname: zookeeper1
    image: confluentinc/cp-zookeeper:6.2.0
    environment:
      ZOOKEEPER_SERVER_ID: 1
      ZOOKEEPER_CLIENT_PORT: 12181
      ZOOKEEPER_DATA_DIR: /zookeeper/data
    ports:
      - 12181:12181
    volumes:
      - ./zookeeper/data/1:/zookeeper/data

  kafka-1:
    hostname: kafka1
    image: confluentinc/cp-kafka:6.2.0
    depends_on:
      - zookeeper-1
    environment:
      KAFKA_BROKER_ID: 1
      KAFKA_ZOOKEEPER_CONNECT: zookeeper1:12181
      KAFKA_ADVERTISED_LISTENERS: PLAINTEXT://kafka1:19092
      KAFKA_LOG_DIRS: /kafka
      KAFKA_AUTO_CREATE_TOPICS_ENABLE: "true"
      KAFKA_OFFSETS_TOPIC_REPLICATION_FACTOR: 1
    ports:
      - 19092:19092
    volumes:
      - ./kafka/logs/1:/kafka

// 주문 정보
  localstack-1:
    hostname: localstack1
    image: localstack/localstack:latest
    environment:
      AWS_DEFAULT_REGION: us-east-2
      EDGE_PORT: 4566
      SERVICES: dynamodb
      DATA_DIR: /tmp/localstack/dynamodb/data
    ports:
      - 4566:4566
    volumes:
      - ./localstack:/tmp/localstack

// 인벤토리 관련 정보
  mysql-1:
    hostname: mysql1
    image: mysql/mysql-server:5.7
    ports:
      - 3306:3306
    environment:
      MYSQL_USER: root
      MYSQL_ROOT_HOST: "%%"
      MYSQL_DATABASE: inventory
      MYSQL_ROOT_PASSWORD: inventorypw
    command: mysqld
      --server-id=1234
      --max-binlog-size=4096
      --binlog-format=ROW
      --log-bin=bin-log
      --sync-binlog=1
      --binlog-rows-query-log-events=ON
    volumes:
      - ./mysql:/var/lib/mysql
```

### **order\_service.py**

```jsx
#!/usr/bin/env python3
#-*- coding: utf-8 -*-

import uuid
import json
import logging
from datetime import datetime

from pynamodb.models import Model
from pynamodb.attributes import UnicodeAttribute, UTCDateTimeAttribute

// 웹 서버
from flask import Flask, request, jsonify

from confluent_kafka import Producer, Consumer

import sys
logger = logging.getLogger()
logger.setLevel(logging.DEBUG)
logger.addHandler(logging.StreamHandler(sys.stdout))

conf_producer = {'bootstrap.servers': 'kafka1:19092'}
conf_consumer = {'bootstrap.servers': 'kafka1:19092', 'group.id': 'cousumer_group', 'auto.offset.reset': 'earliest'}
producer = Producer(conf_producer)
consumer = Consumer(conf_consumer)

class OrderModel(Model):
    class Meta:
        table_name = 'order'
        host = '<http://localhost:4566>'
        region = 'us-east-2'

    transaction_id = UnicodeAttribute(hash_key=True)
    user_id = UnicodeAttribute(range_key=True)
    name = UnicodeAttribute(null=True)
    addr = UnicodeAttribute(null=True)
    tel = UnicodeAttribute(null=True)
    email = UnicodeAttribute(null=True)
    status = UnicodeAttribute(null=True)
    updated = UTCDateTimeAttribute()

if not OrderModel.exists():
    OrderModel.create_table(read_capacity_units=1, write_capacity_units=1, wait=True)

app = Flask(__name__)

@app.route('/v1/order', methods=['GET'])
def show_transaction():
    transaction_id = request.args.get('transaction_id')
    user_id = request.args.get('user_id')
    try:
        order = OrderModel.get(transaction_id, user_id)
        return jsonify(order.attribute_values)
    except OrderModel.DoesNotExist:
        return jsonify({'error': 'data not found'})

@app.route('/v1/order', methods=['POST'])
def create_order():
    entire_req = json.loads(request.data)
    req = entire_req['order']

		// 트랜잭션 ID 발급
    transaction_id = str(uuid.uuid1())
    user_id = req['user_id']
    name = req['name']
    addr = req['addr']
    tel = req['tel']
    email = req['email']

		// DB 저장
    model = OrderModel(transaction_id, user_id)
    model.updated = datetime.now()
    model.status = 'ORDER_CREATED'
    model.save()
		
    entire_req['transaction_id'] = transaction_id
    entire_req['status'] = model.status

		// 프로듀싱
    logging.debug("entire_req: %s" % entire_req) 
    producer.produce('inventory', value=json.dumps(entire_req))
    producer.flush()
    return jsonify({'status': model.status,
                    'user_id': user_id,
                    'transaction_id': transaction_id})    

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=8080, debug=True)
```

### **inventory\_consumer.py**

```jsx
#!/usr/bin/env python3
#-*- coding: utf-8 -*-

import json
import logging
import MySQLdb

from confluent_kafka import Producer, Consumer

import sys

logger = logging.getLogger()
logger.setLevel(logging.DEBUG)
logger.addHandler(logging.StreamHandler(sys.stdout))

conf_producer = {'bootstrap.servers': 'kafka1:19092'}
conf_consumer = {'bootstrap.servers': 'kafka1:19092', 'group.id': 'inventory_consumer_group', 'auto.offset.reset': 'earliest'}
producer = Producer(conf_producer)
consumer = Consumer(conf_consumer)

inbound_event_channel = 'inventory'
outbound_success_event_channel = 'payment'
outbound_failure_event_channel = 'order'

// mysql
db = MySQLdb.connect('127.0.0.1','root','inventorypw', 'inventory')

def process_inventory_reserved(event, transaction_id, inventory_id, quantity):
    logging.info('process_inventory_reserved')
    cursor = db.cursor()
    cursor.execute('UPDATE inventory SET quantity=quantity-%d WHERE id=%d' % (quantity, inventory_id))
    cursor.execute('INSERT INTO inventory_history(transaction_id, inventory_id, quantity) VALUES(\\'%s\\', %d, %d)' % (transaction_id, inventory_id, -1 * quantity))
    db.commit()
    cursor.execute('SELECT price FROM inventory WHERE id=%d' % inventory_id)
    record = cursor.fetchone()
    cursor.close()

    total_price = int(record[0]) * quantity
    logging.debug('total_price: %d' % total_price)
    event['payment'] = {'total_price': total_price}
    event['status'] = 'INVENTORY_RESERVED'
    producer.produce(outbound_success_event_channel, value=json.dumps(event))

def process_order_cancel(event, transaction_id, inventory_id, quantity):
    logging.info('process_order_cancel')
    cursor = db.cursor()
    cursor.execute('UPDATE inventory SET quantity=quantity+%d WHERE id=%d' % (quantity, inventory_id))
    cursor.execute('INSERT INTO inventory_history(transaction_id, inventory_id, quantity) VALUES(\\'%s\\', %d, %d)' % (transaction_id, inventory_id, quantity))
    db.commit()
    cursor.close()
    event['status'] = 'ORDER_CANCEL'
    producer.produce(outbound_failure_event_channel, value=json.dumps(event))
    

def main():
    try:
				// 구독 상태
        consumer.subscribe([inbound_event_channel])
        while True:
						// poll
            message = consumer.poll(timeout=3)
            if message is None:
                continue

            event = message.value()
            if event:
                logging.debug(event)
                evt = json.loads(event)
                status = evt['status']
                transaction_id = evt['transaction_id']
                inventory_id = int(evt['inventory']['id'])
                quantity = int(evt['inventory']['quantity'])

                if status == 'ORDER_CREATED':
                    process_inventory_reserved(evt, transaction_id, inventory_id, quantity)
                elif status == 'ORDER_CANCEL':
										// roleback
                    process_order_cancel(evt, transaction_id, inventory_id, quantity)
                else:
                    logging.error('unknown event')

            else:
                continue

    finally:
        consumer.close()    

if __name__ == '__main__':
    main()
```

### **inventory\_service.js**

```jsx
const express = require("express");
const mysql = require('mysql');
const app = express();

var db = mysql.createConnection({
  host: '127.0.0.1',
  user: 'root',
  password: 'inventorypw',
  database: 'inventory'
});

db.connect(function(error) {
  if (error) throw error;
  console.log('Database Connected.');
});

app.get('/v1/inventory', (request, response) => {
    db.query('SELECT * from inventory', (error, rows) => {
        if(error) throw error;
        db.end();
        console.log('=== rows ===', rows);
        response.send(rows);
    });
});

app.get('/v1/inventory_history', (request, response) => {
    db.query('SELECT * from inventory_history', (error, rows) => {
        if(error) throw error;
        db.end();
        console.log('=== rows ===', rows);
        response.send(rows);
    });
});

app.listen(8090, () => {
    console.log('8090 port listening...');
});
```

### **payment\_consumer.py**

```jsx
#!/usr/bin/env python3
#-*- coding: utf-8 -*-

import sys
import json
import logging

from confluent_kafka import Producer, Consumer

import sys

logger = logging.getLogger()
logger.setLevel(logging.DEBUG)
logger.addHandler(logging.StreamHandler(sys.stdout))

def process_consumer(is_pg_api_call_succeeded):
    conf_producer = {'bootstrap.servers': 'kafka1:19092'}
    producer = Producer(conf_producer)
    conf_consumer = {'bootstrap.servers': 'kafka1:19092', 'group.id': 'payment_consumer_group', 'auto.offset.reset': 'earliest'}
    consumer = Consumer(conf_consumer)

    inbound_event_channel = 'payment'
    outbound_success_event_channel = 'order'
    outbound_failure_event_channel = 'inventory'

    try:
				// 구독
        consumer.subscribe([inbound_event_channel])
        while True:
            message = consumer.poll(timeout=3)
            if message is None:
                continue

            event = message.value()
            if event:
                logging.debug(event)
                evt = json.loads(event)
								// 페이먼츠 성공여부
                if is_pg_api_call_succeeded == 'True':
                    evt['status'] = 'PAYMENT_COMPLETE'
                    producer.produce(outbound_success_event_channel, value=json.dumps(evt))
                else:
                    evt['status'] = 'ORDER_CANCEL'
                    producer.produce(outbound_failure_event_channel, value=json.dumps(evt))
            else:
                continue

    finally:
        consumer.close()    

if __name__ == '__main__':
    if len(sys.argv) != 2:
        print("Please put a required argument: is_pg_api_call_succeeded(True/False)")
        sys.exit()

    is_pg_api_call_succeeded = sys.argv[1]

    process_consumer(is_pg_api_call_succeeded)
```
