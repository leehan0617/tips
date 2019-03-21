### 카프카 로컬에서 실행해보기 (mac os 기준)

0. brew로 kafka를 설치한다. (현재기준 kafka 버전은 2.1.1이다.)
1. 우선 실행을 눈으로 보기 편하게 terminal 4개를 켠다.
* zookeeper 서버용, kafak 서버용, producer console, consumer console
2. brew로 설치를 했다면 kafka가 설치된 디렉토리 경로는 /usr/local/Cellar/kafka/{version} 이 될것이다.
3. 실행스크립트는 kafka경로/libexec/bin에 있고, 환경변수들은 kafka경로/libexec/config 에 있다.
4. kafka를 실행하기 위해서는 zookeeper 서버가 먼저 실행되어야한다. (zookeeper는 kafka를 설치하면 안에 내장되어있는것 같다.)
5. kafka경로/libexec 디렉토리에 들어가서 
```
./bin/zookeeper-server-start.sh config/zookeeper.properties 
```
위와 같이 실행하면 맨 마지막에 2181포트에 binding이 되었다고 콘솔에 출력된다. (2181은 기본포트)
내 기준 아래와 같이 출력됬다.
```
INFO binding to port 0.0.0.0/0.0.0.0:2181 (org.apache.zookeeper.server.NIOServerCnxnFactory)
```
6. kafka를 실행한다. 
```
./bin/kafka-server-start.sh config/kafka.properties
```
로그가 출력되면서 Starting이라는 로그를 확인하면 정상실행 된것이다. (실행될때 zookeeper와 연결을 하므로 반드시 zookeeper를 먼저 실행하자.)
7. sample topic을 생성한다. (메세지를 주고받을 채팅방같은거라 생각하면 될듯)
```
./bin/kafka-topics.sh -create -zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic test
```
위 스크립트는 test라는 topic 을 생성하는 예제이다.
정상적으로 topic이 만들어졌다면 topic 리스트를 확인해본다.
```
./bin/kafka-topics.sh --list --zookeeper localhost:2181
```
8. producer & consumer을 터미널로 띄운다.
```
# producer
./bin/kafka-console-producer.sh --broker-list localhost:9092 --topic test
# consumer
./bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic test --from-beginning
```
다른 블로그나 구글검색을 통해 예제를 보면 consumer실행할때 --bootstrap-server 대신 -zookeeper를 사용하는데, -zookeeper 옵션이 deprecated 된거 같다.
현재 버전에서는 --bootstrap-server property를 사용하자.<br>
9. 둘다 띄운상태에서 producer에 메세지를 입력하고 consumer에 정상출력되는걸 확인하면 끝.
