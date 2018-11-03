---
layout: post
title:  "springcloud和dubbo"
date:   2018-08-24 01:06:05
categories: 知识点
tags: 分布式
excerpt: 分布式
---


* content
{:toc}

### 概念理解

- SOA
 
SOA全英文是Service-Oriented Architecture，中文意思是中文面向服务编程，是一种方法论，一种分布式的服务架构SOA解决多服务凌乱问题，SOA架构解决数据服务的复杂程度，同时SOA又有一个名字，叫做服务治理。

- RPC 

远程服务调用，其调用协议通常包含传输协议和编码协议。HTTP 严格来说跟 RPC不是一个层级的概念，HTTP本身也可以作为 RPC 的传输层协议

### 比较

- 通信方式的不同

Dubbo使用的是RPC通信(自定义的TCP协议作为通行协议)，而SpringCloud 使用的是HTTP，RESTFul（Http作为协议和服务打交道）

- 各个组成部分不同


组件  |  dubbo | springCloud
---|---| ---
服务注册中心| zk，redis,simple,multicast | Spring Cloud Netflix Eureka
服务监控| monitor | Spring boot Admin
断路器| 不完善|Netflix Hystrix
服务网关| 无|SpringCloud Nerflix GateWay
分布式配置|无|SpringCloud Config
服务跟踪|无|SpringCloud sleuth
消息总线|无|SpringCloud Bus
数据流|无|SptringStream
批量任务|无|SpringCloud Task

- 性能

因为 Dubbo 采用单一长连接和NIO异步通讯（保持连接/轮询处理），使用自定义报文的 TCP 协议，并且序列化使用定制 Hessian2 框架，适合于小数据量大并发的服务调用，以及服务消费者机器数远大于服务提供者机器数的情况，但不适用于传输大数据的服务调用。而 Spring Cloud 直接使用 HTTP 协议（但也不是强绑定，也可以使用 RPC 库，或者采用 HTTP 2.0 + 长链接方式（Fegin 可以灵活设置））。

### dubbo

- 优点

1. Dubbo 支持 RPC 调用，服务之间的调用性能会很好。
2. 支持多种序列化协议，如 Hessian、HTTP、WebService。

- 缺点

1. Registry 严重依赖第三方组件（zookeeper 或者 redis），当这些组件出现问题时，服务调用很快就会中断。
2. Dubbo 只支持 RPC 调用。使得服务提供方（抽象接口）与调用方在代码上产生了强依赖，服务提供者需要不断将包含抽象接口的 jar 包打包出来供消费者使用。
3. 不支持跨语言
4. Dubbo 只是实现了服务治理

### springCloud

- 优点

1. 标准化的将微服务的成熟产品和框架结合一起，Spring Cloud 提供整套的微服务解决方案，开发成本较低，且风险较小。
2. 基于 Spring Boot，具有简单配置、快速开发、轻松部署、方便测试的特点。
3. 支持 REST 服务调用，相比于RPC，更加轻量化和灵活（服务之间只依赖一纸契约，不存在代码级别的强依赖），有利于跨语言服务的实现，以及服务的发布部署。另外，结合 Swagger，也使得服务的文档一体化。

- 缺点

1. 支持 REST 服务调用，可能因为接口定义过轻，导致定义文档与实际实现不一致导致服务集成时的问题
2. 支持 REST 服务调用，可能因为接口定义过轻，导致定义文档与实际实现不一致导致服务集成时的问题

### ZooKeeper 和 Eureka 的区别

鉴于服务发现对服务化架构的重要性，Dubbo 实践通常以 ZooKeeper 为注册中心（Dubbo 原生支持的 Redis 方案需要服务器时间同步，且性能消耗过大）。针对分布式领域著名的 CAP 理论（C——数据一致性，A——服务可用性，P——服务对网络分区故障的容错性），Zookeeper 保证的是 CP ，但对于服务发现而言，可用性比数据一致性更加重要，AP 胜过 CP，而 Eureka 设计则遵循 AP 原则。
