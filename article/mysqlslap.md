## mysqlslap -- 负载仿真客户端

MySqLSLAP是一个诊断程序，用于模拟MySQL服务器的客户端负载，并报告每个阶段的时序。它像多个客户端正在访问服务器一样工作。

### mysqlslap 使用场景：

1. Create schema, table, and optionally any stored programs or data to use for the test. This stage uses a single client connection.
2. Run the load test. This stage can use many client connections.
3. Clean up (disconnect, drop table if specified). This stage uses a single client connection.

### 示例：

```sh
#### 提供你的 create 和 query sql语句，每次有 50 个客户端执行 querying 和 200 个客户端执行 selects 。 (enter the command on a single line):

mysqlslap --delimiter=";"
  --create="CREATE TABLE a (b int);INSERT INTO a VALUES (23)"
  --query="SELECT * FROM a" --concurrency=50 --iterations=200


#### 让MySQLSLAP用两个int列和三个VARCHAR列组成一个查询SQL语句。使用五个客户机查询20次。不要创建表或插入数据（也就是说，使用以前的测试的模式和数据）：

mysqlslap --concurrency=5 --iterations=20
  --number-int-cols=2 --number-char-cols=3
  --auto-generate-sql


#### 告诉程序从指定文件中加载create、insert和query SQL语句，其中create.sql文件有多个以“;”分隔的表创建语句，以及多个以“;”分隔的insert语句。查询文件将有多个由“；”分隔的查询。运行所有的加载语句，然后运行五个客户端（五次）的查询文件中的所有查询：

mysqlslap --concurrency=5
  --iterations=5 --query=query.sql --create=create.sql
  --delimiter=";"

```
