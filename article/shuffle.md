### 洗牌算法

方法a：
```php

<?php
$start = microtime_float();
$arr = [];
for ($i = 0; $i < 1000000; $i++) {
    $arr[] = $i;
}
function microtime_float()
{
    list($usec, $sec) = explode(" ", microtime());
    return ((float)$usec + (float)$sec);
}
// 超时 100w数据执行秒数
// 27.365822076797 10w数据执行秒数

function shuf($arr)
{
    $temp = [];
    $total = count($arr);
    for ($i = $total - 1; $i > -1; $i--) {

        $key = array_rand($arr); //返回随机元素的key，估计是拖累性能的元凶！
        $temp[] = $arr[$key];
        unset($arr[$key]);
    }

    return $temp;
}

$new = shuf($arr);
print_r($end - $start);

```


方法b：
```php
$start = microtime_float();
$arr = [];
for ($i = 0; $i < 1000000; $i++) {
    $arr[] = $i;
}
function microtime_float()
{
    list($usec, $sec) = explode(" ", microtime());
    return ((float)$usec + (float)$sec);
}
// 0.82493901252747 100w数据执行秒数
// 0.069424152374268 10w数据执行秒数

function shuf($arr){

    $len = count($arr);
    for($i=$len-1;$i>=0;$i--){
        $key = mt_rand(0, $i);

        if(isset($arr[$key])){
            $arr[] = $arr[$key];
            unset($arr[$key]);
        }else{
            $len += 1;
        }
    }
    return $arr;
}

$new = shuf($arr);
$end = microtime_float();

print_r($end - $start);
```

方法c：

```php
$start = microtime_float();
$arr = [];
for ($i = 0; $i < 1000000; $i++) {
    $arr[] = $i;
}
function microtime_float()
{
    list($usec, $sec) = explode(" ", microtime());
    return ((float)$usec + (float)$sec);
}

// 0.34066200256348 100w数据执行秒数
// 0.047982931137085 10w数据执行秒数
shuffle($arr);
$end = microtime_float();

print_r($end - $start);
```

###

### PHP原生方法

1. 打乱数组： shuffle
2. 打乱字符串： str_shuffle

### 性能对比

方法a，处理小数组勉强可以，数据量一大（百万级），效率非常低，无法使用。
方法b，速度很快，接近于原生shuffle，原理简单（随机将元素向数组末尾移动一遍），可以使用。
方法c，PHP内置shuffle方法效率最高，所以平时还是用这个方法吧。

> 相关：[对1千万条数据进行随机处理](./对1千万条数据进行随机处理.md)
