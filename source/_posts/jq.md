---
title: jq
categories: 
- 工具
tags:     
- json文本处理 
comments: true
---
### jq
    jq可以对json数据进行分片、过滤、映射和转换，和sed、awk、grep等命令一样，都可以让你轻松地把玩文本

<!-- more -->  

### "."
    最简单的jq程序是表达式"."
    ```
    cat json.text|jq .
    ```
    example
    ```
    {
    "name": "tool",
    "url": "http://tool.china.com",
    "addr": {
        "city": "beijing",
        "country": "china"
    },
    "array": [
        {
        "comp": "google",
        "url": "http://www.google.com"
        },
        {
        "comp": "baidu",
        "url": "http://www.baidu.com"
        }
    ]
    }
    ```
### index
    ```
    cat json.text|jq '.[0]'
    ```
### 管道 |
    ```
    cat json.txt | jq '.[0] | {name:.name,city:.addr.city}'
    ```
    echo:

    {
        "name":"jjj",
        "city":"bj"
    }


    ```
    cat json.txt | jq '.[0] | {name:.array[0].comp,city:.addr.city}'
    ```
    echo:

    {
        "name":"google",
        "city":"bj"
    }
### [] 把jq的输出当作一个数组

### {} 自定义key
    ```
    cat json.txt | jq '.[0] | {username:.array[0].comp,livecity:.addr.city}'
    ```