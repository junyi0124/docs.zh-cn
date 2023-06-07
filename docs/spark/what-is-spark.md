---
title: 什么是 Apache Spark？
description: 了解 Apache Spark 和大数据方案。
ms.date: 10/15/2019
ms.topic: conceptual
ms.custom: mvc
ms.openlocfilehash: cde66c4084b7c86e1b78d89c2bad94402dbd7d60
ms.sourcegitcommit: b7a8b09828bab4e90f66af8d495ecd7024c45042
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/04/2020
ms.locfileid: "87555989"
---
# <a name="what-is-apache-spark"></a>什么是 Apache Spark？

[Apache Spark](https://spark.apache.org/) 是一种开放源代码并行处理框架，支持使用内存中处理来提升大数据分析应用程序的性能。 大数据解决方案旨在处理对传统数据库来说太大或太复杂的数据。 Spark 处理内存中的大量数据，这比基于磁盘的替代方法要快得多。

## <a name="common-big-data-scenarios"></a>常见的大数据方案

如果需要存储和处理大量数据、转换非结构化数据或处理流数据，则可以考虑使用大数据体系结构。 Spark 是一种通用的分布式处理引擎，可用于多个大数据方案。

### <a name="extract-transform-and-load-etl"></a>提取、转换和加载 (ETL)

[提取、转换和加载 (ETL)](/azure/architecture/data-guide/relational-data/etl) 是从一个或多个源收集数据、修改数据以及将数据移动到新数据存储的过程。 可通过多种方法来转换数据，其中包括：

* Filtering
* 排序
* 聚合
* 联接
* 清理
* 删除重复项
* 正在验证

### <a name="real-time-data-stream-processing"></a>实时数据流处理

流数据（或实时数据）是移动数据。 IoT 设备、网络日志和点击流中的遥测都是流数据示例。 可以处理实时数据以提供有用的信息，例如地理空间分析、远程监视和异常情况检测。 就像关系数据一样，可以在将数据移动到输出接收器之前筛选、聚合和准备流数据。 Apache Spark 通过 [Spark Streaming](https://spark.apache.org/streaming/) 支持[实时数据流处理](/azure/architecture/data-guide/big-data/real-time-processing)。

### <a name="batch-processing"></a>批处理

[批处理](/azure/architecture/data-guide/big-data/batch-processing)是对静态大数据的处理。 可以使用并行的长时间运行的作业来筛选、聚合和准备非常大的数据集。

### <a name="machine-learning-through-mllib"></a>通过 MLlib 进行机器学习

机器学习用于处理高级分析问题。 计算机可以使用现有数据来预测将来的行为、结果和趋势。 Apache Spark 的机器学习库 [MLlib](https://spark.apache.org/mllib/) 包含多个机器学习算法和实用工具。

### <a name="graph-processing-through-graphx"></a>通过 GraphX 进行图形处理

图形是由边缘连接的节点的集合。 如果有分层数据或有包含互连关系的数据，则可以使用图形数据库。 可以使用 Apache Spark 的 [GraphX](https://spark.apache.org/graphx/) API 来处理此数据。

### <a name="sql-and-structured-data-processing-with-spark-sql"></a>采用 Spark SQL 的 SQL 和结构化数据处理

如果使用的是结构化（格式化）数据，则可以在使用 [Spark SQL](https://spark.apache.org/sql/) 的 Spark 应用程序中使用 SQL 查询。

## <a name="apache-spark-architecture"></a>Apache Spark 体系结构

使用主角色/辅助角色体系结构的 Apache Spark 有三个主要组件：驱动程序、执行程序和群集管理器。

![Apache Spark 体系结构](media/spark-architecture.png)

### <a name="driver"></a>驱动程序

驱动程序由程序（如 C# 控制台应用）和 Spark 会话组成。 Spark 会话会对程序进行处理，将其分成由执行程序处理的较小任务。

### <a name="executors"></a>执行程序

每个执行程序或工作器节点会接收来自驱动程序的任务，并执行该任务。 执行程序驻留在称为群集的实体上。

### <a name="cluster-manager"></a>群集管理器

群集管理器与驱动程序和执行程序进行通信，以执行以下操作：

* 管理资源分配
* 管理程序部分
* 管理程序执行

## <a name="language-support"></a>语言支持

Apache Spark 支持以下编程语言：

* Scala
* Python
* Java
* SQL
* R
* .NET 语言 (C#/F#)

## <a name="spark-apis"></a>Spark API

Apache Spark 支持以下 API：

* [Spark Scala API](https://spark.apache.org/docs/2.2.0/api/scala/index.html)
* [Spark Java API](https://spark.apache.org/docs/2.2.0/api/java/index.html)
* [Spark Python API](https://spark.apache.org/docs/2.2.0/api/python/index.html)
* [Spark R API](https://spark.apache.org/docs/2.2.0/api/R/index.html)
* [Spark SQL](https://spark.apache.org/docs/latest/api/sql/index.html)（内置函数）

## <a name="next-steps"></a>后续步骤

了解如何在 .NET 应用程序中使用 Apache Spark。 使用 .NET for Apache Spark，具有 .NET 经验和业务逻辑的开发人员可使用 C# 和 F# 编写大数据查询。
> [!div class="nextstepaction"]
> [什么是 .NET for Apache Spark](what-is-apache-spark-dotnet.md)
