---
layout: post
title:  "redis深入学习(二十)之Redis事务"
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
 

#### 事务队列

 每个Redis客户端都有自己的事务状态,这个事务状态保存在客户端状态的mstate属性里面:
 
 ```
 typedef struct redisClient{
     //...
     
     //事务状态
     multiState mstate;
     
     //...
 }redisClient;
 
 ```
 
 事务状态包含一个事务队列,以及一个已入队命令的计数器(也可以说是事务队列的长度)
 
 ```
 typedef struct multiState{
     //事务队列,FIFO顺序
     multiCmd * commands;
     //已入队命令计数
     int count;
 }multiState;
 
 ```
 
 事务队列是一个multiCmd类型的素数组,数组中的每个multiCmd结构都保存了一个已入队命令的相关信息,包括指向命令实现函数的指针,命令的参数,以及参数的数量:
 
 ```
 
 typedef struct multiCmd{
     //参数
     robj ** argv;
     
     //参数数量
     int argc;
     
     //命令指针
     struct redisCommand *cmd;
 }multiCmd;
 
 
 ```
 
事务队列以先进先出(FIFO)的方式保存入队的命令,较先入队的命令会被放到数组的前面,而较后入队的命令则会被放到数组的后面.

![image](http://7xpuj1.com1.z0.glb.clouddn.com/redis%E4%BA%8B%E5%8A%A1%E7%9A%84%E7%BB%93%E6%9E%84.png)


#### 执行事务

当一个事务状态的客户端向服务器发送EXEC命令时,这个EXEC命令将立即被服务器执行.服务器会遍历这个客户端的事务队列,执行队列中保存的所有命令,最后执行命令所得结果一次性全部返回给客户端.

EXEC命令的实现原理伪代码:

```
def EXEC();

//创建空白的回复队列
reply_queue = []

//遍历事务队列中的每个项
//读取命令的参数,参数的个数,以及要执行的命令
for argv,argc cmd in client.mstate.commands

  // 执行命令,并取得命令的返回值
  reply = execute_command(cmd,argc,argc)
  
  //将返回值追加到回复队列末尾
  reply_queue.append(reply)
  
 //移除redis_multi标识,让客户端回到非事务状态
 client.flags &=~REDIS_MULTI
 
 //清空客户端的事务状态,包括:
 1. 清零入队命令计数器
 2. 释放事务队列
 
 client.mstate.count = 0
 release_transaction_queue(client.mstate.commands)
 
 //将事务的执行结果返回给客户端
 
 send_reply_to_client(client,reply_queue )


```

#### WATCH 命令的实现

WATCH是一个乐观锁,它可以在EXEC命令执行之前,监视任意数量的数据库健,并在EXEC命令执行时,检查被监视的键是否至少有一个已经被修改过了,如果是的话,服务器拒绝执行事务,并向客户端返回代表事务执行失败的空回复.

以下是一个事务执行失败的例子:

```

redis > WATHC "name"
ok

redis > MULTI
OK

redis > set "name" "peter"
QUEUED

redis > EXEC
(NIL)

```

#### 使用WATCH命令监视数据库键

每个redis数据库都保存着一个watched_keys字典,这个字典的键是某个被WATCH命令监视的数据库键,而字典的值则是一个链表,链表中记录了所有监视相应数据库键的客户端:

```

typedef struct redisDb{
    //正在被WATCH 命令监视的键
    dict * watched_keys;
    
}

```

通过watched_keys 字典,服务器可以清楚低知道哪些数据库键正在被监视,以及哪些客户端正在监视这些数据库键.

通过执行WATCH命令,客户端可以在watched_keys字典中与被监视的键进行关联.


#### 监视机制的触发



[redis设计与实现](https://book.douban.com/subject/25900156/) 

