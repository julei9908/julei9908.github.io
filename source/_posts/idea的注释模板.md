---
title: idea的注释模板
date: 2021-08-30 16:45:28
tags: idea
---

###### 类注释模板

```java
/**
 * 简述功能
 *
 * @author $user$
 * @since 1.0.0_$date$
 */
```

###### 方法注释模板

```java
*
 * 简述功能
 * $param$$return$
 * @author $user$
 * @since 1.0.0_$date$
 */
```

1. date的default value
   
   ```
   date("yyyy年MM月dd日")
   ```

2. param的default value
   
   ```groovy
   groovyScript("def result = '';def params = \"${_1}\".replaceAll('[\\\\[|\\\\]|\\\\s]', '').split(',').toList(); for(i = 0; i < params.size(); i++) {if(params[i] != '')result+='* @param ' + params[i] + ((i < params.size() - 1) ? '\\r\\n ' : '')}; return result == '' ? null : '\\r\\n ' + result", methodParameters())
   ```

3. return的default value
   
   ```groovy
   groovyScript("return \"${_1}\" == 'void' ? null : '\\r\\n * @return ' + \"${_1}\"", methodReturnType())
   ```

4. 名称必须设置成 *
