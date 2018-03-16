### 洗牌算法

```php

<?php

$arr = [1, 2, 3, 4, 5, 6, 7, 8, 9];

function shuf($arr)
{
    $temp = [];
    $total = count($arr);
    for ($i = $total - 1; $i > -1; $i--) {

        $key = array_rand($arr); //返回随机元素的key
        $temp[] = $arr[$key];
        unset($arr[$key]);
    }

    return $temp;
}

print_r(shuf($arr));die;

```

执行结果：

```
Array
(
    [0] => 2
    [1] => 9
    [2] => 3
    [3] => 7
    [4] => 6
    [5] => 4
    [6] => 5
    [7] => 1
    [8] => 8
)
```

### PHP原生方法

1. 打乱数组： shuffle
2. 打乱字符串： str_shuffle

### 性能

shuffle原生方法效率非常高，所以平时还是用这个方法吧。
上面的shuf方法处理小数组可以，数据量一大（百万级），效率非常低，无法使用。

> 相关：[对1千万条数据进行随机处理](./对1千万条数据进行随机处理.md)
