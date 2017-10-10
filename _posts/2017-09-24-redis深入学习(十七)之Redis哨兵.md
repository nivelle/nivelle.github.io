---
layout: post
title:  "redis深入学习(十七)之Redis哨兵"
date:   2017-09-22 01:06:05
categories: noSQL
tags: redis
excerpt: redis
---

### Sentinel

Sentinel是redis的高可用性解决方案:由一个或多个Sentinel实例组成的Sentinel系统可以监视任意多个主服务器,以及这些主服务器属下的所有从服务器,并在被监视的主服务器进入下线状态时,自动将下线主服务器属下的某个从服务器升级为新的主服务器,然后由新的主服务器代替已经下线的主服务器继续处理命令请求.

![image](http://7xpuj1.com1.z0.glb.clouddn.com/%E6%9C%8D%E5%8A%A1%E5%99%A8%E4%B8%8Esentinel%E7%B3%BB%E7%BB%9F.png  )


### 启动并初始化Sentinel

启动一个sentinel可以使用命令;

```
$ redis-sentinel /path/to/your/sentinel.conf

```

或者

```
$ redis-sentinel /path/to/your/sentinel.conf --sentinel

```

Sentinel本质上是一个特殊模式下的Redis服务器.


![image](http://7xpuj1.com1.z0.glb.clouddn.com/sentinel%E6%A8%A1%E5%BC%8F%E4%B8%8B%E6%9C%8D%E5%8A%A1%E5%99%A8%E4%BD%BF%E7%94%A8%E6%83%85%E5%86%B5.png)


### Sentinel结构

```
struct sentinelState{
    
    //当前纪元,用于实现故障转移
    uint64_t current_epoch;
    
    //保存了所有被这个sentinel监视的主服务器
    // 字典的键是主服务器的名字
    //字段的值是一个指向sentinelRedisInstance结构的指针
    
    dict *masters;
    
    //是否进入了TILT模式
    int tilt;
    
    //目前正在执行的脚本的数量
    int running_scripts;
    
    //进入TILT模式的事件
    mstime_t tilt_start_time;
    
    //最后一次执行时间处理器的时间
    mstime_t previous_time;
    
    //一个FIFO队列,包含了所有需要执行的用户脚本
    list *scripts_queue;
}sentinel


```

### 初始化Sentinel状态的masters属性

Sentinel状态中的masters字典记录了所有被Sentinel监视的主服务器的相关信息,其中:

- 字典的键是被监视主服务器的名字
- 字典的值是被监视主服务器对应的sentinel.c/sentinelRedisInstance结构


sentinelRedisInstance结构代表一个被Sentinel监视的Redis服务器实例,这个实例可以是服务器,从服务器,或者另外一个Sentinel监视的Redis服务器实例,这个实例可以是主服务器,从服务器,挥着另外一个Sentinel.


```
typedef struct sentinelRedisInstance{
    //标识值,记录了实例的类型,以及该实例的当前状态
    int flags;
    //实例的名字
    //主服务器的名字由用户在配置文件中设置
    //从服务器以及Sentinel的名字由Sentinel自动设置
    //格式为ip:port,例如"127.0.0.1:26379""
    char *name;
    //实例的运行ID
    char *runid;
    
    //配置纪元,用于实现故障转移
    uint64_t config_epoch;
    
    //实例的地址
    sentinelAddr *addr;
    
    //SENTINEL down-after-milliseconds 选项设定的值
    // 实例无响应多少毫秒之后才会被判断为主观下线(subjectively down)
    mstime_t down_after_period;
    //SENTINEL monitor<master-name><IP><port><quorum>选项中的quorum参数
    //判断这个实例为客观下线(objectively down)所需的支持投票数量
    int quorum;
    //SENTINEL parallel-syncs<master-name><number>选项的值
    //在执行故障转移操作时,可以同时对新的主服务器进行同步的从服务器数量
    int parallel_syncs;
    //SENTINEL failover-timeout <master-name><ms>选项的值
    //刷新故障迁移状态的最大时限
    mstime_t failover_timeout;
}sentinelRedisInstance;




```
