---
title: shell基本知识
date: 2021-11-08 19:42:44
tags:
- 工作
- 技术
- shell
categories: 技术
description: shell入门
photos:
---

# var 
shell_var='lover'  
如果使用  
`${shell_var}  `  
赋值+字符串拼接  
shell_var="${shell_var}abcd"

# shell 循环

```shell
shell_var='aaa'
for((i=1;i<10;i++));
do
...
done
```

# 传参

第n个参数 `$n` , n={0,1,2,3...}  
判断第n个参数存不存在  
```shell
if [ ! -n '$1' ];then
    echo "aaa"
    exit 1
fi
```
# 执行结果返回
想返回什么数值，就使用echo输出
```shell
# shell script tmp.sh
echo 'aaaa'

#获取返回值 并赋值给相关变量
shell_var=${sh tmp.sh}
```