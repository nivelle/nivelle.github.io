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
