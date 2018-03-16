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