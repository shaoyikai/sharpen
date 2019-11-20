#### MySQL启动时，它会在哪些位置查找配置文件：

`mysql --help | findstr my.cnf`

#### 查看支持的存储引擎

mysql> `show engines\G`

#### 通过socket方式连接mysql，（server在本机的情况下才行）

```php
<?php

$link = mysql_connect('127.0.0.1:3306:E:/xampp/mysql/mysql.sock', '', '');
// $link = mysql_connect('localhost', 'root', '');
if (!$link) {
    die('Could not connect: ' . mysql_error());
}
echo 'Connected successfully';
mysql_close($link);
```


#### 慢查询日志

```sh
# 是否开启慢查询
show variables like 'log_slow_queries';
# 查看慢查询设置的时间阈值long_query_time
show variables like '%long%';
# 查看哪些语句没有使用索引
show variables like 'log_queries_not_using_indexes';

# 使用mysqldumpslow工具
mysqldumpslow nh122-190-slow.log
```

#### 查询mysql占用空间

```sql
select TABLE_SCHEMA, concat(truncate(sum(data_length)/1024/1024,2),' MB') as data_size,
concat(truncate(sum(index_length)/1024/1024,2),'MB') as index_size
from information_schema.tables
group by TABLE_SCHEMA
order by data_length desc;
```

#### 查询mysql数据量大的表

```sql
SELECT TABLE_NAME,TABLE_ROWS,INDEX_LENGTH,DATA_LENGTH FROM information_schema.TABLES 
WHERE TABLE_SCHEMA = 'noc' order by TABLE_ROWS desc limit 10;
```
