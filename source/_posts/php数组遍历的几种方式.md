---
title: php数组遍历的几种方式
date: 2018-07-30 16:17:09
tags:
    - 数组遍历
    - for foreach each list
    - php
category: php

---

第一种: for()循环,只能遍历纯索引数组 
索引数组-> 带有数字索引的,没有指定"键"的 [1,2,3]
关联数组-> 带有"键"的 ['a'=>1,'b'=>2]

<!-- more -->

```
    $arr=array(1,2,3,4,5,6);
    $arr=[1,2,3,4,5,6];
    $num=count($arr);
    for($i=0;$i<$num;$i++){
        echo $arr[$i].'<br>';
    }//count()最好放在for循环外面,可以让函数只执行一次

```


第二种: foreach()
```
$arr1=[
    "a"=>[
        ["name"=>"李四","age"=>"12"],
        ["name"=>"王四","age"=>"12"]
    ],
    "b"=>[
        ["name"=>"张三","age"=>"12"]
    ]
];
foreach ($arr1 as $key=>$value){
    print_r($value);
    echo $key.'班:<br>';
    foreach ($value as $key1=>$value1){
        echo '第'.($key1+1).'个同学:<br>';
        foreach ($value1 as $key2=>$value2){
            echo $key2.'=>'.$value2.'<br>';
        }
    }
}
```



第三种: list()、each()、while()遍历数组
注意1: list()只解析索引数组

```
$arr2=[2,3,"a"=>[0,9],4]; 
$arr2=[2,3,4];
list($a,$b,$c)=$arr2;
echo 'a->'.$a.'<br>'.'b->'.$b.'<br>'.'c->'.$c.'<br>';

```
注意2: list()可以通过空参数,选择性的解析数组的值 
```
list($a,,$b)=[1,2,3];
echo 'a->'.$a.'<br>'.'b->'.$b.'<br>';
```
each()用于返回数组当前指针所在位的键值对,并将指针后移一位,
返回值：如果指针有下一位,返回一个数组,包含一个索引数组(0-键,1-值)和一个关联数组("key"-键,"value"-值),
Array ( [1] => 456 [value] => 456 [0] => b [key] => b )
如果指针没有下一位,返回false,    
数组使用each()遍历完一遍后,指针使用处于最后一位的下一位,即再用each(),始终返回false
如果还需使用,需用reset($arr)函数,重置数组指针
```
    $arr3=['c','d'];
    while($a = each($arr3)){
        echo "{$a[0]}-->{$a[1]}<br>";  
        echo "{$a['key']}---->{$a['value']}<br>";  
        print_r($a);
    } 
    reset($arr3);
```
结果:
```
    Array ( [1] => c [value] => c [0] => 0 [key] => 0 ) 
    Array ( [1] => d [value] => d [0] => 1 [key] => 1 )
```
```
    $indexData=['a'=>'123','b'=>'456'];
    while (list($key, $value) = each($indexData)) {
        echo '<br>'.$key.'->'.$value;
    }
```


第四种:数组指针遍历数组
next：将数组指针,后移一位 并返回后一位的值,空数组返回false
prev：将数组指针,前移一位 并返回前一位的值
end：将数组指针,移至最后一位,返回最后一位的值
reset:将数组指针,恢复到第一位 并返回第一位的值
key: 返回当前指针所在位的键,
current:返回当前指针所在位的值,

next举例: 
```
    $arr4 = [5,6,7,8,"one"=>9];
    while(true){
        echo '<br>'.key($arr4).'->'.current($arr4).'<br>';
        if(!next($arr4)){
            break;
        }
    }
    reset($arr4);
```

prev举例:
```    
    end($arr4);
    do{
        echo '<br>'.key($arr4)."=>".current($arr4)."<br>";
    }while(prev($arr4));
```