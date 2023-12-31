---
title: '云原生15要素'
date: 2023-11-25T22:40:43+08:00
lastmod: 2023-11-25T22:40:43+08:00
draft: false
---
# 云原生15要素
12要素应用（Twelve-Factor App），出自Heroku创始人 Adam Wiggins 在2012年编写的《The Twelve-Factor App》，最开始是用来衡量一个后端服务是否适合搬到云上，描述了云端应用服务应当遵循的一些最佳实践，被称为“云12要素”。

在云原生的理念推广后，Pivotal的 Kevin Hoffman 在《Beyond the Twelve-Factor App》书中修订了最初的12个因素，并增加了3个额外的因素，被称为“云原生15要素”。

本文按后者的“云原生15要素”讲述

## 1.基准代码 Codebase
**一份基准代码，多份部署 (One codebase tracked in revision control, many deploys)。**

部署到不同环境（如开发、测试、预发、生产等环境）的同一个应用，基准代码库应该相同，每份部署包含各自环境中的不同配置。

## 2.依赖（Dependencies）
**显式地声明依赖关系 (Explicitly declare and isolate dependencies)。**

应用程序不会隐式依赖系统级的类库，它通过“依赖清单”确切地声明所有依赖项。此外，在运行过程中通过“依赖隔离”工具来确保程序不会调用系统中存在但清单中未声明的依赖项。这一做法会统一应用到生产和开发环境。

完善的依赖管理机制、显式的依赖声明文件和版本锁机制，能够减少因为错误的依赖版本导致的Bug。

## 3.配置（Config）
**在环境中存储配置(Store config in the environment)。**

配置数据和构建产物完全分离，配置数据单独管理，只在运行环境中出现。环境变量可以非常方便地在不同的部署间做修改，却不动一行代码；与一些传统的解决配置问题的机制（如Java的属性配置文件）相比，环境变量与语言和系统无关。

若是配置不完全和代码分离，相当于两个危险的化合物一开始就被混合在一起，部署的时候原地爆炸也不足为奇。

实践心得：有部分开发团队把环境相关的配置，混在容器镜像、甚至代码包中，每个环境需要单独构建打包一个版本，每次修改配置都需要重新出容器镜像，导致效率极其低下。

## 4.后端服务（Backing Services）
**分离基础的后端组件（Treat backing services as attached resources）。**

所有依赖的基础组件或者其他应用服务，比如数据库、缓存服务、消息队列、二方/三方服务，都视为外部资源，独立部署，通过网络访问

从面向对象的角度看，依赖取代组合，耦合性更弱，可以增加大量的容错处理逻辑。

应用不会区别对待本地服务或第三方服务。对应用而言，两种都是附加资源，通过一个URL或其他存储在配置中的服务定位、服务证书来获取数据。“12要素”应用的任意部署，都应该可以在不进行任何代码改动的情况下执行，比如数据库地址和SMTP服务器地址，仅需修改配置中的资源地址。

## 5.构建、发布、运行（Build、Release、Run）
**严格分离构建、发布、运行（Strictly separate build and run stages）。**

构建是开发测试人员更关注的、发布是产品经理更关注的、运行是运维更关注的。每个阶段有专门的工具和方法论。

流水线模式可以带来的效率提升，以及各阶段之间的缓冲空间。自动化系统很重要。用好CI/CD系统、项目管理系统，制定好规则和流程并自动运转起来。

## 6.无状态的服务进程（Processes）
**以一个或多个无状态进程运行应用（Execute the app as one or more stateless processes）。**

无状态是水平扩展的前提，对于Serverless应用更是必要条件。

## 7.端口绑定（Port Binding）
**通过端口绑定提供服务（Export services via port binding）。—**

应用完全自我加载，不依赖任何网络服务器就可以创建一个面向网络的服务。互联网应用通过端口绑定来提供服务，并监听发送至该端口的请求。

## 8.并发（Concurrency）
**通过进程模型进行扩展（Scale out via the process model）。**

云原生应用，注重的是伸缩能力，无状态应用很容易实现水平扩展。

应用的进程主要借鉴于UNIX守护进程模型。开发人员可以运用这个模型设计应用架构，将不同的工作分配给不同的进程类型。

## 9.易处理（Disposability）
**快速启动和优雅终止可最大化健壮性（Maximize robustness with fast startup and graceful shutdown）。**

