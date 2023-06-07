---
title: 将 .NET for Apache Spark 连接到 MongoDB
description: 了解如何从 .NET for Apache Spark 应用程序连接到 MongoDB 实例。
ms.author: nidutta
author: Niharikadutta
ms.date: 10/09/2020
ms.topic: conceptual
ms.custom: mvc,how-to
ms.openlocfilehash: 945e494e8a027d438bf4659d989da6033a13f6f0
ms.sourcegitcommit: 34968a61e9bac0f6be23ed6ffb837f52d2390c85
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/17/2020
ms.locfileid: "94687598"
---
# <a name="connect-net-for-apache-spark-to-mongodb"></a>将 .NET for Apache Spark 连接到 MongoDB

本文介绍如何从 .NET for Apache Spark 应用程序连接到 MongoDB 实例。

## <a name="prerequisites"></a>先决条件

1. 启动并运行 MongoDB 服务器，向其添加一个[数据库和一些集合](https://docs.mongodb.com/manual/core/databases-and-collections/)（对于本地服务器，请下载[此社区服务器](https://www.mongodb.com/try/download/community)，或者对于 MongoDB 云服务，可以尝试 [MongoDB Atlas](https://www.mongodb.com/cloud/atlas)。）

## <a name="set-up-your-mongodb-instance"></a>设置 MongoDB 实例

若要让 .NET for Apache Spark 与 MongoDB 实例进行通信，你需要执行以下操作，确保它已正确设置：

1. 为应用程序创建一个用户名和密码以进行连接，并通过 mongo shell 使用以下命令为用户提供必要的权限/角色：

    ```bash
    use database
    db.createUser(
      {
        user: "mySparkUser",
        pwd: "<password>",
        roles: [ { role: "userAdminAnyDatabase", db: "admin" }, "readWriteAnyDatabase" ]
      }
    )
    ```

2. 请确保运行 .NET for Apache Spark 应用程序的计算机的 IP 地址已列入允许清单，以便 MongoDB 服务器能够连接。 你可以参考[本指南](https://docs.atlas.mongodb.com/security/add-ip-address-to-list/)了解如何执行此操作。

## <a name="configure-your-net-for-apache-spark-application"></a>配置 .NET for Apache Spark 应用程序

1. 设置以下变量，将应用程序配置为与 MongoDB 实例通信并从集合中读取。
    1. **authURI**："授权应用程序连接到所需 MongoDB 实例的连接字符串"。 其格式如下：

        ```
        "mongodb+srv://<username>:<password>@<cluster_address>/<database>.<collection>"
        ```

    2. **username**：在上一部分的步骤 1 中创建的帐户用户名
    3. **password**：创建的用户帐户的密码
    4. **cluster_address**：MongoDB 群集的主机名/地址
    5. **database**：要连接到的 MongoDB 数据库
    6. **collection**：要读取的 MongoDB 集合。 （在本例中，我们将使用每个 Apache Spark 安装中提供的标准 [`people.json`](https://github.com/apache/spark/blob/master/examples/src/main/resources/people.json) 示例文件。）

2. 使用的 `com.mongodb.spark.sql.DefaultSource` 格式为 `spark.Read()`，如下面简单的代码片段中所示：

    ```csharp
    class Program
    {
        static void Main()
        {
            var authURI = "mongodb+srv://<username>:<password>@<cluster_address>/<database>.<collection>?retryWrites=true&w=majority";
            SparkSession spark = SparkSession
                .Builder()
                .AppName("Connect to Mongo DB example")
                .Config("spark.mongodb.input.uri", authURI)
                .GetOrCreate();

            DataFrame df = spark.Read().Format("com.mongodb.spark.sql.DefaultSource").Load();
            df.PrintSchema();
            df.Show();
            spark.Stop();
        }
    }
    ```

## <a name="run-your-application"></a>运行应用程序

为了运行 .NET for Apache Spark 应用程序，应在 Spark 项目中将 `mongo-spark-connector` 模块定义为生成定义的一部分，对于 sbt 项目，请使用 `build.sbt` 中的 `libraryDependency`。 对于 Spark 环境（如 `spark-submit` 或 `spark-shell`），请使用 `--packages` 命令行选项，如下所示：

```bash
spark-submit --master local --packages org.mongodb.spark:mongo-spark-connector_2.12:3.0.0 --class org.apache.spark.deploy.dotnet.DotnetRunner microsoft-spark-<spark_majorversion-spark_minorversion>_<scala_majorversion.scala_minorversion>-<spark_dotnet_version>.jar yourApp.exe
```

> [!NOTE]
> 请确保包含与正在运行的 Spark 版本相符的包版本。

显示的结果为 DataFrame (`df`)，如下所示：

```text
+--------------------+----+-------+
|                 _id| age|   name|
+--------------------+----+-------+
|[5f7c28438029a134...|null|Michael|
|[5f7c287f8029a134...|  30|   Andy|
|[5f7c289a8029a134...|  19| Justin|
+--------------------+----+-------+
```
