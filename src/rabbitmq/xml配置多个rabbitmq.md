# xml配置多个rabbitmq

💡 使用declared-by属性

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:rabbit="http://www.springframework.org/schema/rabbit"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans-3.0.xsd
    http://www.springframework.org/schema/rabbit
    http://www.springframework.org/schema/rabbit/spring-rabbit-1.2.xsd">

    <!-- 配置第一个连接工厂 -->
    <rabbit:connection-factory id="connectionFactory" addresses="localhost:5672" username="guest" password="guest"/>

    <!-- creates a rabbit template for convenient access to the broker -->
    <rabbit:template id="amqpTemplate" connection-factory="connectionFactory"/>

    <!-- creates a rabbit admin  used to manage exchanges, queues and bindings -->
    <rabbit:admin id="connectAdmin" connection-factory="connectionFactory"/>

    <!-- 声明队列 -->
    <rabbit:queue name="queueTest" durable="true" auto-delete="false" exclusive="false" declared-by="connectAdmin"/>

    <!-- 定义direct exchange，绑定queueTest -->
    <rabbit:direct-exchange name="exchangeTest" durable="true" auto-delete="false" declared-by="connectAdmin">
        <rabbit:bindings>
            <rabbit:binding queue="queueTest" key="queueTest"></rabbit:binding>
        </rabbit:bindings>
    </rabbit:direct-exchange>

    <!-- 定义消费者 -->
    <bean id="messageConsumer" class="com.example.consumer.MessageConsumer"></bean>

    <!-- 定义消费者监听的队列 -->
    <rabbit:listener-container connection-factory="connectionFactory">
        <rabbit:listener queues="queueTest" ref="messageConsumer"/>
    </rabbit:listener-container>

    <!-- 配置第二个连接工厂 -->
    <rabbit:connection-factory id="connectionFactory2" addresses="localhost:5672" username="guest" password="guest"/>

    <!-- creates a rabbit template for convenient access to the broker -->
    <rabbit:template id="amqpTemplate2" connection-factory="connectionFactory2"/>

    <!-- creates a rabbit admin  used to manage exchanges, queues and bindings -->
    <rabbit:admin id="connectAdmin2" connection-factory="connectionFactory2"/>

    <!-- 声明队列 -->
    <rabbit:queue name="queueTest2" durable="true" auto-delete="false" exclusive="false" declared-by="connectAdmin2"/>

    <!-- 定义direct exchange，绑定queueTest2 -->
    <rabbit:topic-exchange name="exchangeTest2" durable="true" auto-delete="false" declared-by="connectAdmin2">
        <rabbit:bindings>
            <rabbit:binding queue="queueTest2" pattern="queueTest2"></rabbit:binding>
        </rabbit:bindings>
    </rabbit:topic-exchange>

    <!-- 定义消费者 -->
    <bean id="messageConsumer2" class="com.example.consumer.MessageConsumer2"></bean>

    <!-- 定义消费者监听的队列 -->
    <rabbit:listener-container connection-factory="connectionFactory2" >
        <rabbit:listener queues="queueTest2" ref="messageConsumer2"/>
    </rabbit:listener-container>

</beans>
```