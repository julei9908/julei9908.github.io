# rabbitmq-direct默认交换机

如果用空字符串去声明一个exchange，那么系统就会使用AMQP default这个exchange，创建一个queue时,默认的都会有一个和新建queue同名的routingKey绑定到默认的exchange

```java
@Bean
public Queue defaultQueue() {
    return new Queue(Constants.DEFALUT_QUEUE);
}
```