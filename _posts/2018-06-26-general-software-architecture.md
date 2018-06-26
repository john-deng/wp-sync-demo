---
ID: 288
post_title: 软件架构
post_name: general-software-architecture
author: 邓冰寒
post_date: 2018-06-26 10:00:00
layout: post
link: >
  https://devops.vpclub.cn/general-software-architecture/
published: true
tags:
  - Software Architecture
categories:
  - 通用
---

# 软件架构

## 技术架构

### 概述

谈到软件架构，实际上是我们在对整个软件系统做重要的规划。这意味着我们的决定会关系到所有的需求（包括性能、安全等），系统组织架构，系统各部分之间如何通讯，如果有外部依赖，如何指引工程师去实现，其中有什么风险需要考虑等等。

没有经过缜密的架构设计的软件，代码往往杂乱无章堆积，当代码越堆越多，维护成本会非常高，特别是传统的单体服务架构，往往为了业务能快速上线而堆积业务代码，团队成员的更迭及技术或业务水平参差不齐亦会带来维护的困难。

很显然，我们需要软件架构，而软件架构包含一系列的选型和决策。如果我们需要对软件架构做更改，需要考虑是否会带来很大的影响，是否难以更改？

### 软件架构的基本要求

* 必须安全可靠，容易维护；
* 要有领域概念，容易理解；
* 应该长期可用，容易扩展；
* 应对需求变更，容易实现；
* 能够水平扩展，要高可用；
* 没有重复代码，容易重构；
* 对于新增功能，性能不变；

除了以上基本要求，软件架构应有以下特点

* 用户体验要好；
* 可伸缩性好，用户容易适应；
* 能支撑业务增长（如海量用户）；
* 高性能，速度快；
* 容易更改和新增功能；
* 容易运行测试。

### 传统单体服务技术架构

* 成功的应用会随着时间变得巨大，最终代码变的很难修改，无法应对快速迭代
* 难以测试和持续集成部署，部署时间长
* 可靠性低，任何局部问题可以导致整个系统崩溃，任何改动都可能造成巨大风险
* 系统启动时间变长，难以使用最新的技术来提升性能

### 微服务介绍和发展历程

![svc-history](/images/general-software-architecture/svc-revolution.png)

* 单体服务架构：服务臃肿，业务错综复杂，相互依赖，紧耦合，高风险
* 面向服务架构：服务拆分，松耦合，服务治理，稳定性提高
* 微服务架构：粒度更细，去中心化，服务自治，灵活部署

### 微服务的优点与挑战

* 优点：
  * 根据不同业务领域拆分成细粒度的服务，解决了复杂性问题
  * 每个微服务有自己的数据库
  * 服务可以由不同功能团队完成，遵循服务自治原则
  * 独立部署，容易持续集成和持续交付
  * 根据业务量来水平伸缩特定服务
* 挑战及应对措施：
  * 服务间通讯变得复杂怎么办？
    * 使用服务发现机制及RPC通讯
  * 客户端要向N个服务发送请求？  
    * 使用API网关，客户端只需要和API网关通讯

### 微服务开发遵循的十二个基本要素

1. 基准代码: 一份基准代码只允许一份部署
2. 依赖: 显式声明依赖关系
3. 配置: 在环境中存储配置，不提交到基准代码库
4. 后端服务: 把后端服务当作附加资源，保持松耦合
5. 构建，发布，运行: 严格分离构建和运行，不要修改运行中的代码
6. 进程: 以一个或多个无状态进程运行应用，数据持久化
7. 基准代码: 一份基准代码只允许一份部署
8. 依赖: 显式声明依赖关系
9. 配置: 在环境中存储配置，不提交到基准代码库
10. 后端服务: 把后端服务当作附加资源，保持松耦合
11. 构建，发布，运行: 严格分离构建和运行，不要修改运行中的代码
12. 进程: 以一个或多个无状态进程运行应用，数据持久化

### 服务发现

* 为什么要服务发现？
  * 微服务部署在容器上，IP是动态分配的
  * 扩展、失败、升级导致ip变化
  * 为了实现自动化部署
* 常用的服务发现机制
  * Netflix Eureka
  * Zookeeper
  * Kubernetes DNS Service Discovery

### 服务间通讯

* 由于系统被拆分成许多的微服务，服务间通信协议一般有下面几种：
  * REST
  * Thrift
  * GRPC

### 传统服务事务管理

* 单体应用通常使用ACID事务
  * 原子化
  * 一致性
  * 隔离
  * 持久
* 事务在微服务架构下的挑战
  * 保持服务一致性
  * 跨服务数据查询

