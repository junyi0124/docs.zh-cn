---
title: .NET for Apache Spark 入门
description: 了解如何在 Windows、macOS 和 Ubuntu 上使用 .NET Core 运行 .NET for Apache Spark 应用。
ms.date: 10/09/2020
ms.topic: tutorial
ms.custom: mvc
ms.author: luquinta
author: luisquintanilla
ms.openlocfilehash: 16ccc8f40f290c4bc10f03d1f4d1b296b17f6b11
ms.sourcegitcommit: 34968a61e9bac0f6be23ed6ffb837f52d2390c85
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/17/2020
ms.locfileid: "94687814"
---
# <a name="tutorial-get-started-with-net-for-apache-spark"></a>教程：.NET for Apache Spark 入门

本教程介绍如何在 Windows、macOS 和 Ubuntu 上使用 .NET Core 运行 .NET for Apache Spark 应用。

在本教程中，你将了解：

> [!div class="checklist"]
>
> * 为 .NET for Apache Spark 作环境准备
> * 编写你的第一个 .NET for Apache Spark 应用程序
> * 生成和运行 .NET for Apache Spark 应用程序

## <a name="prepare-your-environment"></a>准备环境

在开始编写应用之前，需要先设置一些必备依赖项。 如果可从命令行环境运行 `dotnet`、`java`、`spark-shell`，则表示你的环境已准备就绪且你可跳到下一部分。 如果无法运行任何或部分命令，请执行以下步骤。

### <a name="1-install-net"></a>1.安装 .NET

要开始构建 .NET 应用，需要下载并安装 .NET SDK（软件开发工具包）。

下载并安装 [.NET Core SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1)。 安装 SDK 会将 `dotnet` 工具链添加到路径。

安装 .NET Core SDK 后，打开一个新的命令提示符或终端，然后运行 `dotnet`。

如果该命令运行并打印出有关如何使用 dotnet 的信息，则可转到下一步。 如果收到 `'dotnet' is not recognized as an internal or external command` 错误，请确保在运行命令之前已打开新的命令提示符或终端。

### <a name="2-install-java"></a>2.安装 Java

