# rabbitmq-fanout绑定动态队列

#### 获取本机ip地址

```java
public static String getIp() {
    return InetAddress.getLocalHost().getHostAddress();
}
```

#### 创建队列

```java
private static final String QUEUE_NAME = getIp();

@Bean
public Queue queue() {
    return new Queue(QUEUE_NAME, true, false, true);
}
```

#### 使用SPEL表达式获取队列名

```java
@RabbitListener(queues = "#{queue.name}")
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