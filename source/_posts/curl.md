---
title: curl参数
categories: 
- 协议
tags: 
comments: true
---

## curl常用参数  

<!-- more --> 

## -o 输出到文件
```
curl -o home.html  http://www.baidu.com
```
## -s 不输出任何东西
```
curl -s 
```
## -# 进度条显示当前传送状态

## -O 后面的url要具体到某个文件，不然抓不下来

```
 curl -# -O  http://www.baidu.com/1.jpg
```
## -x 使用http代理
```
 curl -x 24.10.28.84:32779 -o home.html http://www.baidu.com
```
## 断点续传，-C(大写的)
```
curl -C -O  http://www.baidu.com/1.jpg
```
## 使用cookie
```
curl -b ./cookie.txt  http://www.baidu.com/admin 
```
## 模拟表单信息，模拟登录，保存cookie信息
```
curl -c ./cookie_c.txt -F log=aaaa -F pwd=****** http://blog.51yip.com/wp-login.php  
```
## 模拟表单信息，模拟登录，保存头信息
```
curl -D ./cookie_c.txt -F log=aaaa -F pwd=****** http://blog.51yip.com/wp-login.php  
```