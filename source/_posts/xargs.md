---
title: xargs
categories: 
- 工具
tags:
- xargs 
comments: true
---
### xargs

<!-- more -->

    命令可以通过管道接受字符串，并将接收到的字符串通过空格分割成许多参数(默认情况下是通过空格分割) 然后将参数传递给其后面的命令，作为后面命令的命令行参数


### -d  
    xargs将其标准输入中的内容以空白(包括空格、Tab、回车换行等)分割成多个后当作命令行参数传给后面的命令
```
    echo "11@22@33" |xargs -d '@' echo 
```