安装适用于 Windows 和 macOS 的 [Java 8.1](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)，或适用于 Ubuntu 的 [OpenJDK 8](https://openjdk.java.net/install/)。

选择适用于操作系统的合适版本。 例如，为 Windows x64 计算机选择 jdk-8u201-windows-x64.exe（如下所示），或为 macOS 计算机选择 jdk-8u231-macosx-x64.dmg 。 然后，使用命令 `java` 来验证安装。

![Java 下载](https://dotnet.microsoft.com/static/images/java-jdk-downloads-windows.png?v=6BbJHoNyDO-PyYVciImr5wzh2AW_YHNcyb3p093AwPA)

### <a name="3-install-compression-software"></a>3.安装压缩软件

Apache Spark 以 .tgz 压缩文件的形式下载。 使用提取程序（如 [7-Zip](https://www.7-zip.org/) 或 [WinZip](https://www.winzip.com/)）来提取文件。

### <a name="4-install-apache-spark"></a>4.安装 Apache Spark

[下载并安装 Apache Spark](https://spark.apache.org/downloads.html)。 需要从版本 2.3.* 或者 2.4.0、2.4.1、2.4.3、2.4.4、2.4.5、2.4.6、2.4.7、3.0.0 或 3.0.1 中进行选择（.NET for Apache Spark 与其他版本的 Apache Spark 不兼容）。

以下步骤中使用的命令假定你已[下载并安装 Apache Spark 3.0.1](https://spark.apache.org/downloads.html)。 若想要使用其他版本，请将 3.0.1 替换为适当的版本号。 然后，提取 .tar 文件和 Apache Spark 文件。

要提取嵌套的 .tar 文件：

* 找到你已下载的 spark-3.0.1-bin-hadoop2.7.tgz 文件。
* 右键单击该文件，然后选择“7-Zip”->“提取到此处”。
* spark-3.0.1-bin-hadoop2.7.tar 是与你下载的 .tgz 文件一起创建的 。

要提取 Apache Spark 文件：

* 右键单击 spark-3.0.1-bin-hadoop2.7.tar，然后选择“7-Zip”->“提取文件...” 
* 在“提取到”字段输入“C:\bin” 。
* 取消勾选“提取到”字段下面的复选框。
* 选择“确定”。
* Apache Spark 文件会提取到 C:\bin\spark-3.0.1-bin-hadoop2.7\

![安装 Spark](./media/spark-extract-with-7-zip.png)

运行以下命令，以设置用于查找 Apache Spark 的环境变量。 在 Windows 上，确保在管理员模式下运行命令提示符。

#### <a name="windows"></a>[Windows](#tab/windows)

```console
setx /M HADOOP_HOME C:\bin\spark-3.0.1-bin-hadoop2.7\
setx /M SPARK_HOME C:\bin\spark-3.0.1-bin-hadoop2.7\
setx /M PATH "%PATH%;%HADOOP_HOME%;%SPARK_HOME%\bin"
```

#### <a name="maclinux"></a>[Mac/Linux](#tab/linux)

```bash
export SPARK_HOME=~/bin/spark-3.0.1-bin-hadoop2.7/
export PATH="$SPARK_HOME/bin:$PATH"
source ~/.bashrc
```

---

安装所有内容并设置环境变量后，打开新的命令提示符或终端并运行以下命令：

```text
spark-submit --version
```

如果该命令运行并打印出版本信息，则可转到下一步。

如果收到 `'spark-submit' is not recognized as an internal or external command` 错误，请确保已打开新的命令提示符。

### <a name="5-install-net-for-apache-spark"></a>5.安装 .NET for Apache Spark

从 .NET for Apache Spark GitHub 下载 [Microsoft.Spark.Worker](https://github.com/dotnet/spark/releases)。 例如，如果你在使用 Windows 计算机并计划使用 .NET Core，请[下载 Windows x64 netcoreapp3.1 版本](https://github.com/dotnet/spark/releases)。

要提取 Microsoft.Spark.Worker：

* 找到你已下载的 Microsoft.Spark.Worker.netcoreapp3.1.win-x64-1.0.0.zip 文件。
* 右键单击并选择“7-Zip”->“提取文件...”。
* 在“提取到”字段输入“C:\bin” 。
* 取消勾选“提取到”字段下面的复选框。
* 选择“确定”。

### <a name="6-install-winutils-windows-only"></a>6.安装 WinUtils（仅限 Windows）

.NET for Apache Spark 要求与 Apache Spark 一起安装 WinUtils。 [下载 winutils.exe](https://github.com/steveloughran/winutils/blob/master/hadoop-2.7.1/bin/winutils.exe)。 然后，将 WinUtils 复制到 C:\bin\spark-3.0.1-bin-hadoop2.7\bin。

> [!NOTE]
> 如果你在使用其他版本的 Hadoop（相关批注可参见 Spark 安装文件夹名称的末尾），请[选择与你的 Hadoop 版本兼容的 WinUtils 版本](https://github.com/steveloughran/winutils)。

### <a name="7-set-dotnet_worker_dir-and-check-dependencies"></a>7.设置 DOTNET_WORKER_DIR 并检查依赖项

运行以下命令之一来设置 `DOTNET_WORKER_DIR` 环境变量，.NET 应用使用该变量查找 .NET for Apache Spark 辅助角色二进制文件。 确保将 `<PATH-DOTNET_WORKER_DIR>` 替换为你下载并提取 `Microsoft.Spark.Worker` 的目录。 在 Windows 上，确保在管理员模式下运行命令提示符。

#### <a name="windows"></a>[Windows](#tab/windows)

```console
setx /M DOTNET_WORKER_DIR <PATH-DOTNET-WORKER-DIR>
```

#### <a name="maclinux"></a>[Mac/Linux](#tab/linux)

```bash
export DOTNET_WORKER_DIR=<PATH-DOTNET-WORKER-DIR>
```

---

最后，仔细检查是否可从命令行运行 `dotnet`、`java`、`spark-shell`，然后再转到下一部分。

## <a name="write-a-net-for-apache-spark-app"></a>编写 .NET for Apache Spark 应用

### <a name="1-create-a-console-app"></a>1.创建控制台应用

在命令提示符处或终端中，运行以下命令来创建新的控制台应用程序：

```dotnetcli
dotnet new console -o MySparkApp
cd MySparkApp
```

`dotnet` 命令将创建 `console` 类型的 `new` 应用程序。 `-o` 参数将创建名为 MySparkApp 的目录，其中会存储你的应用并填充必需文件。 `cd MySparkApp` 命令会将目录更改为你创建的应用目录。

### <a name="2-install-nuget-package"></a>2.安装 NuGet 包

要在应用中使用 .NET for Apache Spark，请安装 Microsoft.Spark 包。 在命令提示符处或终端中运行下面的命令：

```dotnetcli
dotnet add package Microsoft.Spark
```

> [!NOTE]
> 除非另行指定，否则本教程使用最新版本的 `Microsoft.Spark` NuGet 包。

### <a name="3-write-your-app"></a>3.编写你的应用

在 Visual Studio Code 中打开 Program.cs 或打开任何文本编辑器，再将所有代码替换为以下内容：

```csharp
using Microsoft.Spark.Sql;
using static Microsoft.Spark.Sql.Functions;

namespace MySparkApp
{
    class Program
    {
        static void Main(string[] args)
        {
            // Create Spark session
            SparkSession spark =
                SparkSession
                    .Builder()
                    .AppName("word_count_sample")
                    .GetOrCreate();

            // Create initial DataFrame
            string filePath = args[0];
            DataFrame dataFrame = spark.Read().Text(filePath);

            //Count words
            DataFrame words =
                dataFrame
                    .Select(Split(Col("value")," ").Alias("words"))
                    .Select(Explode(Col("words")).Alias("word"))
                    .GroupBy("word")
                    .Count()
                    .OrderBy(Col("count").Desc());

            // Display results
            words.Show();

            // Stop Spark session
            spark.Stop();
        }
    }
}
```

[SparkSession](xref:Microsoft.Spark.Sql.SparkSession) 是 Apache Spark 应用程序的入口点，用于管理应用程序的上下文和信息。 使用 [Text](xref:Microsoft.Spark.Sql.DataFrameReader.Text%2A) 方法，`filePath` 指定的文件中的文本数据将读入 [DataFrame](xref:Microsoft.Spark.Sql.DataFrame)。 DataFrame 是一种将数据组织到一组命名列中的方法。 然后，将应用一系列转换来拆分文件中的句子，对每个单词进行分组和计数，并按降序顺序进行排序。 这些操作的结果存储在另一个 DataFrame 中。 请注意，此时不会执行任何操作，因为 .NET for Apache Spark 会对数据进行惰性计算。 在调用 [Show](xref:Microsoft.Spark.Sql.DataFrame.Show%2A) 方法之前，不会向控制台显示 `words` 转换为 DataFrame 的内容，即执行在上面的行中定义的操作。 不再需要 Spark 会话后，请使用 [Stop](xref:Microsoft.Spark.Sql.SparkSession.Stop%2A) 方法停止会话。

### <a name="4-create-data-file"></a>4.创建数据文件

你的应用会处理包含多行文本的文件。 在 MySparkApp 目录中创建一个名为 input.txt 的文件，其中包含以下文本：

```text
Hello World
This .NET app uses .NET for Apache Spark
This .NET app counts words with Apache Spark
```

保存更改并关闭文件。

## <a name="run-your-net-for-apache-spark-app"></a>运行 .NET for Apache Spark 应用

运行以下命令来构建应用程序：

```dotnetcli
dotnet build
```

导航到生成输出目录，并使用 `spark-submit` 命令提交要在 Apache Spark 上运行的应用程序。 确保将 `<version>` 替换为 .NET 辅助角色的版本，并存储包含 input.txt 文件的路径的 `<path-of-input.txt>`。

### <a name="windows"></a>[Windows](#tab/windows)

```console
spark-submit ^
--class org.apache.spark.deploy.dotnet.DotnetRunner ^
--master local ^
microsoft-spark-3-0_2.12-<version>.jar ^
dotnet MySparkApp.dll <path-of-input.txt>
```

### <a name="maclinux"></a>[Mac/Linux](#tab/linux)

```bash
spark-submit \
--class org.apache.spark.deploy.dotnet.DotnetRunner \
--master local \
microsoft-spark-3-0_2.12-<version>.jar \
dotnet MySparkApp.dll <path-of-input.txt>
```

---

> [!NOTE]
> 此命令假设已下载 Apache Spark 并添加到 PATH 环境变量，以便能够使用 `spark-submit`。 如果不是，需要使用完整路径（例如 C:\bin\apache-spark\bin\spark-submit 或 ~/spark/bin/spark-submit）。

在应用运行时，input.txt 的字数统计数据会写入控制台。

```console
+------+-----+
|  word|count|
+------+-----+
|  .NET|    3|
|Apache|    2|
|   app|    2|
|  This|    2|
| Spark|    2|
| World|    1|
|counts|    1|
|   for|    1|
| words|    1|
|  with|    1|
| Hello|    1|
|  uses|    1|
+------+-----+
```

祝贺你！ 你已成功创建并运行 .NET for Apache Spark 应用。

## <a name="next-steps"></a>后续步骤

在本教程中，你了解了如何执行以下操作：
> [!div class="checklist"]
>
> * 为 .NET for Apache Spark 作环境准备
> * 编写你的第一个 .NET for Apache Spark 应用程序
> * 生成和运行 .NET for Apache Spark 应用程序

若要观看解说上述步骤的视频，请查看 [.NET for Apache Spark 101 视频系列](https://channel9.msdn.com/Series/NET-for-Apache-Spark-101/Run-Your-First-NET-for-Apache-Spark-App)。

查看资源页以了解详细信息。
> [!div class="nextstepaction"]
> [.NET for Apache Spark 资源](../resources/index.md)
