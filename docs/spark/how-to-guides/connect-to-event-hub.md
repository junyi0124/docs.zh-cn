---
title: 将 .NET for Apache Spark 连接到 Azure 事件中心
description: 了解如何从本地 .NET for Apache Spark 实例连接到 Azure 事件中心。
ms.author: nidutta
author: Niharikadutta
ms.date: 10/09/2020
ms.topic: conceptual
ms.custom: mvc,how-to
ms.openlocfilehash: c8fd10992e63674032af4148e0673a5330d9086c
ms.sourcegitcommit: 67ebdb695fd017d79d9f1f7f35d145042d5a37f7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/20/2020
ms.locfileid: "92223968"
---
# <a name="connect-net-for-apache-spark-to-azure-event-hubs"></a>将 .NET for Apache Spark 连接到 Azure 事件中心

本文介绍如何将 [.NET for Apache Spark](https://github.com/dotnet/spark) 应用程序连接到 Azure 事件中心，以读取和写入 Apache Kafka 流。

## <a name="prerequisites"></a>先决条件

使用事件中心准备好事件中心命名空间。 有关查看分步指南，请参阅 [快速入门：使用 Azure 门户创建事件中心](/azure/event-hubs/event-hubs-create)。 创建事件中心命名空间时，请确保选择标准定价层。

## <a name="what-is-azure-event-hubs"></a>什么是 Azure 事件中心

[Azure 事件中心](/azure/event-hubs/event-hubs-about)是大数据流式处理平台和事件引入服务。 它是一种完全托管的平台即服务 (PaaS)，可以轻松地与 [Apache Kafka](https://kafka.apache.org/) 集成，为你提供无需管理、配置或运行自己的群集的 PaaS Kafka 体验。

## <a name="connect-your-application-to-azure-event-hubs"></a>将应用程序连接到 Azure 事件中心

1. 获取事件中心连接字符串和完全限定域名 (FQDN) 供以后使用。 有关说明，请参阅[获取事件中心连接字符串](/azure/event-hubs/event-hubs-get-connection-string)。
2. 在代码中使用命名空间中的详细信息设置以下配置，开始从适用于 Kafka 的事件中心读取：
    1. 在应用程序中更新 BOOTSTRAP_SERVERS 和 EH_SASL ：

    ```csharp
    string BOOTSTRAP_SERVERS = "hostname:9093"; // 9093 is the port used to communicate with Event Hubs, see [troubleshooting guide](https://docs.microsoft.com/azure/event-hubs/troubleshooting-guide)
    string EH_SASL = "org.apache.kafka.common.security.plain.PlainLoginModule required username=\"$ConnectionString\" password=\"<CONNECTION_STRING>\";"; // Connection string obtained from Step 1
    ```

## <a name="read-from-azure-event-hub-stream"></a>从 Azure 事件中心流读取

你现在可以使用事件中心进行流式处理，就像使用 Kafka 一样。 示例代码如下所示：

```csharp
SparkSession spark = SparkSession
    .Builder()
    .AppName("Connect Event Hub")
    .GetOrCreate();

DataFrame df = spark
    .ReadStream()
    .Format("kafka")
    .Option("kafka.bootstrap.servers", BOOTSTRAP_SERVERS)
    .Option("subscribe", "spark-test")
    .Option("kafka.sasl.mechanism", "PLAIN")
    .Option("kafka.security.protocol", "SASL_SSL")
    .Option("kafka.sasl.jaas.config", EH_SASL)
    .Option("kafka.request.timeout.ms", "60000")
    .Option("kafka.session.timeout.ms", "60000")
    .Option("failOnDataLoss", "false")
    .Load();

DataFrame dfWrite = df
    .WriteStream()
    .OutputMode("append")
    .Format("console")
    .Start();
```

## <a name="write-to-azure-event-hub-stream"></a>写入 Azure 事件中心流

你也可以使用相同方式写入事件中心，如下所示：

```csharp
// df is the DataFrame to write

df.WriteStream()
    .Format("kafka")
    .Option("topic", topics)
    .Option("kafka.bootstrap.servers", BOOTSTRAP_SERVERS)
    .Option("kafka.sasl.mechanism", "PLAIN")
    .Option("kafka.security.protocol", "SASL_SSL")
    .Option("kafka.sasl.jaas.config", EH_SASL)
    .Option("checkpointLocation", "./checkpoint")
    .Start();
```

## <a name="run-your-application"></a>运行应用程序

为了运行 .NET for Apache Spark 应用程序，请在 Spark 项目中将 `spark-sql-kafka-0-10` 模块定义为生成定义的一部分，对于 sbt 项目，应使用 `build.sbt` 中的 `libraryDependency`。 对于 Spark 环境（如 `spark-submit` 或 `spark-shell`），请使用 `--packages` 命令行选项，如下所示：

```bash
spark-shell --packages org.apache.spark:spark-sql-kafka-0-10_2.12:2.4.5
```

> [!NOTE]
> 请确保包含与正在运行的 Spark 版本相符的包版本。
