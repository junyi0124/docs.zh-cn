---
title: 使用 ML.NET CLI 分析情绪
description: 从示例数据集自动生成 ML 模型和相关 C# 代码
author: cesardl
ms.author: cesardl
ms.date: 06/03/2020
ms.custom: mvc,mlnet-tooling
ms.topic: tutorial
ms.openlocfilehash: 89fc5169eee539aa857a9be03c82bf084fe4b60d
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90554431"
---
# <a name="analyze-sentiment-using-the-mlnet-cli"></a>使用 ML.NET CLI 分析情绪

了解如何使用 ML.NET CLI 自动生成 ML.NET 模型和基础 C# 代码。 用户提供想要实现的数据集和机器学习任务，CLI 使用 AutoML 引擎创建模型生成和部署源代码以及分类模型。

在本教程中，将执行以下步骤：
> [!div class="checklist"]
>
> - 为选定的机器学习任务准备数据
> - 从 CLI 运行“mlnet classification”命令
> - 查看质量指标结果
> - 了解生成的 C# 代码，以在应用程序中使用模型
> - 探索生成用于训练模型的 C# 代码

> [!NOTE]
> 本主题涉及目前处于预览状态的 ML.NET CLI 工具，且材料可能会有所变化。 有关详细信息，请访问 [ML.NET](https://dotnet.microsoft.com/apps/machinelearning-ai/ml-dotnet) 页。

ML.NET CLI 是 ML.NET 的一部分，其主要目标是在学习 ML.NET 时为 .NET 开发人员“普及”ML.NET，以便无需从头编写代码即可开始使用。

可以在任何命令提示符（Windows、Mac 或 Linux）上运行 ML.NET CLI，以根据提供的训练数据集生成高质量的 ML.NET 模型和源代码。

## <a name="pre-requisites"></a>先决条件

- [.NET Core 3.1 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1) 或更高版本
- （可选）[Visual Studio 2019](https://visualstudio.microsoft.com/vs/)
- [ML.NET CLI](../how-to-guides/install-ml-net-cli.md)

可以通过 Visual Studio 或使用 `dotnet run` (.NET Core CLI) 运行生成的 C# 代码项目。

## <a name="prepare-your-data"></a>准备数据

我们将使用用于“情绪分析”方案（二元分类机器学习任务）的现有数据集。 用户可以通过类似的方式使用自己的数据集，系统将为用户生成模型和代码。

1. 下载 [UCI Sentiment Labeled Sentences 数据集 zip 文件（参见以下备注中的引文）](http://archive.ics.uci.edu/ml/machine-learning-databases/00331/sentiment%20labelled%20sentences.zip)并在选择的任何文件夹中解压缩文件。

    > [!NOTE]
    > 本教程使用的数据集摘自 KDD 2015 中由 Kotzias 等提出的“From Group to Individual Labels using Deep Features”， 并托管在 UCI 机器学习存储库中（Dua, D. 和 Karra Taniskidou, E.(2017)）。 UCI 机器学习存储库 [http://archive.ics.uci.edu/ml ]。 加利福尼亚州，加利福尼亚大学：欧文分校，信息与计算机科学学院。

2. 将 `yelp_labelled.txt` 文件复制到之前创建的任何文件夹（例如 `/cli-test`）。

3. 打开首选命令提示符并移至复制数据集文件的文件夹。 例如：

    ```console
    cd /cli-test
    ```

    使用任何文本编辑器（例如 Visual Studio Code）都可以打开并浏览 `yelp_labelled.txt` 数据集文件。 可以看到结构：

    - 文件没有标头。 将使用列的索引。

    - 只有两列：

        | 文本（列索引 0） | 标签（列索引 1）|
        |--------------------------|-------|
        | 哇...喜欢这个地方。 | 1 |
        | 酥皮不行。 | 0 |
        | 不够高雅，纹理令人反感。 | 0 |
        | ...更多文本行... | ...（1 或 0）... |

    确保从编辑器中关闭数据集文件。

    现在，已准备好开始将 CLI 用于此“情绪分析”方案。

    > [!NOTE]
    > 完成本教程后，还可以尝试使用自己的数据集，前提是它们可供用于 ML.NET CLI 预览版目前支持的任何 ML 任务，即“二元分类”、“分类”、“回归”和“建议” 。

## <a name="run-the-mlnet-classification-command"></a>运行“mlnet classification”命令

1. 运行以下 ML.NET CLI 命令：

    ```console
    mlnet classification --dataset "yelp_labelled.txt" --label-col 1 --has-header false --train-time 10
    ```

    此命令运行 **`mlnet classification` 命令**：
    - 对于“分类”的“ML 任务”
    - 使用**数据集文件 `yelp_labelled.txt`** 作为训练和测试数据集（CLI 在内部将使用交叉验证或将其拆分为两个数据集，一个用于训练，另一个用于测试）
    - 想要预测的**目的/目标列**（通常称为 **“标签”** ）是**索引为 1 的列**（这是第二列，因为索引从 0 开始）
    - **不使用带有列名的文件标头**，因为此特定数据集文件没有标头
    - 试验的“目标探索/训练时间”为 10 秒 

    将看到 CLI 的输出，类似于：

    ![PowerShell 上的 ML.NET CLI 分类](./media/mlnet-cli/mlnet-classification-powershell.gif)

    在此特殊情况下，只需 10 秒且只需提供小型数据集，CLI 工具就能够运行相当多次迭代，这意味着基于不同的算法/配置组合，以及不同的内部数据转换和算法的超参数进行多次训练。

    最后，10 秒内找到的“最佳质量”模型是使用具有任何特定配置的特定训练程序/算法的模型。 根据探索时间，命令可以生成不同的结果。 选择基于显示的多个指标，例如 `Accuracy`。

    **了解模型的质量指标**

    评估二元分类模型的第一个指标（同时也是最简单的指标）是准确性，该指标易于理解。 “准确性指正确预测与测试数据集的比例。” 越接近 100% (1.00) 越好。

    但是，存在仅使用准确性指标进行测量达不到要求的情况，特别是当测试数据集中的标签（在本例中为 0 和 1）处于非平衡状态时。

    如需其他指标和更多**有关指标的详细信息**（例如准确性、AUC、AUCPR 以及用于评估不同模型的 F1 分数），请参阅[了解 ML.NET 指标](../resources/metrics.md)。

    > [!NOTE]
    > 可以尝试使用完全相同的数据集并为 `--max-exploration-time` 指定数分钟时间（例如三分钟，因此指定 180 秒），这将找到一个更好的“最佳模型”，其具有用于此数据集（非常小，1,000 行）的不同训练管道配置。

    为了找到针对较大数据集的“生产就绪模型”“最佳/优质”模型，应该使用 CLI 进行试验，其通常根据数据集的大小指定更多探索时间。 实际上，在许多情况下，可能需要进行多个小时的探索，特别是在数据集的行数和列数很多的情况下。

1. 之前的命令执行生成了以下资产：

    - 可供使用的序列化模型 .zip 文件（“最佳模型”）。
    - 用于运行生成的模型/对生成的模型评分的 C# 代码（使用该模型在最终用户应用中进行预测）。
    - 用于生成模型的 C# 训练代码（学习目的）。
    - 具有探索过的所有迭代的日志文件，其中包含有关使用其超参数和数据转换组合尝试过的每个算法的特定详细信息。

    前两个资产（.zip 文件模型和用于运行该模型的 C# 代码）可以直接用于最终用户应用（ASP.NET Core Web 应用、服务、桌面应用等），以使用生成的 ML 模型进行预测。

    第三个资产（训练代码）显示 CLI 用于训练生成的模型的 ML.NET API 代码，因此可以研究 CLI 选择的特定训练程序/算法和超参数。

这些枚举资产将在本教程的后续步骤中进行说明。

## <a name="explore-the-generated-c-code-to-use-for-running-the-model-to-make-predictions"></a>浏览生成的 C# 代码，该代码用于运行模型以进行预测

1. 在 Visual Studio（2017 或 2019）中，打开在原始目标文件夹（在教程中命名为 `/cli-test`）内名为 `SampleClassification` 的文件夹中生成的解决方案。 应该看到类似于如下的解决方案：

    > [!NOTE]
    > 在本教程中，我们建议使用 Visual Studio，但也可以使用任何文本编辑器浏览生成的 C# 代码（两个项目），并在 macOS、Linux 或 Windows 计算机上使用 `dotnet CLI` 运行生成的控制台应用。

    ![CLI 生成的 VS 解决方案](./media/mlnet-cli/mlnet-cli-solution-explorer.png)

    - 生成的**类库**（包含序列化 ML 模型（.zip 文件）和数据类（数据模型））可以直接用于最终用户应用程序，甚至可以通过直接引用该类库（或根据需要移动代码）进行使用。
    - 生成的**控制台应用**包含必须查看的执行代码，随后通常会重用“评分代码”（运行 ML 模型以进行预测的代码），方法是将该简单代码（仅几行）移至想要进行预测的最终用户应用程序。

1. 打开类库项目中的 ModelInput.cs  和 ModelOutput.cs  类文件。 可发现这些类是用于保存数据的“数据类”或 POCO 类。 它是“样板代码”，但如果数据集有数十甚至数百列，则生成它很有用。
    - 从数据集中读取数据时使用 `ModelInput` 类。
    - `ModelOutput` 类用于获取预测结果（预测数据）。

1. 打开 Program.cs 文件并浏览代码。 只需几行，就能够运行模型并进行示例预测。

    ```csharp
    static void Main(string[] args)
    {
        // Create single instance of sample data from first line of dataset for model input
        ModelInput sampleData = new ModelInput()
        {
            Col0 = @"Wow... Loved this place.",
        };

        // Make a single prediction on the sample data and print results
        var predictionResult = ConsumeModel.Predict(sampleData);

        Console.WriteLine("Using model to make single prediction -- Comparing actual Col1 with predicted Col1 from sample data...\n\n");
        Console.WriteLine($"Col0: {sampleData.Col0}");
        Console.WriteLine($"\n\nPredicted Col1 value {predictionResult.Prediction} \nPredicted Col1 scores: [{String.Join(",", predictionResult.Score)}]\n\n");
        Console.WriteLine("=============== End of process, hit any key to finish ===============");
        Console.ReadKey();
    }
    ```

    - 第一行代码创建一个样本数据，在本例中基于数据集的第一行用于预测。 还可以通过更新代码来创建自己的硬编码数据：

        ```csharp
        ModelInput sampleData = new ModelInput()
        {
            Col0 = "The ML.NET CLI is great for getting started. Very cool!"
        };
        ```

    - 下一行代码对指定的输入数据使用 `ConsumeModel.Predict()` 方法，以预测并返回结果（基于 ModelOutput.cs 架构）。
    - 最后几行代码输出样本数据的属性（本例中为注释）、情绪预测以及正面情绪 (1) 和负面情绪 (2) 的相应评分。

1. 使用从数据集第一行加载的原始示例数据或通过提供自己的自定义硬编码示例数据来运行项目。 应该获得与以下内容相当的预测：

![ML.NET CLI 从 Visual Studio 运行应用](./media/mlnet-cli/mlnet-cli-console-app.png))

1. 尝试将硬编码示例数据更改为具有不同情绪的其他句子，并查看模型预测正面或负面情绪的表现如何。

## <a name="infuse-your-end-user-applications-with-ml-model-predictions"></a>为最终用户应用程序注入 ML 模型预测

可以使用类似的“ML 模型评分代码”在最终用户应用程序中运行模型并进行预测。

例如，可以直接将该代码移至任何 Windows 桌面应用程序（例如 WPF 和 WinForms），并以与在控制台应用中相同的方式运行模型。

但是，应该优化实现这些代码行来运行 ML 模型的方式（即，缓存模型 .zip 文件并加载一次），并使用单一实例对象，而不是在每个请求上创建它们，特别是在应用程序需要具有可伸缩性（例如 Web 应用程序或分布式服务）的情况下，如以下部分所述。

### <a name="running-mlnet-models-in-scalable-aspnet-core-web-apps-and-services-multi-threaded-apps"></a>在可缩放的 ASP.NET Core Web 应用和服务（多线程应用）中运行 ML.NET 模型

应该优化模型对象（从模型的 .zip 文件加载的 `ITransformer`）和 `PredictionEngine` 对象的创建，尤其是在可缩放的 Web 应用和分布式服务上运行时。 对于第一种情况，模型对象 (`ITransformer`) 的优化很简单。 由于 `ITransformer` 对象为线程安全型，所以可将该对象作为单一实例或静态对象进行缓存，这样就只需加载模型一次。

对于第二个对象（`PredictionEngine` 对象），优化并不容易，因为 `PredictionEngine` 对象不为线程安全型，所以无法在 ASP.NET Core 应用中将此对象实例化为单一实例或静态对象。 此[博客文章](https://devblogs.microsoft.com/cesardelatorre/how-to-optimize-and-run-ml-net-models-on-scalable-asp-net-core-webapis-or-web-apps/)深入讨论了此线程安全和可伸缩性问题。

然而，事情已经变得比博客文章中解释的容易得多。 我们开发出一个更简单的方法，并创建了一个很不错的 **“.NET Core 集成包”** ，用户可通过在应用程序 DI 服务（依赖项注入服务）中注册它来轻松地在 ASP.NET Core 应用和服务中使用它，然后可通过代码直接使用它。 查看以下相关教程和示例：

- [教程：在可缩放的 ASP.NET Core Web 应用和 WebAPI 上运行 ML.NET 模型](../how-to-guides/serve-model-web-api-ml-net.md)
- [示例：ASP.NET Core WebAPI 上的可缩放 ML.NET 模型](https://aka.ms/mlnet-sample-netcoreintegrationpkg)

## <a name="explore-the-generated-c-code-that-was-used-to-train-the-best-quality-model"></a>探索生成用于训练“最佳质量”模型的的 C# 代码

出于更高级的学习目的，还可以探索 CLI 工具生成用于训练生成的模型的 C# 代码。

该“训练模型代码”目前在名为 `ModelBuilder` 的生成的自定义类中生成，因此可以研究该训练代码。

更重要的是，对于此特定方案（情绪分析模型），还可以将该生成的训练代码与以下教程中所述的代码进行比较：

- 比较：[教程：在情绪分析二元分类方案中使用 ML.NET](sentiment-analysis.md)。

将教程中选择的算法和管道配置与 CLI 工具生成的代码进行比较很有趣。 根据迭代和搜索更好的模型所花费的时间，所选算法及其特定的超参数和管道配置可能会有所不同。

在本教程中，你将了解：
> [!div class="checklist"]
>
> - 为选定 ML 任务（要解决的问题）准备数据
> - 在 CLI 工具中运行“mlnet classification”命令
> - 查看质量指标结果
> - 了解生成用于运行模型的 C# 代码（用于最终用户应用的代码）
> - 探索生成的用于训练“最佳质量”模型的 C# 代码（学习目的）

## <a name="see-also"></a>请参阅

- [使用 ML.NET CLI 自动进行模型训练](../automate-training-with-cli.md)
- [教程：在可缩放的 ASP.NET Core Web 应用和 WebAPI 上运行 ML.NET 模型](../how-to-guides/serve-model-web-api-ml-net.md)
- [示例：ASP.NET Core WebAPI 上的可缩放 ML.NET 模型](https://aka.ms/mlnet-sample-netcoreintegrationpkg)
- [ML.NET CLI auto-train 命令参考指南](../reference/ml-net-cli-reference.md)
- [ML.NET CLI 中的遥测](../resources/ml-net-cli-telemetry.md)
