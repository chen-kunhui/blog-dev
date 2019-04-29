---
title: shell 脚本简记
mathjax: false
date: 2019-04-27 18:00:34
tags:
categories:
  - linux
  - shell
---

# shell 特殊变量

    $$    当前进程的pid
    $!    最近一个后台任务的pid
    $?    上一条命令的退出码
    $*    参数列表，如："$1 $2 .."
    $@    参数列表，如："$1" "$2" ..
    $#    参数个数
    $0    当前脚本文件名
    $1~$n 脚本传入的第n个参数

# shell 中定义本地变量，local 关键字

```sh
#!/usr/bin/env bash

a=1

# 这里的 a 与外面的 a 共用
fun1(){
  echo $a #=> 1
  a=2     # 这里会同时改变外部的 a
  echo $a #=> 2
}

# 这里的 a 将只在 fun2 中有效，与外面的 a 相互独立
fun2(){
  local a
  echo $a #=> ''
  a=2     # 这里不会改变外部的 a
  echo $a #=> 2
}

fun1
echo $a

a=1
fun2
echo $a
```

# for 循环

```sh
#!/usr/bin/env bash

for((i=0;i<10;i++));do
  echo $i
done
```

# while 循环读取文件

```sh
#!/usr/bin/env bash

while read line; do
  if [[ $line -ne 0 ]]; then
    return $line
  fi
done < file_ptah
```

# if 分支控制

```sh
#!/usr/bin/env bash

if [[ 条件，或命令，非0退出则为false ]];then
  ...
elif [[ 条件，或命令，非0退出则为false ]];then
  ...
else
  ...
fi
```

# case

```sh
#!/usr/bin/env bash

case $变量名 in
"值 1")
  ....
  ;;
"值 2")
  ....
  ;;
*)
  如果变量的值都不是以上的值，则执行此程序
  ;;
esac
```

# 数组

```sh
#!/usr/bin/env bash

arr=(a b "c" d)

# 取值
echo ${arr[0]}  #=> a
echo ${arr[2]}  #=> c
echo ${arr[55]} #=> ''

# 赋值
arr[55]=f
echo ${arr[55]} #=> f

echo "数组长度: ${#arr[@]}"     #=> 数组长度: 5
echo "数组长度: ${#arr[*]}"     #=> 数组长度: 5
echo "数组的元素为: ${arr[@]}"  #=> 数组的元素为: a b c d f
echo "数组的元素为: ${arr[*]}"  #=> 数组的元素为: a b c d f
echo "数组下标: ${!arr[@]}"    #=>数组下标: 0 1 2 3 55

#也可以写成for element in ${array[*]}
for element in ${arr[@]};do
  echo $element
done

# 类hash
arr[g]=88
echo ${arr[g]} #=> 88
```