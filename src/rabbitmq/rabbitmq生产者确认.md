# rabbitmq生产者确认

#### 配置

```
spring.rabbitmq.publisher-confirm-type=correlated
spring.rabbitmq.publisher-returns=true
spring.rabbitmq.template.mandatory=true
```

#### rabbitTemplate配置

```java
@Bean
public RabbitTemplate rabbitTemplate(ConnectionFactory connectionFactory) {
    RabbitTemplate rabbitTemplate = new RabbitTemplate(connectionFactory);
    // 消息发送到交换机回调
    rabbitTemplate.setConfirmCallback((correlationData, ack, cause) -> {
        if (!ack) {
            LOG.warn(String.format("消息发送失败，cause：%s，id：%s", cause, correlationData.getId()));
        }
    });
    // 消息未路由到队列回调
    rabbitTemplate.setReturnCallback((message, replyCode, replyText, exchange, routingKey) -> {
        LOG.warn(String.format("消息路由失败，message：%s，replyCode：%s，replyText：%s", message.toString(), replyCode, replyText));
    });
    return rabbitTemplate;
}
```

#### [官网示例](https://github.com/spring-projects/spring-amqp-samples/tree/main/spring-rabbit-confirms-returns)