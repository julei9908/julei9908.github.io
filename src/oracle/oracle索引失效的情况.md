# oracle索引失效的情况

#### 没有 WHERE 子句

#### 使用 IS NULL 或 IS NOT NULL

```sql
SELECT * FROM EMP WHERE EMP_ID IS NULL
```

#### WHERE子句中使用函数

```sql
SELECT * FROM STAFF WHERE TRUNC(BIRTHDATE) = '2021-01-01 00:00:00'
```

如果没有使用基于函数的索引，那么where子句中对存在索引的列使用函数时，会使优化器忽略掉这些索引，但是把函数应用在条件上，索引是可以生效的，把上面的语句改成下面的语句，就可以通过索引进行查找

```sql
SELECT * FROM STAFF WHERE BIRTHDATE = TO_DATE('2021-01-01 00:00:00', 'yyyy-mm-dd hh24:mi:ss')
```

💡 注意：对于 MIN, MAX函数，仍然使用索引

#### 使用LIKE '%T%'进行模糊查询

#### WHERE子句中使用不等于操作

💡 不等于操作包括：<> != 这个限制条件可以通过or替代，例如：colum <> 0 替换成colum > 0 or colum < 0

#### 等于和范围索引不会被合并使用

```sql
SELECT * FROM EMP WHERE JOB='MANAGER' AND DEPTNO > 10
```

JOB和DEPTNO都是非唯一索引，这种条件下oracle不会合并索引，它只会使用第一个索引

#### 比较不匹配数据类型

```sql
SELECT * FROM DEPT WHERE DEPT_ID = 900198
```

DEPT_ID是一个VARCHAR2类型的字段，在这个字段上有索引，但是上面的语句会执行全表扫描。这是因为 oracle会自动把where子句转换成TO_NUMBER(DEPT_ID) = 900198，相当于使用函数，这样就限制了索引的使用