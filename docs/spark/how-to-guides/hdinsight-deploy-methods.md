---
title: 将 .NET for Apache Spark 作业提交到 Azure HDInsight
description: 了解如何使用 spark-submit 和 Apache Livy 将 .NET for Apache Spark 作业提交到 Azure HDInsight。
ms.date: 10/09/2020
ms.topic: conceptual
ms.custom: mvc,how-to
ms.openlocfilehash: 74be4ecf33a9a881633da0630fa1c1a4bf0b19c6
ms.sourcegitcommit: 34968a61e9bac0f6be23ed6ffb837f52d2390c85
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/17/2020
ms.locfileid: "94688158"
---
# <a name="submit-a-net-for-apache-spark-job-to-azure-hdinsight"></a>将 .NET for Apache Spark 作业提交到 Azure HDInsight

可通过两种方法将 .NET for Apache Spark 作业部署到 HDInsight：`spark-submit` 和 Apache Livy。

## <a name="deploy-using-spark-submit"></a>使用 spark-submit 进行部署

可以使用 [spark-submit](https://spark.apache.org/docs/latest/submitting-applications.html) 命令将 .NET for Apache Spark 作业提交到 Azure HDInsight。

1. 导航到 Azure 门户中的 HDInsight Spark 群集，然后选择“SSH + 群集登录名”  。

2. 复制 ssh 登录信息，并将登录名粘贴到终端。 使用在群集创建期间设置的密码登录到群集。 应会看到“欢迎使用 Ubuntu 和 Spark”的消息。

3. 使用 spark-submit 命令在 HDInsight 群集上运行应用  。 请记得将示例脚本中的 mycontainer 和 mystorageaccount 替换为 blob 容器和存储帐户的实际名称   。 另外，还需将 microsoft-spark jar 替换为要使用的 Spark 和 .NET for Apache Spark 版本。

   ```bash
   $SPARK_HOME/bin/spark-submit \
   --master yarn \
   --class org.apache.spark.deploy.dotnet.DotnetRunner \
   wasbs://mycontainer@mystorageaccount.blob.core.windows.net/microsoft-spark-<spark_majorversion-spark_minorversion>_<scala_majorversion.scala_minorversion>-<spark_dotnet_version>.jar \
   wasbs://mycontainer@mystorageaccount.blob.core.windows.net/publish.zip mySparkApp
   ```

## <a name="deploy-using-apache-livy"></a>使用 Apache Livy 进行部署

可以使用 [Apache Livy](https://livy.incubator.apache.org/)（即 Apache Spark REST API）将 .NET for Apache Spark 作业提交到 Azure HDInsight Spark 群集。 有关详细信息，请参阅[使用 Apache Livy 提交远程作业](/azure/hdinsight/spark/apache-spark-livy-rest-interface)。

可使用 `curl` 在 Linux 上运行以下命令：

```bash
curl -k -v -X POST "https://<your spark cluster>.azurehdinsight.net/livy/batches" \
-u "<hdinsight username>:<hdinsight password>" \
-H "Content-Type: application/json" \
-H "X-Requested-By: <hdinsight username>" \
-d @- << EOF
{
    "file":"abfss://<your-file-system-name>@<your-storage-account-name>.dfs.core.windows.net/<some dir>/microsoft-spark-<spark_majorversion-spark_minorversion>_<scala_majorversion.scala_minorversion>-<spark_dotnet_version>.jar",
    "className":"org.apache.spark.deploy.dotnet.DotnetRunner",
    "files":["abfss://<your-file-system-name>@<your-storage-account-name>.dfs.core.windows.net/<some dir>/<udf assembly>", "abfss://<your-file-system-name>@<your-storage-account-name>.dfs.core.windows.net/<some dir>/<file>"],
    "args":["abfss://<your-file-system-name>@<your-storage-account-name>.dfs.core.windows.net/<some dir>/<your app>.zip","<your app>","<app arg 1>","<app arg 2>,"...","<app arg n>"]
}
EOF
```

## <a name="next-steps"></a>后续步骤

* [.NET for Apache Spark 入门](../tutorials/get-started.md)
* [将 .NET for Apache Spark 应用程序部署到 Azure HDInsight](../tutorials/hdinsight-deployment.md)
* [HDInsight 文档](/azure/hdinsight/)
