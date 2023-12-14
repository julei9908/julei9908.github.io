# rabbitmq消费者确认

#### 配置

```java
spring.rabbitmq.listener.direct.acknowledge-mode=manual
spring.rabbitmq.listener.simple.acknowledge-mode=manual
```

#### 监听方法

```java
@RabbitListener(queues = "queueName")
public void manual(String in, Channel channel, @Header(AmqpHeaders.DELIVERY_TAG) long tag) {
    ...
    // 业务处理成功后调用，消息会被确认消费
    channel.basicAck(tag, false);
    // 业务处理失败后调用, 第二个参数非批量确定，第三个参数表示消息重新入队列
    channel.basicNack(tag, false, true);
    // 业务处理失败后调用, 第二个参数表示消息重新入队列
    channel.basicReject(tag, true);
}
```