---
layout: post
title:  "redis深入学习(二十)之Redis事物"
date:   2017-11-26 01:06:05
categories: noSQL
tags: redis
excerpt: redis
---


### 事务的定义

Redis通过MULTI,EXEC,WATCH等命令来实现事务.事务提供了一种将多个命令请求打包,然后一次性,按顺序低执行多个命令的机制,并且在事务执行期间,服务器不会中断事务而改去执行其他客户端的命令请求,它会将事务中的所有命令都执行完毕,然后才去处理其他客户端命令请求.

```
redis > MULTI
ok

redis > set "name" "Pratical common lisp"
QUEUED

redis > get "name"
QUEUED

redis > set "author" "peter seibel"
QUEUED

redis > get "author"
QUEUED

redis > EXEC

```

#### 事务的实现

一个事务从开始到结束,会经历以下三个阶段:

- 事务开始

MULTI 命令的执行标志着事务的开始:

```
redis > MULTI
ok

```
MULTI命令可以将执行该命令的客户端从非事务状态切换至事务状态,这一切换是通过在客户端状态的flags属性中打开redis_multi标识来完成的,MULTI命令的实现可以用如下伪代码来表示:

```
def MULTI():

//打开事务标识
client.flags != REDIS_MULTI

//返回ok回复
replyOK()


```

#### 命令入队

当一个客户端处于非事务状态时,这个客户端发送的命令会立即被服务器执行:

```
redis > SET "name" "Practical Common Lisp"
ok

redis > GET "name" 
"Practical Common Lisp"


```

当一个客户端切换到事务状态时,服务器会根据这个客户端发来的不同命令执行不同操作:

  - 如果客户端发送的命令为EXEC,DISCARD,WATCH,MULTI四个命令的其中一个,那么服务器立即执行这个命令
  - 与此相反,如果客户端发送的命令是EXEC,DISCARD,WATCH,MULTI四个命令意外的其他命令,那么服务器并不立即执行这个命令,而是将这个命令放入一个事务队列里面,然后想客户端返回queued回复.

![image](http://7xpuj1.com1.z0.glb.clouddn.com/redis%E4%BA%8B%E5%8A%A1%E9%98%9F%E5%88%97.png)
 

- 命令入队
- 事务执行

