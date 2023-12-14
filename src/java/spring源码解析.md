# spring源码解析

### 源码（3.2.x）编译问题

#### 替换build.gradle仓库地址

```
repositories {
    // maven { url "https://repo.spring.io/plugins-release" }
    maven { url "https://maven.aliyun.com/repository/spring-plugin" }
}

repositories {
    // maven { url "https://repo.spring.io/libs-release" }
    maven { url "http://maven.aliyun.com/nexus/content/groups/public" }
}
```

---

### IOC容器的初始化

IOC容器的初始化是由refresh()方法来启动的，这个方法标志着IOC容器的正式启动。具体来说，这个启动包括BeanDefinition的Resouce定位、载入和注册三个基本过程

#### Resource定位过程

Resource定位指的是BeanDefinition的资源定位，它由ResourceLoader通过统一的Resource接口来完成，这个Resource对各种形式的BeanDefinition的使用都提供了统一接口。比如，在文件系统中的Bean定义信息可以使用FileSystemResource来进行抽象；在类路径中的Bean定义信息可以使用前面提到的ClassPathResource来使用等等。这个定位过程类似于容器寻找数据的过程，就像用水桶装水先要把水找到一样

#### BeanDefinition的载入

这个载入过程是把用户定义好的Bean表示成IoC容器内部的数据结构，而这个容器内部的数据结构就是BeanDefinition。具体来说，这个BeanDefinition实际上就是POJO对象在IoC容器中的抽象，通过这个BeanDefinition定义的数据结构，使IoC容器能够方便地对POJO对象也就是Bean进行管理

#### BeanDefinition向IoC容器注册

把载入过程中解析得到的BeanDefinition向IoC容器进行注册，这个过程是通过调用BeanDefinitionRegistry接口的实现来完成的。IoC容器内部将BeanDefinition注入到一个HashMap中去，IoC容器就是通过这个HashMap来持有这些BeanDefinition数据的

---

### BeanDefinition在IOC容器中的注册

BeanDefinition在IoC容器中载入和解析完成以后，用户定义的BeanDefinition信息已经在IoC容器内建立起了自己的数据结构以及相应的数据表示，但此时这些数据还不能供IoC容器直接使用，需要在IoC容器中对这些BeanDefinition数据进行注册。这个注册为IoC容器提供了更友好的使用方式，在DefaultListableBeanFactory中，是通过一个HashMap来持有载入的BeanDefinition的，这个HashMap的定义在DefaultListableBeanFactory中可以看到

```java
/** Map of bean definition objects, keyed by bean name */
private final Map<String, BeanDefinition> beanDefinitionMap = new ConcurrentHashMap<String, BeanDefinition>(64);
```

在DefaultListableBeanFactory中实现了BeanDefinitionRegistry的接口，这个接口的实现完成BeanDefinition向容器的注册。这个注册过程不复杂，就是把解析得到的BeanDefinition设置到hashMap中去。需要注意的是，如果遇到同名的BeanDefinition，进行处理的时候需要依据allowBeanDefinitionOverriding的配置来完成

```java
public void registerBeanDefinition(String beanName, BeanDefinition beanDefinition) throws BeanDefinitionStoreException {

    Assert.hasText(beanName, "Bean name must not be empty");
    Assert.notNull(beanDefinition, "BeanDefinition must not be null");

    if (beanDefinition instanceof AbstractBeanDefinition) {
        try {
            ((AbstractBeanDefinition) beanDefinition).validate();
        } catch (BeanDefinitionValidationException ex) {
            throw new BeanDefinitionStoreException(beanDefinition.getResourceDescription(), beanName, "Validation of bean definition failed", ex);
        }
    }

    BeanDefinition oldBeanDefinition;
    //注册的过程需要synchronized，保证数据的一致性
    synchronized (this.beanDefinitionMap) {
        /*
            这里检查是不是有相同名字的BeanDefinition已经在IoC容器中注册了
             如果有相同名字的BeanDefinition，但又不允许覆盖，那么会抛出异常
        */
        oldBeanDefinition = this.beanDefinitionMap.get(beanName);
        if (oldBeanDefinition != null) {
            if (!this.allowBeanDefinitionOverriding) {
                throw new BeanDefinitionStoreException(beanDefinition.getResourceDescription(), beanName,
                        "Cannot register bean definition [" + beanDefinition + "] for bean '" + beanName +
                                "': There is already [" + oldBeanDefinition + "] bound.");
            } else {
                if (this.logger.isInfoEnabled()) {
                    this.logger.info("Overriding bean definition for bean '" + beanName +
                            "': replacing [" + oldBeanDefinition + "] with [" + beanDefinition + "]");
                }
            }
        }
        /*
            这是正常注册BeanDefinition的过程，把Bean的名字存入到beanDefinitionNames的同时，把beanName作为Map的key,把
            beanDefinition作为value存入到IoC容器持有的beanDefinitionMap中去
        */
        else {
            this.beanDefinitionNames.add(beanName);
            this.frozenBeanDefinitionNames = null;
        }
        this.beanDefinitionMap.put(beanName, beanDefinition);
    }

    if (oldBeanDefinition != null || containsSingleton(beanName)) {
        resetBeanDefinition(beanName);
    }
}
```