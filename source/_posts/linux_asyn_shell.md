---
title: shell 模拟并发
mathjax: false
date: 2019-04-27 14:01:49
tags:
categories:
  - linux
  - shell
---

在一个 shell 脚本中，通过用 & 的方式将一条命令或一个方法放到后台执行，相当于又起来一个子进程
在其启动子进程的后面紧接用变量 $! 可以获得该子任务进程的 pid
脚本中可以通过 wait 方式，等待该进程执行完才推出，从而在shell脚步中实现并发

wait 不加参数 相当于等待所有子进程结束，wait + 子进程 pid 相当于等待某个子进程结束

注意，shell 脚步退出了，他的子进程并不会跟着推出，因为他们之间是独立的进程
同样在子任务中调用 exit 退出子任务，并不会影响 shell 脚步继续执行

PID是程序被操作系统加载到内存成为进程后动态分配的资源。
PPID（parent process ID）：PPID是程序的父进程号。

父进程退出并不会通知子进程推出，子进程会被pid 为1 的init进程领养

过多的子进程是会拖慢运行速度的

shell 模拟并发脚本:
> note: 在脚本中如果强制父进程退出了，子进程如果还在运行并不会退出

```sh
#!/usr/bin/env bash

start_time=$(date +%s)
root_path=$(pwd)

# 子进程的退出状态码统一保存的文件
RESULT_FILE="${root_path}/${0}.result.log"

# 避免有残留数据
rm -rf $RESULT_FILE

# 启动一个子进程运行命令，实现并发
run_in_asyn(){
  run_cmd $@ &
}

# 将命令的执行结果保存到一个文件中
run_cmd(){
  local cmd=$@
  eval $cmd
  echo $? >> $RESULT_FILE
}

# 判断子进程是否有执行失败的
is_success(){
  local result_file=$1
  while read line; do
    if [[ $line -ne 0 ]]; then
      return $line
    fi
  done < $result_file
  return 0
}

# 实例
test(){
  for((i=0;i<5;i++));do
    sleep 1
    echo $1
  done
}

run_in_asyn "test t1"
run_in_asyn "test t2"
run_in_asyn "rr"

# 实例 end

wait

is_success $RESULT_FILE
# 通过 $? 获取上一个命令的返回结果，即 is_success 的返回值
exit_code=$?

# 清理临时创建的文件
rm -rf $RESULT_FILE

# 输出脚本的耗时
end_time=$(date +%s)
echo "run $0 duration: $((end_time - start_time))s"

exit $exit_code
```
