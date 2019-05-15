---
title: mysql数据库使用规范总结
categories: 
- 数据库
tags:
- mysql
- 数据库规范 
comments: true
---

### 基本规范  

<!-- more -->

- 表存储引擎设置为innodb
- 表字符集默认使用utf8,必要时使用utf8mb4。utf8mb4是utf8的超集,有存储4字节例如表情符号时使用他。
- 禁止数据库中存储大文件,例如照片.可以将大文件存储在对象存储系统中。数据库存储路径

- 一般不使用存储过程、视图、触发器,event.对数据库性能影响大，调试，排错，迁移都比较困难.拓展性差
- 对于互联网业务，能让站点层和服务层干的事,不要交到数据库

### 命名规范

- 库名，表名,列名都必须小写,采用下划线分割
- 库名，表名,列名最好见名知意。长度不超过32个字符
- 库备份以bak为前缀，日期为后缀
- 从库以-s为后缀
- 备库以-ss为后缀

### 建表规范
- 单实例表个数控制在2000个以内
- 单表分表个数控制在1024个以内
- 表必须有主键，一般使用unsigned整数类型为主键  

- 删除无主键的表,如果是row模式的主从架构。从库会挂住
- 一般不使用外键,如果要保证完整性。应该由应用程式实现  

- 外键使得表之间相互耦合，影响update/delete性能.有可能造成死锁
- 建议将大字段,访问频率少的字段拆分到单独表中存储，分离冷热数据  

### 列设计规范  

- 根据业务区分使用tinyint/int/bigint,分别占用1/4/8字节
- 根据业务区分使用char/varchar

- 字段长度固定,或者长度近似的业务场景,适用char,能减少碎片,查询性能高
- 字段长度相差大，或者更新少的业务场景，适合使用varchar,可以减少空间
- 根据业务区分使用datetime/timestamp(分别占5/4字节)

- 把not null字段设置为默认值.因为null的列使用索引统计,值都更加复杂。更难优化。只能使用is null 或者is not null 而在=/!=/in/not in有大坑

- 使用int unsigned 存储ipv4 ,不要用char(15).
- 使用varchar(20)存储手机号,不要用整数
- varchar可以模糊查询

### 索引规范  

- 唯一索引使用uniq_[字段名]来命名
- 非唯一索引使用idx_[字段名]来命名
- 单张表索引数量控制在5个以内

- 太多索引影响写性能;生成执行计划时,如果索引太多,会降低性能.并可能导致mysql选择不到最优索引
- 组合索引字段数建议不超过5个
- 不建议在频繁更新的字段上建立索引
- 非必要不用join查询，如果要join查询,被join的字段必须类型相同,并建立索引
- 组合索引最前缀原则,避免重复建索引,如果建立了(a,b,c)相当于建立了(a),(a,b),(a,b,c)

### sql书写规范  

- 尽量不写select *，只获取必要字段.否则会增加cpu/io/内存/带宽的消耗
- 同一个字段上的or改写成in,in的值要少于50个
- 应用程序捕获sql异常，方便定位线上问题         
- insert必须指定字段,禁止使用insert into table values()
- 尽量不对大表join和子查询


### 建表实例  

```
    CREATE TABLE `user` (
    `user_id` bigint(20) NOT NULL AUTO_INCREMENT COMMENT '会员信息表主键，自增长',
    `uuid` bigint(20) DEFAULT '0' COMMENT '会员唯一标识表主键，自增长',
    `guid` bigint(20) DEFAULT '0' COMMENT '用户中心全局ID',
    `is_certed` tinyint(4) DEFAULT '2' COMMENT '是否认证，1 认证 2  未认证',
    `mobile` varchar(40) COLLATE utf8mb4_bin NOT NULL COMMENT '手机号码',
    `last_login_tm` datetime(6) DEFAULT '0000-00-00 00:00:00.000000' COMMENT '最后一次登录时间',
    `last_login_device` varchar(20) COLLATE utf8mb4_bin DEFAULT NULL COMMENT '最后一次登录设备号',
    `remark` varchar(250) COLLATE utf8mb4_bin DEFAULT '' COMMENT '备注',
    `is_deleted` tinyint(4) DEFAULT '1' COMMENT '是否删除，1 未删除，2 已删除',
    `created_tm` datetime(6) DEFAULT CURRENT_TIMESTAMP(6) COMMENT '创建时间，默认是 CURRENT_TIMESTAMP(6)',
    `updated_tm` datetime(6) DEFAULT '0000-00-00 00:00:00.000000' ON UPDATE CURRENT_TIMESTAMP(6) COMMENT '修改时间，修改时 CURRENT_TIMESTAMP(6)',
    PRIMARY KEY (`user_id`)
    ) ENGINE=InnoDB AUTO_INCREMENT=0 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_bin COMMENT='会员信息表';  
```