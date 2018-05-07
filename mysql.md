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


----------------------------------------------------
docker gui