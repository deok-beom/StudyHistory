메세지 큐(Message Queue)
===
## 메세지 큐(Message Queue)
- 메세지 큐(Message Queue, MQ)란 프로그램 간에 데이터를 교환할 때 사용하는 통신 방법 중에 하나로 더 큰 개념으로는 메세지 지향 미들웨어(Message Oriented Middleware, MOM)를 의미한다. 
    - MOM이란 비동기 메세지를 사용하는 프로그램 간의 데이터 송수신을 의미하는데 MOM을 구현한 시스템을 MQ라고 한다.
    - MQ는 작업을 늦출 수 있는 유연성을 제공한다.

### 메세지 브로커(Message Broker)
- 메세지 브로커는 publisher가 생산한 메세지를 메세지 큐에 저장하고, 저장된 데이터를 Consumer가 가져갈 수 있도록 중간 다리 역할을 해주는 브로커다.
흔히 S/W 관점에서 미들웨어라고 한다.
- 보통 서로 다른 시스템(혹은 소프트웨어 / 이기종간) 사이에서 데이터를 비동기 형태로 처리하기 위해 사용한다.
- 이러한 구조를 보통 pub/sub 구조라고 하며 대표적으로 RabbitMQ 소프트웨어가 있고 GCP의 pubsub, AWS의 SQS 같은 서비스가 있다.
- 이와 같은 메세지 브로커들은 Consumer가 큐에서 데이터를 가져가게 되면 즉시 혹은 짧은 시간 내에 큐에서 데이터가 삭제되는 특징이 있다.

### 이벤트 브로커
- 이벤트 브로커 또한 기본적으로 메세지 브로커의 큐 기능을 가지고 있어 메세지 브로커의 역할도 할 수 있다.
- 이벤트 브로커는 publisher가 생성한 이벤트를 저장하여, 후에 consumer가 특정 시점부터 이벤트를 다시 소비할 수 있다.
- 대용량 데이터 처리에 있어서 메세지 브로커보다는 더 많은 양의 데이터를 처리할 수 있는 능력이 있다.

### Advanced Message Queuing Protocol(AMQP)
- 메세지를 교환할 때 사용하는 프로토콜
- ISO 응용 계층의 MOM 표준으로 JMS(Java Message Service)와 비교된다.
    - JMS는 MOM을 자바에서 지원하는 표준 API이다.
    - JMS는 다른 Java Application간에 통신은 가능하지만 다른 MOM(AMQP, SMTP 등)끼리는 통신할 수 없다.
    - 그에 반해 AMQP는 Protocol만 일치한다면 다른 AMQP를 사용한 Application과도 통신이 가능하다.
    - AMQP는 write-protocol을 제공하는데 이는 octet stream을 이용해서 다른 네트워크 사이에 데이터를 전송할 수 있는 포맷이다.

### 메세지 큐의 장점
1. 비동기 : Queue에 넣기 때문에 나중에 처리할 수 있다.
2. 비동조 : Application과 분리할 수 있다.
3. 탄력성 : 일부가 실패한다고 해도 전체에 영향을 주지 않는다.
4. 과잉 : 실패할 경우 재실행이 가능하다.
5. 확장성 : 다수의 프로세스들이 큐에 메세지를 보낼 수 있다.

---
## MQ 대표 솔루션
1. kafka (event broker)
2. RabbitMQ (message broker)
3. ActiveMQ (message broker)

## Apache Kafka

### 특징
1. 카프카는 지향하는 바가 다른 메세지 큐와 다르다.
2. pub-sub 구조를 가진다.
3. 디스크에 메세지를 저장한다.
4. 분산 환경에 특화되도록 설계되었다.
5. 클러스터 구성, fail-over, replication과 같은 여러 특징을 가지고 있다.

### 구조와 주요 용어
- Topic : 메세지 종류, 특정한 데이터 스트림을 의미한다.
  - 토픽의 개수는 제한이 없고, 이름으로 구분된다.
  - topic이 다시 특정한 개수의 partition들이 된다. 개수의 명시가 필요하다
- Partition : Topic이 나눠지는 단위이다.
  - 유한한 시간(기본 1주)동안 디스크에 저장되어 있다.
  - 파티션에 한 번 쓰여진 데이터는 변경이 불가능하다.
  - 토픽을 구성하는 파티션은 정렬되어 있지만 쓰여지는 데이터는 키 값이 제공되지 않으면 파티션에 랜덤하게 할당된다.
  - 각 파티션 속 메세지는 오름차순 ID 값인 Offset을 가지고 있다.
- Offset : 속해 있는 파티션에서만 의미 있는 값으로 파티션 내에서 각 메세지가 가지는 고유 ID이다.
  - 순서는 속해 있느 파티션에서만 보장된다.
  - 다른 파티션에서 동일한 Offset이 가리키는 데이터는 같지 않다.


