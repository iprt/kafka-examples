# kafka examples

spring boot kafka examples

## references

[reference](https://docs.spring.io/spring-kafka/reference/reference.html)

[Configuring Topics](https://docs.spring.io/spring-kafka/reference/kafka/configuring-topics.html)


## kafka docker compose

```yaml
services:
  kafka:
    image: 'bitnami/kafka'
    container_name: kafka-server
    user: root
    volumes:
      - "./data:/bitnami/kafka"
    ports:
      - '9092:9092'
      - '9093:9093'
    environment:
      # 允许使用kraft，即Kafka替代Zookeeper
      - KAFKA_ENABLE_KRAFT=yes
      - KAFKA_CFG_NODE_ID=1
      # kafka角色，做broker，也要做controller
      - KAFKA_CFG_PROCESS_ROLES=controller,broker
      # 定义kafka服务端socket监听端口（Docker内部的ip地址和端口）
      - KAFKA_CFG_LISTENERS=PLAINTEXT://:9092,CONTROLLER://:9093
      # 定义外网访问地址（宿主机ip地址和端口）ip不能是0.0.0.0
      - KAFKA_CFG_ADVERTISED_LISTENERS=PLAINTEXT://dev.iproute.org:9092
      # 定义安全协议
      - KAFKA_CFG_LISTENER_SECURITY_PROTOCOL_MAP=CONTROLLER:PLAINTEXT,PLAINTEXT:PLAINTEXT
      # 集群地址
      - KAFKA_CFG_CONTROLLER_QUORUM_VOTERS=1@kafka:9093
      # 指定供外部使用的控制类请求信息
      - KAFKA_CFG_CONTROLLER_LISTENER_NAMES=CONTROLLER
      # 设置broker最大内存，和初始内存
      - KAFKA_HEAP_OPTS=-Xmx1G -Xms1G
      # 使用Kafka时的集群id，集群内的Kafka都要用这个id做初始化，生成一个UUID即可(22byte)
      - KAFKA_KRAFT_CLUSTER_ID=xYcCyHmJlIaLzLoBzVwIcP
      # 允许使用PLAINTEXT监听器，默认false，不建议在生产环境使用
      - ALLOW_PLAINTEXT_LISTENER=yes
      # 不允许自动创建主题
      - KAFKA_CFG_AUTO_CREATE_TOPICS_ENABLE=false
      # broker.id，必须唯一，且与KAFKA_CFG_NODE_ID一致
      - KAFKA_BROKER_ID=1
```