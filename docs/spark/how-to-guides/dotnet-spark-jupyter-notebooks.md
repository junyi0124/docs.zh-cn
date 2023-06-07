---
title: 使用 Jupyter Notebook
titleSuffix: .NET for Apache Spark
description: 在 Jupyter Notebook、Jupyter Lab 或 Visual Studio Code (VS Code) 等交互式环境中使用 .NET for Apache Spark
ms.author: luquinta
author: luisquintanilla
ms.date: 10/09/2020
ms.topic: conceptual
ms.custom: mvc, how-to
ms.openlocfilehash: cf19b4e4b7a7b9033fb97b2b2736ab0383c11f93
ms.sourcegitcommit: 9d525bb8109216ca1dc9e39c149d4902f4b43da5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/04/2020
ms.locfileid: "96598928"
---
# <a name="use-net-for-apache-spark-in-jupyter-notebooks"></a>在 Jupyter Notebook 中使用 .NET for Apache Spark

本文介绍如何在 Jupyter Notebook 和 Visual Studio Code (VS Code) 中使用 .NET Interactive 以交互方式运行 .NET for Apache Spark。

## <a name="about-jupyter"></a>关于 Jupyter

[Jupyter](https://jupyter.org/) 是一种开源的跨平台计算环境，可让用户以交互方式构建和开发应用程序。 你可以通过各种界面（例如 Jupyter Notebook、Jupyter Lab 和 VS Code）与 Jupyter 交互。

在 .NET 上下文中，.net Core 全局工具 [.NET Interactive](https://github.com/dotnet/interactive) 提供了一个用于在 Jupyter Notebook 等交互式计算环境中编写 .NET 代码 (C#/F#) 的内核。

## <a name="prerequisites"></a>先决条件

- [.NET Core 3.1 SDK](../../core/install/index.yml)
- Apache Spark[](https://spark.apache.org/downloads.html)
- [Apache Spark .NET 工作器](https://github.com/dotnet/spark/releases)

请参阅[入门教程](../tutorials/get-started.md)，详细了解如何设置 .NET for Apache Spark 环境。

## <a name="prepare-environment"></a>准备环境

若要使用 Jupyter Notebook，需要完成两项操作。

1. 安装 [.NET Interactive 全局 .NET 工具](https://github.com/dotnet/interactive/blob/main/docs/NotebooksLocalExperience.md)
1. 下载 `Microsoft.Spark` NuGet 包。
    1. 导航到 [Microsoft.Spark](https://www.nuget.org/packages/Microsoft.Spark/) NuGet 包页面。

        > [!IMPORTANT]
        > 默认下载最新版本的包。 **请确保下载的版本与 Apache Spark .NET 工作器版本相同。**

    1. 在“信息”窗格中，选择“下载包”以下载最新版本的包 。 包的名称类似于 microsoft.spark.[PACKAGE-VERSION].nupkg。
    1. 解压缩下载的包。 解压缩的目录应包含名为 jars 的子目录。 记下该路径，因为稍后将使用它。

## <a name="start-net-for-apache-spark"></a>启动 .NET for Apache Spark

运行以下命令，在调试模式下启动 .NET for Apache Spark。 此 `spark-submit` 命令启动一个进程，并等待来自 [SparkSession](xref:Microsoft.Spark.Sql.SparkSession) 的连接。 确保为正在使用的相应版本的 .NET for Apache Spark 提供 `microsoft-spark-<spark_majorversion-spark_minorversion>_<scala_majorversion.scala_minorversion>-<spark_dotnet_version>.jar` 路径。

**Ubuntu**

```bash
spark-submit \
    --class org.apache.spark.deploy.dotnet.DotnetRunner \
    --master local \
    <path-to-microsoft-spark-jar> \
    debug
```

**Windows**

```cmd
spark-submit ^
    --class org.apache.spark.deploy.dotnet.DotnetRunner ^
    --master local ^
    <path-to-microsoft-spark-jar> ^
    debug
```

## <a name="create-a-notebook"></a>创建笔记本

你可以使用不同的界面与 Jupyter 交互。 若要使用基于浏览器的界面，请使用 Jupyter Notebook 或 Jupyter Lab。 如果需要本地编辑器的体验，请使用 VS Code。

### <a name="jupyter-notebooks--jupyter-lab"></a>Jupyter Notebooks 和 Jupyter Lab

1. 在另一个命令提示符处，使用以下命令之一启动 Jupyter Notebook 或 Jupyter Lab：

    **Jupyter 笔记本**

    ```bash
    jupyter notebook
    ```

    **Jupyter Lab**

    ```bash
    jupyter lab
    ```

    这些命令将使用 Jupyter Notebook 或 Jupyter Lab 界面启动浏览器窗口。

1. 在浏览器中，创建新笔记本。

    **Jupyter 笔记本**

    选择“新建”>“.NET (C#)”或“新建”>“.NET (F#)” 

    **Jupyter Lab**

    在启动器窗口中，选择 .NET (C#) 或 .NET (F#) 

### <a name="visual-studio-code-preview"></a>Visual Studio Code（预览）

> [!IMPORTANT]
> 若要在 VS Code 中使用 Jupyter Notebook，必须安装以下各项：
>
>- [VS Code Insiders](https://code.visualstudio.com/insiders/)
>- [.NET 交互式笔记本扩展](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.dotnet-interactive-vscode)

1. 打开 VS Code。
1. 打开命令面板“查看”>“命令面板”。

    出现命令面板时，输入以下命令以创建新的 .NET 交互式笔记本：

    ```text
    >.NET Interactive: Create new blank notebook
    ```

    或者，如果想要使用 ipynb 扩展打开现有的 .NET 交互式笔记本，请使用以下命令：

    ```text
    >.NET Interactive: Open notebook
    ```

## <a name="initialize-a-spark-session"></a>初始化 Spark 会话

1. 打开笔记本时，请安装 `Microsoft.Spark` NuGet 包。 请确保安装的版本与 .NET 工作器版本相同。

    ```text
    #r "nuget:Microsoft.Spark, 0.12.1"
    ```

1. 向笔记本添加以下 using 语句。

    ```csharp
    using Microsoft.Spark.Sql;
    ```

1. 初始化 [SparkSession](xref:Microsoft.Spark.Sql.SparkSession)。

    ```csharp
    var sparkSession =
    SparkSession
        .Builder()
        .AppName("dotnet-interactive-spark")
        .GetOrCreate();
    ```

笔记本应与下图中的类似。 此示例使用 VS Code，但 Jupyter Notebook 和 Jupyter Lab 应大致相同。

> [!div class="mx-imgBorder"]
![.NET for Apache Spark Jupyter Notebook VS Code](media/dotnet-spark-jupyter-notebooks/jupyter-notebooks-dotnet-spark-vscode.png)

## <a name="next-steps"></a>后续步骤

- [.NET for Apache Spark 入门](../tutorials/get-started.md)
- [使用 .NET for Apache Spark 和 ML.NET 预测情绪](../tutorials/ml-sentiment-analysis.md)
- 如需详细了解 .NET Interactive，请参阅 [.NET 交互文档](https://github.com/dotnet/interactive/blob/main/docs/README.md)。
