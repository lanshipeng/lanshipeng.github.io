---
title: memcached's note
categories: 
- 缓存
tags: memcached
comments: true
---

#### save cmd  

<!-- more -->   

1. set key flags exptime bytes [noreply]  
value  

    说明：
    （如果set的key已经存在，该命令可以更新该key所对应的原来的数据，也就是实现更新的作用。

    [key]: key-value 中的key 用于查找缓存值
    [flags]: 包括简直对的整型参数，客户机使用它存储关于简直对的额外信息  
    [exptime]: 在缓存中保存键值对的时间长度 （以s为单位）    
    [bytes]: 缓存中字节数  
    [noreply]: 告知服务器不需要返回数据  
    [value]: 存储的值(始终位于第二行)  

    ```  
        set runoob 0 900 9
        memcached
        STORED
        get runoob
        VALUE runoob 0 9
        memcached
        END  
    ```
    out info:

    -- STORED：保存成功后输出。
    -- ERROR：在保存失败后输出。

2. add key flags exptime bytes [noreply]  
value
 
    说明：
    （如果 add 的 key 已经存在，则不会更新数据(过期的 key 会更新)，之前的值将仍然保持相同，并且您将获得响应 NOT_STORED）

    ```
    add new_key 0 900 10
    data_value
    STORED
    get new_key
    VALUE new_key 0 10
    data_value
    END
    ```
    out info;

    -- STORED：保存成功后输出。
    -- NOT_STORED ：在保存失败后输出。

3. replace key flags exptime bytes [noreply]  
value

    说明：
    （如果 key 不存在，则替换失败，并且您将获得响应 NOT_STORED。）

    ```
    add mykey 0 900 10
    data_value
    STORED
    get mykey
    VALUE mykey 0 10
    data_value
    END
    replace mykey 0 900 16
    some_other_value
    get mykey
    VALUE mykey 0 16
    some_other_value
    END
    ```
    out info:

    -- STORED：保存成功后输出。
    -- NOT_STORED：执行替换失败后输出。

4. append key flags exptime bytes [noreply]  
value

    说明：
    （Memcached append 命令用于向已存在 key(键) 的 value(数据值) 后面追加数据 。）

    ```
    set runoob 0 900 9
    memcached
    STORED
    get runoob
    VALUE runoob 0 9
    memcached
    END
    append runoob 0 900 5
    redis
    STORED
    get runoob
    VALUE runoob 0 14
    memcachedredis
    END
    ```
    out info:
    -- STORED：保存成功后输出。
    -- NOT_STORED：该键在 Memcached 上不存在。
    -- CLIENT_ERROR：执行错误。

5. prepend key flags exptime bytes [noreply]  
value

    说明：
    （
    Memcached prepend 命令用于向已存在 key(键) 的 value(数据值) 前面追加数据 。）

    ```
    set runoob 0 900 9
    memcached
    STORED
    get runoob
    VALUE runoob 0 9
    memcached
    END
    prepend runoob 0 900 5
    redis
    STORED
    get runoob
    VALUE runoob 0 14
    redismemcached
    END

    ```
    out info:
    -- STORED：保存成功后输出。
    -- NOT_STORED：该键在 Memcached 上不存在。
    -- CLIENT_ERROR：执行错误。

6. cas key flags exptime bytes unique_cas_token [noreply] value 

    
    params:
        unique_cas_token通过 gets 命令获取的一个唯一的64位值。
    out info:
    -- STORED：保存成功后输出。
    -- ERROR：保存出错或语法错误。
    -- EXISTS：在最后一次取值后另外一个用户也在更新该数据。
    -- NOT_FOUND：Memcached 服务上不存在该键值。


7. get key

    说明： 
    （
    Memcached get 命令获取存储在 key(键) 中的 value(数据值) ，如果 key 不存在，则返回空。）

    多个key使用空格隔开：
        get key1 key2 key3    

8. gets key

    说明：
    (Memcached gets 命令获取带有 CAS 令牌存 的 value(数据值) ，如果 key 不存在，则返回空。)

    ```
    set runoob 0 900 9
    memcached
    STORED
    gets runoob
    VALUE runoob 0 9 1
    memcached
    END
    ```
    使用 gets 命令的输出结果中，在最后一列的数字 1 代表了 key 为 runoob 的 CAS 令牌    

9. delete key [noreply]  

    说明：
    (删除已存在的key)
    ```
    set runoob 0 900 9
    memcached
    STORED
    get runoob
    VALUE runoob 0 9
    memcached
    END
    delete runoob
    DELETED
    get runoob
    END
    delete runoob
    NOT_FOUND

    ```
    out info:
    -- DELETED：删除成功。
    -- ERROR：语法错误或删除失败。
    -- NOT_FOUND：key 不存在。      

10. incr key increment_value  

    说明：
    key：键值 key-value 结构中的 key，用于查找缓存值。
    increment_value： 增加的数值。
    ```
    set visitors 0 900 2
    10
    STORED
    get visitors
    VALUE visitors 0 2
    10
    END
    incr visitors 5
    15
    get visitors
    VALUE visitors 0 2
    15
    END
    ```
    ```
    set visitors 0 900 2
    10
    STORED
    get visitors
    VALUE visitors 0 2
    10
    END
    decr visitors 5
    5
    get visitors
    VALUE visitors 0 1
    5
    END
    ```

    out info:
    -- NOT_FOUND：key 不存在。
    -- CLIENT_ERROR：自增值不是对象。
    -- ERROR其他错误，如语法错误等。  

#### count cmd  

11. stats  

    Memcached stats 命令用于返回统计信息例如 PID(进程号)、版本号、连接数等。


12. stats items  

    Memcached stats items 命令用于显示各个 slab 中 item 的数目和存储时长(最后一次访问距离现在的秒数)。

13. stats sizes 

    Memcached stats sizes 命令用于显示所有item的大小和个数。

    该信息返回两列，第一列是 item 的大小，第二列是 item 的个数。

14. stats slabs

    Memcached stats slabs 命令用于显示各个slab的信息，包括chunk的大小、数目、使用情况等。

15. flush_all  

    Memcached flush_all 命令用于清理缓存中的所有 key=>value(键=>值) 对。
该命令提供了一个可选参数 time，用于在制定的时间后执行清理缓存操作。