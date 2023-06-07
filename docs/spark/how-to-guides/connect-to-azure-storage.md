---
title: 从本地计算机连接到远程存储
description: 使用 .NET for Apache Spark 从本地计算机连接到 Azure 存储帐户。
ms.author: nidutta
author: Niharikadutta
ms.date: 10/09/2020
ms.topic: conceptual
ms.custom: mvc,how-to
ms.openlocfilehash: dc0c3b44279756596f3d456616821e690710ae04
ms.sourcegitcommit: 67ebdb695fd017d79d9f1f7f35d145042d5a37f7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/20/2020
ms.locfileid: "92224018"
---
# <a name="connect-to-azure-data-lake-storage-gen-2-or-wasb-account"></a>连接到 Azure Data Lake Storage Gen 2 或 WASB 帐户

本文介绍如何通过 Windows 计算机上本地运行的 [.NET for Apache Spark](https://github.com/dotnet/spark) 实例连接到 Azure Data Lake Storage (ADLS) Gen 2 或 Windows Azure Storage Blob (WASB) 帐户。

## <a name="set-up-the-environment"></a>设置环境

1. 请从[官方网站](https://archive.apache.org/dist/spark/)下载无 Hadoop 的 Apache Spark 分发版（选择一个 [.NET for Apache Spark](https://github.com/dotnet/spark#supported-apache-spark) 支持的版本），并将其解压缩到某个目录中。 将 `SPARK_HOME` 环境变量设置为此目录。
2. 从[此处](http://hadoop.apache.org/releases.html)下载 Apache Hadoop 二进制文件并将其解压缩到一个文件夹中，然后将 `HADOOP_HOME` 环境变量设置为此文件夹。
3. 从[此位置](https://github.com/cdarlint/winutils)下载 `winutils.exe` 和 `hadoop.dll` 文件（具体取决于上一步中安装的 Hadoop 版本），并将它们放在 Hadoop 的 bin 文件夹中。 Windows 上需要这些二进制文件才能正确设置所有内容（[此处的 Apache Wiki](https://cwiki.apache.org/confluence/display/HADOOP2/WindowsProblems) 对其进行了详细介绍）。
4. 对 `%HADOOP_HOME%\etc\hadoop\hadoop-env.cmd` 文件进行以下更改，以配置 Hadoop 安装：
    1. 使用 DOS 路径设置 `JAVA_HOME` 属性（因为 Hadoop 不太支持在 `JAVA_HOME` 中使用空格）。 具体类似如下：`C:\Progra~1\Java\jdk1.8.0_241`（指向本地计算机上安装的任何 Java 版本）。
    2. 将 `%HADOOP_HOME%\share\hadoop\tools\lib\*` 追加到 `HADOOP_CLASSPATH`。
    最终的 `hadoop-env.cmd` 文件应如下所示：

    ![最终的 hadoop-env.cmd 文件](./media/connect-external-sources/hadoop-env.png)

## <a name="configure-your-storage-account-in-hadoop"></a>在 Hadoop 中配置存储帐户

1. 打开要通过 [Azure门户](https://portal.azure.com)连接的 ADLS Gen 2 或 WASB 存储帐户，然后打开“设置”边栏选项卡下的“访问密钥”面板，复制 key1 下的密钥值  。
2. 现在，为了配置 Hadoop 以访问 ADLS Gen2 帐户，请务必编辑 `core-site.xml`（位于 `%HADOOP_HOME%\etc\hadoop\`）文件，其中包含群集范围的配置。 将以下属性添加到此文件中的 `<configuration>` 标记内：

    ```xml
    <configuration>
      <property>
        <name>fs.azure.account.auth.type.YOUR_ACCOUNT_NAME.dfs.core.windows.net</name>
        <value>SharedKey</value>
        <description></description>
      </property>
      <property>
        <name>fs.azure.account.key.YOUR_ACCOUNT_NAME.dfs.core.windows.net</name>
        <value>YOUR ACCESS KEY (copied from Step 1)</value>
        <description>The secret password. Never share these.</description>
      </property>
    </configuration>
    ```

    如果尝试连接到 WASB 帐户，请将属性名称中的 `dfs` 替换为 `blob`。 例如，`fs.azure.account.auth.type.YOUR_ACCOUNT_NAME.blob.core.windows.net`。
3. 你可以在 `%HADOOP_HOME%` 目录中运行以下命令，通过 Hadoop 测试是否连接到存储帐户：

    ```bash
    bin\hdfs dfs -ls <URI to your account>
    ```

这会显示 URI 提供的路径中的所有文件/文件夹列表。

> [!NOTE]
> 为存储帐户派生 URI 的格式如下：
>
> ```
> For ADLS: abfs[s]://<file_system>@<account_name>.dfs.core.windows.net/<path>/<file_name>
> For WASB: wasb[s]://<file_system>@<account_name>.blob.core.windows.net/<path>/<file_name>
> ```

## <a name="connect-to-your-storage-account"></a>连接到你的存储帐户

1. 如果以上命令正常运行，现可继续通过 Spark 访问此存储帐户。 首先从 `%HADOOP_HOME%` 中的命令行运行命令 `hadoop classpath`，然后复制输出。
2. 将步骤 1 中运行的命令的输出设置为环境变量 `SPARK_DIST_CLASSPATH` 的值。
3. 现在，你应该能够使用 abfs URI 通过 Spark .NET 访问 ADLS 或 WASB 存储帐户，如下面的简单示例所示。 （在本例中，我们将使用每个 Apache Spark 安装中提供的标准 [`people.json`](https://github.com/apache/spark/blob/master/examples/src/main/resources/people.json) 示例文件）：

    ```csharp
    SparkSession spark = SparkSession
       .Builder()
       .AppName("Connect to Azure Storage locally")
       .GetOrCreate();
    DataFrame df = spark.Read().Json("wasbs://file_system@account_name.blob.core.windows.net/path/people.json");
    //DataFrame df = spark.Read().Json("abfss://file_system@account_name.dfs.core.windows.net/path/file.json");
    df.Show();
    ```

    显示的结果为 DataFrame (`df`)，如下所示：

    ```text
    +----+-------+
    | age|   name|
    +----+-------+
    |null|Michael|
    |  30|   Andy|
    |  19| Justin|
    +----+-------+
    ```