### 分布式微服务事务管理

* 各个服务自动更新数据库和发布事件
* 消息代理确保事件传递成功
* 提供弱一致性，最终一致性
* 案例：
  * 订单服务创建订单，并发布“订单创建”事件
  * 库存服务获取“订单创建”事件，预扣相应库存，并发布“库存预扣”事件
  * 订单服务获取“库存预扣”事件并处理订单支付，如订单处理成功，发布“订单支付”事件
  * 库存服务获取“订单支付”事件，扣除库存，并发布“库存扣除”事件。

## 业务架构

![biz-arch](/images/general-software-architecture/biz-arch.png)

整体系统架构由应用层、服务层、支撑层三部分组成，前后端分离，其中：

应用层：面向最终客户的交互界面。展现形式包含：Android客户端、IOS客户端,微信公众号（业务辅助平台）, PC客户端和其它互联网APP 。

服务层：包括B2C, B2C2C 电商业务功能、营销能力、API能力功能模块。

支撑层：通过对接外部服务以及基础服务，提供对服务层的核心功能和系统的支撑.

## 部署架构

传统数据中心历经了传统的小机时代、X86化到IaaS资源池化的发展进程，但是整体IT架构依然处于“烟囱式”的架构模式。整个IT运营管理面临着应用部署手动化、应用扩容缩容非实时性、前端负载均衡技术无法实现应用动态引流、无法持续集成与部署，无法快速迭代应用程序、监控方法分散不统一、容灾环境各异的难题，同时传统的数据中心高可用能力比较差，这些都增添了IT运营的成本及工作量。在这种环境下，新型PaaS平台对改造滞后的IT系统架构提供了针对性很强的解决方案！

微品致远采用开源的Openshift社区版本Origin，Openshift Origin是一个基于Kubernetes和Docker的PaaS平台。

Kubernetes是Google开源的容器集群管理系统。它构建在Docker技术之上，为容器化的应用提供资源调度、部署运行、服务发现、扩容缩容等一整套功能.

微品从2015年就投入容器技术的研发，并且采用了DevOps开发模式，通过持续集成／持续交付，在容器部署方面拥有非常多的经验。由于Openshift是基于Kubernetes的PaaS平台，故部署Openshift的服务可以无缝对接部署到其它的Kubernetes平台上。

### Openshift Origin的整体架构

![msvc-arch](/images/general-software-architecture/msvc-arch.png)

Docker服务提供了打包和创建基于Linux的轻量级容器镜像的抽象。 Kubernetes在多个主机上提供集群管理和编排容器，OpenShift Origin有以下主要特性:

* 提供资源、权限和角色管理；
* 为开发者管理代码，构建和部署服务，持续集成和持续交付；
* 在整个系统中管理和伸缩镜像实例；
* 管理应用服务，提供弹性扩缩容调度；
* 适用各种规模的开发团队管理和跟踪应用服务；
* 支持群集的网络基础架构；
* 提供容器的监控、日志收集和查询；
* 支持动态按需申请挂载磁盘。

### 服务发现／治理

Openshift提供了DNS服务，每个应用服务部署完毕会被分配到一个以服务名称来命名的域名，指向Service的虚拟IP（VIP），通过Service的VIP就能找到Pod的VIP，最终调用到我们想要的服务接口。

#### 热点数据缓存

传统方式是应用系统在启动时通过数据库加载到应用实例缓存中，提高数据的访问速度，但往往因为加载的数据过多而导致系统启动过慢。

在设计时将热点数据存放到缓存服务器（如Redis、Hazelcast）中, 在启动时避免执行加载缓存动作，提高应用系统启动速度，相关热点数据可通过手动或自动方式更新到缓存服务器中，应用通过访问缓存服务器获取相关热点数据。

### 高并发请求入消息队列

随着互联网技术的快速发展，大量用户同时请求资源会影响服务的响应速度，为了解决这个问题，在设计时将高并发的数据先入队列，在资源有限的情况下，缓解了服务器的压力，并给用户提供良好的用户体验。

### 应用无状态化

应用开发设计时将用户的会话信息保存到缓存服务器（Hazelcast）中，根据用户的唯一标识session_id来对缓存服务器进行存取用户的会话信息，这样会话信息与应用实例分离，实现应用无状态话。

### 和网络有关的有两块：路由层和服务层。

路由层是一个容器化的HAproxy。每个计算节点最多有一个，一个Openshift集群中可以有多个路由实例。需要注意的是，这个容器化的HAproxy，目前只能处理7层的http/https/websockets/TLS（with SNI）请求。路由层负责将应用的FQDN最终解析成容器的Pod IP。