可以瞬间开启或停止，有利于快速、弹性地伸缩应用，以及管理带来部署变化的代码或配置，保证稳健地部署应用。

云原生应用需要保持更优秀的可伸缩性，服务的部署实例随时可能被创建出来、或者被销毁掉，这就要求服务自身提供快速启动和优雅退出能力。

不具有快速启动能力，水平扩容的速度受限；不具备优雅退出能力，缩容时未处理完的业务中断，会导致用户请求错误、数据不一致性等问题。

## 10.开发环境与线上环境等价（Dev and Prod Parity）
**尽可能保持开发、预发布、线上环境相同（Keep development, staging, and production as similar as possible）。**

应用要想做到持续部署，就必须缩小开发环境与线上环境的差异。“12要素”应用的开发人员应该反对在不同环境间使用不同的后端服务，因为不同的后端服务意味着会突然出现不兼容，从而导致测试、预发布都正常的代码在线上出现问题。

实践心得：大量的开发人员向我甩锅式的抱怨 “我本地是正常的啊”、“开发环境是正常的啊”、“是不是环境/机器问题” ，就是因为开发环境和测试环境不一致。

## 11.日志（Logs）
**将日志当作事件流（Treat logs as event streams）。**

“单一职责原则”。应用本身从不考虑存储自己的输出流。不应该试图去写或者管理日志文件。相反，每一个运行的进程都要求直接标准输出（Stdout）事件流。在开发环境中，开发人员可以通过这些数据流，在终端实时地看到应用的活动。在预发布或线上部署中，每个进程的输出流由运行环境截获，并将其他输出流整理在一起，然后一并发送给一个或多个最终处理程序，用于查看或长期存档。这些存档路径对应用来说不可见也不可配置，而是完全交给程序的运行环境管理。

日志如何处理是平台的职责，而非应用自身的业务。因此，应用服务只要把日志作为事件流抛出去就好了，容器环境中，最好的办法就是直接打印到标准输出和标准错误（stdout, stderr）。

实践心得：一些项目中写了一堆log4xx的配置，可能这是传统的软件这是必备的，但是云原生应用，只需要打印到标准输出/标准错误，额外添加log4xxx之类SDK类型的日志组件，会导致看日志时有时候需要看容器日志，有时候又需要看服务自己的日志，产生混乱。

## 12.管理类任务（Admin Processes）
**将后台管理任务作为一次性进程运行（Run admin/management tasks as one-off processes）。**

这里是Admin Processes指的是执行数据库DDL、周期执行的运维任务、一次性的数据迁移和修复等等这类事情，更贴切的说法是“后台管理类任务”

善于利用Kuernetes提供的CronJob机制。

《Beyond the Twelve-Factor App》书中甚至建议彻底去除Admin Processes，所有的东西都是可伸缩的Backing Service

## 13.优先考虑API设计（API First）

API如同契约，对云原生应用的开发者来说，设计出合理的、具有高兼容性的交互API非常重要。API可以有多种实现方式，比如RESTful API、WebService API等，需要优先对系统的API进行设计。不过，设计一个具有前瞻性的API并不容易，需要尽可能考虑API的向下兼容，提供每次改动唯一的版本号。

## 14.通过遥测感知系统状态（Telemetry）

云原生应用实例很多时候是无状态的、无差别地混合部署的。当出现问题时，应用本身可以直接关闭并从服务器集群中移除，通过调度编排系统在其他服务器中自动进行补充。在这个过程中，需要遥测感知来监控服务器的状况，应用可以通过APM、健康检查、日志、链路追踪等方式进行监控。

对于云原生系统，要杜绝传统的 “SSH进去运行Debug工具” 的事情发生，“遥测”是实现这一点的唯一手段。监控、告警、链路追踪，在微服务系统中缺一不可，这是可观测性的一部分。

## 15.认证和授权（Authentication and Authorization）

安全在云原生中至关重要，企业不能简单地将安全问题抛给云平台，需要诸如OAuth2认证和RBAC授权等基于应用级别的安全机制。

云原生服务对终端用户的认证授权往往在网关层就通过OAuth 2.0/OpenID Connect等协议统一处理；对服务之间调用的认证授权通过Service Mesh可以做到零信任安全模式。
