---
layout: post
title: "Lambda architecture for real-time big data processing"
description: "Lambda architecture for real-time big data processing"
categories: [Distributed-Computing]
tags: [Lambda架构, 大数据, 实时处理]
redirect_from:
  - /2018/12/21/
---

* Kramdown table of contents
{:toc .toc}

# Lambda architecture for real-time big data processing

本文转载自 [可口可乐的CSDN博客](https://blog.csdn.net/brucesea/article/details/45937875)

## 1.Lambda架构背景介绍
Lambda架构是由Storm的作者Nathan Marz提出的一个实时大数据处理框架。Marz在Twitter工作期间开发了著名的实时大数据处理框架Storm，Lambda架构是其根据多年进行分布式大数据系统的经验总结提炼而成。  
Lambda架构的目标是设计出一个能满足实时大数据系统关键特性的架构，包括有：高容错、低延时和可扩展等。Lambda架构整合离线计算和实时计算，融合不可变性（Immutability），读写分离和复杂性隔离等一系列架构原则，可集成Hadoop，Kafka，Storm，Spark，HBase等各类大数据组件。

## 2.大数据系统的关键特性
Marz认为大数据系统应具有以下的关键特性：
+ Robust and fault-tolerant（容错性和鲁棒性）：对大规模分布式系统来说，机器是不可靠的，可能会当机，但是系统需要是健壮、行为正确的，即使是遇到机器错误。除了机器错误，人更可能会犯错误。在软件开发中难免会有一些Bug，系统必须对有Bug的程序写入的错误数据有足够的适应能力，所以比机器容错性更加重要的容错性是人为操作容错性。对于大规模的分布式系统来说，人和机器的错误每天都可能会发生，如何应对人和机器的错误，让系统能够从错误中快速恢复尤其重要。
+ Low latency reads and updates（低延时）：很多应用对于读和写操作的延时要求非常高，要求对更新和查询的响应是低延时的。
+ Scalable（横向扩容）：当数据量/负载增大时，可扩展性的系统通过增加更多的机器资源来维持性能。也就是常说的系统需要线性可扩展，通常采用scale out（通过增加机器的个数）而不是scale up（通过增强机器的性能）。
+ General（通用性）：系统需要能够适应广泛的应用，包括金融领域、社交网络、电子商务数据分析等。
+ Extensible（可扩展）：需要增加新功能、新特性时，可扩展的系统能以最小的开发代价来增加新功能。
+ Allows ad hoc queries（方便查询）：数据中蕴含有价值，需要能够方便、快速的查询出所需要的数据。
+ Minimal maintenance（易于维护）：系统要想做到易于维护，其关键是控制其复杂性，越是复杂的系统越容易出错、越难维护。
+ Debuggable（易调试）：当出问题时，系统需要有足够的信息来调试错误，找到问题的根源。其关键是能够追根溯源到每个数据生成点。

## 3.数据系统的本质
为了设计出能满足前述的大数据关键特性的系统，我们需要对数据系统有本质性的理解。我们可将数据系统简化为：
```
数据系统 = 数据 + 查询
```
从而从数据和查询两方面来认识大数据系统的本质。
