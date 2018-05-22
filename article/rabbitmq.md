1. centos系统安装RabbitMQ

`yum install erlang`

2. 安装 RabbitMQ Server （CentOS 7）

`yum list rabbitmq-server`

`yum install rabbitmq-server`

3. 运行 RabbitMQ Server

启动服务

`chkconfig rabbitmq-server on`

启动

`service rabbitmq-server start`

停止

`service rabbitmq-server stop`


4. Managing the Broker

- Invoke `rabbitmqctl stop` to stop the server.
- Invoke `rabbitmqctl status` to check whether it is running.

5. 配置

`/etc/rabbitmq/rabbitmq.config`

查看当前配置信息

`rabbitmqctl environment`

### 消息队列的使用场景

** 异步处理 ** 

注册邮件发送，注册短信。

** 应用解耦 **

订单系统：用户下单后，订单系统完成持久化处理，将消息写入消息队列，返回用户订单下单成功

库存系统：订阅下单的消息，采用拉/推的方式，获取下单信息，库存系统根据下单信息，进行库存操作

假如：在下单时库存系统不能正常使用。也不影响正常下单，因为下单后，订单系统写入消息队列就不再关心其他的后续操作了。实现订单系统与库存系统的应用解耦


** 流量削锋 **

秒杀

** 日志处理 **

** 消息通讯 **

点对点消息队列：客户端A和客户端B使用同一队列，进行消息通讯。

聊天室：客户端A，客户端B，客户端N订阅同一主题，进行消息发布和接收。实现类似聊天室效果。


### RabbitMQ 使用举例

1. “Hello World!”

普通模式，一个 producer 和一个 consumer 通过特定的 quene 进行通信

服务端
```php
<?php
// receive.php
require_once __DIR__ . '/vendor/autoload.php';
use PhpAmqpLib\Connection\AMQPStreamConnection;

$connection = new AMQPStreamConnection('localhost', 5672, 'guest', 'guest');
$channel = $connection->channel();

$channel->queue_declare('hello', false, false, false, false);

echo ' [*] Waiting for messages. To exit press CTRL+C', "\n";

$callback = function($msg){
        echo " [x] Received ". $msg->body. "\n";
};

$channel->basic_consume('hello','',false,true,false,false, $callback);

while(count($channel->callbacks)){
        $channel->wait();
}
```

客户端
```php
<?php
// send.php
require_once __DIR__ . '/vendor/autoload.php';
use PhpAmqpLib\Connection\AMQPStreamConnection;
use PhpAmqpLib\Message\AMQPMessage;

$connection = new AMQPStreamConnection('localhost', 5672, 'guest', 'guest');
$channel = $connection->channel();

$channel->queue_declare('hello',false,false,false,false);

$msg = new AMQPMessage('Hello World!');
$channel->basic_publish($msg, '' ,'hello');

echo " [x] Sent 'Hello World!'\n";

$channel->close();
$connection->close();

```

- `php receive.php` # 在A窗口输入
- `php send.php` # 在B窗口输入

2. Work queues

一个 producer 多个 consumers （轮询处理）

This concept is especially useful in web applications where it's impossible to handle a complex task during a short HTTP request window.

producer

```php
<?php
// new_task.php
require_once __DIR__ . '/vendor/autoload.php';
use PhpAmqpLib\Connection\AMQPStreamConnection;
use PhpAmqpLib\Message\AMQPMessage;

$connection = new AMQPStreamConnection('localhost', 5672, 'guest', 'guest');
$channel = $connection->channel();

$channel->queue_declare('hello',false,false,false,false);

$data = implode(' ', array_slice($argv,1));
if(empty($data)) $data = "Hello World!";
$msg = new AMQPMessage($data);
$channel->basic_publish($msg, '' ,'hello');

echo " [x] Sent 'Hello World!'\n";

$channel->close();
$connection->close();

```

consumer

```php
<?php
// work.php
require_once __DIR__ . '/vendor/autoload.php';
use PhpAmqpLib\Connection\AMQPStreamConnection;

$connection = new AMQPStreamConnection('localhost', 5672, 'guest', 'guest');
$channel = $connection->channel();

$channel->queue_declare('hello', false, false, false, false);

echo ' [*] Waiting for messages. To exit press CTRL+C', "\n";

$callback = function($msg){
        echo " [x] Received ". $msg->body. "\n";
        sleep(substr_count($msg->body, '.'));
        echo " [x] Done", "\n";
};

$channel->basic_consume('hello','',false,true,false,false, $callback);

while(count($channel->callbacks)){
        $channel->wait();
}

```

运行结果

![Alt text](/images/2018/rb001.png "运行结果")