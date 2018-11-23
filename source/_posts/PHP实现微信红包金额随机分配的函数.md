---
title: PHP实现微信红包金额随机分配的函数
date: 2015-11-22 12:10:54
tags: 
  - PHP
  - 微信
  - Wechat
---
```php
<?php
/*
    参数请自己校验
    $money 准备发送多少钱(分)
    $n 个数
    $rate 控制红包的系数
*/
function makeRedPacket($money, $n, $rate = 0.5)
{
    //每个红包先保留1分钱
    $hold = $n;
    //分剩下的钱
    $remainder = $money - $hold;
    $result = [];
    for($i = 1; $i <= $n; $i++){
        //如果剩余的钱没有了就给1分钱
        if($remainder <= 0){
            $result[] = 1;
        }
        else{
            $max = floor($remainder * $rate);
            $rand = mt_rand(1, $max);
            //把保留的1分钱加进去
            $result[] = $rand + 1;
            //剩余的钱需要减去刚才发出去的
            $remainder -= $rand;
        }
    }
    //如果剩余的钱没有分配完直接给到第一个元素
    if($remainder > 0){
        $result[0] += $remainder;
    }
    //把数组随机打乱
    shuffle($result);
    return $result;
}
$result = makeRedPacket(1789, 8);
print_r($result);
$sum = array_sum($result);
print_r($sum);
```