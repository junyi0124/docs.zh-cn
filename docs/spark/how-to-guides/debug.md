---
title: 在 Windows 上部署 .NET for Apache Spark 应用程序
description: 了解如何在 Windows 上部署 .NET for Apache Spark 应用程序。
ms.date: 10/09/2020
ms.topic: conceptual
ms.custom: mvc,how-to
ms.openlocfilehash: 43531e6b2f9a79658f89b804dfa2bb97d6e9645b
ms.sourcegitcommit: b59237ca4ec763969a0dd775a3f8f39f8c59fe24
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/12/2020
ms.locfileid: "91954979"
---
# <a name="debug-a-net-for-apache-spark-application"></a>部署 .NET for Apache Spark 应用程序

此操作说明介绍在 Windows 上部署 .NET for Apache Spark 应用程序的步骤。

## <a name="debug-your-application"></a>调试应用程序

打开“新命令提示符”窗口并运行以下命令：

```shell
spark-submit \
  --class org.apache.spark.deploy.dotnet.DotnetRunner \
  --master local \
  <path-to-microsoft-spark-jar> \
  debug
```

运行命令时，会看到以下输出：

```console
***********************************************************************
* .NET Backend running debug mode. Press enter to exit *
***********************************************************************
```

在调试模式下，DotnetRunner 不启动 .NET 应用程序，而是等待你启动 .NET 应用。 将此命令提示符窗口保持为打开状态，并通过 C# 调试器启动 .NET 应用程序以调试应用程序。 使用 C# 调试器（[适用于 Windows/macOS 的 Visual Studio 调试器](https://visualstudio.microsoft.com/vs/)或 [Visual Studio Code 中的 C# 调试器扩展](https://code.visualstudio.com/Docs/editor/debugging)）启动 .NET 应用程序以调试应用程序。

## <a name="debug-a-user-defined-function-udf"></a>调试用户定义函数 (UDF)

> [!NOTE]
> 只有安装了 Visual Studio 调试器的 Windows 上支持用户定义函数。

在运行 `spark-submit`之前，请设置以下环境变量：

```bat
set DOTNET_WORKER_DEBUG=1
```

运行 Spark 应用程序时，将弹出 `Choose Just-In-Time Debugger` 窗口。 选择 Visual Studio 调试器。

调试器将在 [TaskRunner.cs](https://github.com/dotnet/spark/blob/5e9c08b430b4bc56b5f42252c4b73437377afaed/src/csharp/Microsoft.Spark.Worker/TaskRunner.cs#L52) 中的以下位置处中断：

```csharp
if (EnvironmentUtils.GetEnvironmentVariableAsBool("DOTNET_WORKER_DEBUG"))
{
    Debugger.Launch(); // <-- The debugger will break here.
}
```

导航到包含计划调试的 UDF 的 .cs 文件，并[设置断点](/visualstudio/debugger/using-breakpoints?view=vs-2019)。 断点将显示 `The breakpoint will not currently be hit`，因为工作线程尚未加载包含 UDF 的程序集。

点击 `F5` 继续应用程序，最终会点击断点。

> [!NOTE]
> 将为每个任务弹出“选择实时调试器”窗口。 若要避免弹出过多窗口，请将执行器数设置为较小数目。 例如，可以将“--master local[1]”选项用于 spark-submit，以将任务数设置为 1，这将启动一个调试器实例。

## <a name="debug-scala-code"></a>调试 Scala 代码

如果需要调试 Scala 端代码（`DotnetRunner`、`DotnetBackendHandler` 等），可以使用以下命令，并使用 [IntelliJ](https://www.jetbrains.com/help/idea/attaching-to-local-process.html) 将调试程序附加到正在运行的进程：

```shell
spark-submit \
  --driver-java-options -agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=5005 \
  --class org.apache.spark.deploy.dotnet.DotnetRunner \
  --master local \
  <path-to-microsoft-spark-jar> \
  <path-to-your-app-exe> <argument(s)-to-your-app>
```

运行命令后，使用 [Intellij](https://www.jetbrains.com/help/idea/attaching-to-local-process.html) 将调试程序附加到正在运行的进程。

## <a name="next-steps"></a>后续步骤

* [.NET for Apache Spark 入门](../tutorials/get-started.md)
* [将 .NET for Apache Spark 应用程序部署到 Azure HDInsight](../tutorials/hdinsight-deployment.md)
* [将 .NET for Apache Spark 应用程序部署到 Databricks](../tutorials/databricks-deployment.md)
* [将 .NET for Apache Spark 应用程序部署到 Amazon EMR Spark](../tutorials/amazon-emr-spark-deployment.md)
