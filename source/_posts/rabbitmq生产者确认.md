---
title: rabbitmq生产者确认
date: 2021-11-05 16:11:59
tags: rabbitmq
---

###### 添加配置

```properties
spring.rabbitmq.publisher-confirm-type=correlated
spring.rabbitmq.publisher-returns=true
spring.rabbitmq.template.mandatory=true
```

###### rabbitTemplate方法配置

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

###### [spring-rabbit-confirms-returns](https://github.com/spring-projects/spring-amqp-samples/tree/main/spring-rabbit-confirms-returns)