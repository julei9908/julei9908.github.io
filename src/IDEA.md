# IntelliJ IDEA

#### 注释模板

> File | Settings | Editor | Live Templates 新增Template Group、Live Template

##### 类模板

```java
/**
 * 简述功能
 *
 * @author $user$
 * @since $date$
 */
```

1. date设置default value为date("yyyy年MM月dd日")

2. 名称设置为c

##### 方法模板

```java
*
 * 简述功能
 * $param$$return$
 * @author $user$
 * @since $date$
 */
```

1. param设置default value
    
    ```groovy
    groovyScript("def result = '';def params = \"${_1}\".replaceAll('[\\\\[|\\\\]|\\\\s]', '').split(',').toList(); for(i = 0; i < params.size(); i++) {if(params[i] != '')result+='* @param ' + params[i] + ((i < params.size() - 1) ? '\\r\\n ' : '')}; return result == '' ? null : '\\r\\n ' + result", methodParameters())
    ```
    
2. return设置default value
    
    ```groovy
    groovyScript("return \"${_1}\" == 'void' ? null : '\\r\\n * @return ' + \"${_1}\"", methodReturnType())
    ```
    
3. 名称设置成 *

---

#### 控制台中文乱码

1. File | Settings | Editor | General | Console | Default Encoding 设置成UTF-8

2. VM options 增加 -Dfile.encoding=UTF-8