---
title: ML.NET CLI 命令参考
description: ML.NET CLI 工具中 auto-train 命令的概述、示例和参考。
ms.date: 06/03/2020
ms.custom: mlnet-tooling
ms.openlocfilehash: 6f07cd8b4237f8931bbc0ec97bc0bbe18c488f16
ms.sourcegitcommit: e395fabeeea5c705d243d246fa64446839ac85b6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/03/2021
ms.locfileid: "97856063"
---
# <a name="the-mlnet-cli-command-reference"></a>ML.NET CLI 命令参考

`classification`、`regression` 和 `recommendation` 命令是 ML.NET CLI 工具提供的主要命令。 这些命令允许使用自动化机器学习 (AutoML) 以及用于运行模型/对模型评分的示例 C# 代码生成用于分类、回归和建议模型的高质量 ML.NET 模型。 此外，还会生成用于训练模型的 C# 代码，以便你可以研究模型的算法和设置。

> [!NOTE]
> 本主题涉及目前处于预览状态的 ML.NET CLI 和 ML.NET AutoML，且材料可能会有所变化。

## <a name="overview"></a>概述

示例用法：

```console
mlnet regression --dataset "cars.csv" --label-col price
```

`mlnet` ML 任务命令（`classification`、`regression` 和 `recommendation`）生成以下资产：

- 可供使用的序列化模型 .zip 文件（“最佳模型”）。
- 用于运行生成的模型和对其进行评分的 C# 代码。
- 包含用于生成该模型的训练代码的 C# 代码。

前两个资产可以直接用于最终用户应用（ASP.NET Core Web 应用、服务、桌面应用等），以使用模型进行预测。

第三个资产（训练代码）显示 CLI 用于训练生成的模型的 ML.NET API 代码，因此可以研究模型的特定算法和设置。

## <a name="examples"></a>示例

用于分类问题的最简单的 CLI 命令（AutoML 从提供的数据中推断出大部分配置）：

```console
mlnet classification --dataset "customer-feedback.tsv" --label-col Sentiment
```

另一个用于回归问题的简单 CLI 命令：

``` console
mlnet regression --dataset "cars.csv" --label-col Price
```

使用训练数据集、测试数据集和进一步的自定义显式参数创建和训练分类模型：

```console
mlnet classification --dataset "/MyDataSets/Population-Training.csv" --test-dataset "/MyDataSets/Population-Test.csv" --label-col "InsuranceRisk" --cache on --train-time 600
```

## <a name="command-options"></a>命令选项

`mlnet` ML 任务命令（`classification`、`regression` 和 `recommendation`）基于提供的数据集和 ML.NET CLI 选项训练多个模型。 这些命令还可用于选择最佳模型，将模型保存为序列化的 .zip 文件，并生成用于评分和训练的相关 C# 代码。

### <a name="classification-options"></a>分类选项

运行 `mlnet classification` 将训练分类模型。 如果希望 ML 模型将数据分类为 2 个或更多个类（例如情绪分析），请选择此命令。

```console
mlnet classification

--dataset <path> (REQUIRED)

--label-col <col> (REQUIRED)

--cache <option>

--has-header (Default: true)

--ignore-cols <cols>

--log-file-path <path>

--name <name>

-o, --output <path>

--test-dataset <path>

--train-time <time> (Default: 30 minutes, in seconds)

--validation-dataset <path>

-v, --verbosity <v>

-?, -h, --help

```

### <a name="regression-options"></a>回归选项

运行 `mlnet regression` 将训练回归模型。 如果希望 ML 模型预测数值（例如价格预测），请选择此命令。

```console
mlnet regression

--dataset <path> (REQUIRED)

--label-col <col> (REQUIRED)

--cache <option>

--has-header (Default: true)

--ignore-cols <cols>

--log-file-path <path>

--name <name>

-o, --output <path>

--test-dataset <path>

--train-time <time> (Default: 30 minutes, in seconds)

--validation-dataset <path>

-v, --verbosity <v>

-?, -h, --help

```

### <a name="recommendation-options"></a>建议选项

运行 `mlnet recommendation` 将训练建议模型。  如果希望 ML 模型根据评级（例如产品建议）向用户推荐项，请选择此命令。

```console
mlnet recommendation

--dataset <path> (REQUIRED)

--item-col <col> (REQUIRED)

--rating-col <col> (REQUIRED)

--user-col <col> (REQUIRED)

--cache <option>

--has-header (Default: true)

--log-file-path <path>

--name <name>

-o, --output <path>

--test-dataset <path>

--train-time <time> (Default: 30 minutes, in seconds)

--validation-dataset <path>

-v, --verbosity <v>

-?, -h, --help

```

无效输入选项会导致 CLI 工具发出有效输入和错误消息列表。

## <a name="dataset"></a>数据集

`--dataset | -d`（字符串）

此参数向以下任一选项提供文件路径：

- *A：整个数据集文件：* 如果使用此选项且用户未提供 `--test-dataset` 和 `--validation-dataset`，则将在内部使用交叉验证（K 折等）或自动数据拆分方法来验证模型。 在这种情况下，用户将只需要提供数据集文件路径。

- *B：训练数据集文件：* 如果用户还提供用于模型验证的数据集（使用 `--test-dataset` 和可选的 `--validation-dataset`），则 `--dataset` 参数意味着仅拥有“训练数据集”。 例如，当使用 80%-20% 方法来验证模型的质量以及获取准确性指标时，“训练数据集”将拥有 80% 的数据，而“测试数据集”将拥有 20% 的数据。

## <a name="test-dataset"></a>测试数据集

`--test-dataset | -t`（字符串）

指向测试数据集文件的文件路径，例如，在进行常规验证以获取准确性指标时使用 80%-20% 方法的情况。

如果使用 `--test-dataset`，则还需要 `--dataset`。

除非使用 --validation-dataset，否则 `--test-dataset` 参数是可选的。 在这种情况下，用户必须使用三个参数。

## <a name="validation-dataset"></a>验证数据集

`--validation-dataset | -v`（字符串）

指向验证数据集文件的文件路径。 验证数据集在任何情况下都是可选的。

如果使用 `validation dataset`，则行为应为：

- `test-dataset` 和 `--dataset` 参数也是必需的。

- `validation-dataset` 数据集用于估算模型选择的预测误差。

- `test-dataset` 用于评估最终选定模型的泛化误差。 理想情况下，测试集应保存在“保管库”中，并仅在数据分析结束时显示。

一般来说，使用 `validation dataset` 以及 `test dataset` 时，验证阶段拆分为两部分：

1. 在第一部分中，只需查看模型并使用验证数据选择性能最佳的方法即可（= 验证）
2. 然后，估算选定方法的准确性（= 测试）。

因此，数据分离可以是 80/10/10 或 75/15/10。 例如：

- `training-dataset` 文件应拥有 75% 的数据。
- `validation-dataset` 文件应拥有 15% 的数据。
- `test-dataset` 文件应拥有 10% 的数据。

在任何情况下，这些百分比都将由使用 CLI 的用户决定，用户将提供已经拆分的文件。

## <a name="label-column"></a>标签列

`--label-col`（int 或 string）

借助此参数，可以使用数据集标头中设置的列名或数据集文件中列的数字索引（列索引值从 0 开始）来指定特定的目的/目标列（想要预测的变量）。

此参数用于分类和回归问题 。

## <a name="item-column"></a>项列

`--item-col`（int 或 string）

项列包含用户评分的项目列表（向用户建议的项）。 可以使用数据集标头中设置的列名或数据集文件中列的数字索引（列索引值从 0 开始）来指定此列。

此参数仅用于建议任务。

## <a name="rating-column"></a>评级列

`--rating-col`（int 或 string）

评级列包含用户给项的评级列表。 可以使用数据集标头中设置的列名或数据集文件中列的数字索引（列索引值从 0 开始）来指定此列。

此参数仅用于建议任务。

## <a name="user-column"></a>用户列

`--user-col`（int 或 string）

用户列包含为项评级的用户列表。 可以使用数据集标头中设置的列名或数据集文件中列的数字索引（列索引值从 0 开始）来指定此列。

此参数仅用于建议任务。

## <a name="ignore-columns"></a>忽略列

`--ignore-columns`（字符串）

借助此参数，可以忽略数据集文件中的现有列，这样一来，训练过程便不会加载和使用它们。

指定想要忽略的列名。 使用“, ”（逗号加空格）或“ ”（空格）分隔多个列名。 可以对包含空格的列名使用引号（例如，“logged in”）。

示例：

`--ignore-columns email, address, id, logged_in`

## <a name="has-header"></a>具有标头

`--has-header`（布尔值）

指定数据集文件是否拥有标头行。
可能的值有：

- `true`
- `false`

如果用户未指定此参数，则 ML.NET CLI 将尝试检测此属性。

## <a name="train-time"></a>训练时间

`--train-time`（字符串）

默认情况下，最长探索/训练时间为 30 分钟。

此参数设置探索多个训练程序和配置的过程的最长持续时间（以秒为单位）。 如果提供的时间对于单次迭代来说太短（例如 2 秒），则可能会超出配置的时间。 在这种情况下，实际时间是在单次迭代中生成一个模型配置所需的时间。

迭代所需的时间可能因数据集的大小而异。

## <a name="cache"></a>缓存

`--cache`（字符串）

如果使用缓存，则整个训练数据集将在内存中加载。

对于中小型数据集，使用缓存可以大幅提高训练性能，这意味着训练时间可以比不使用缓存时更短。

但是，对于大型数据集，在内存中加载所有数据可能会产生负面影响，因为可能会出现内存不足的情况。 如果使用大型数据集文件进行训练而不使用缓存，ML.NET 将在训练期间需要加载更多数据时从驱动器中流式传输数据块。

你可以指定以下值：

`on`：强制在训练时使用缓存。
`off`：强制在训练时不使用缓存。
`auto`：是否使用缓存取决于 AutoML 试探法。 通常情况下，如果使用 `auto` 选项，小型/中型数据集将使用缓存，而大型数据集将不使用缓存。

如果未指定 `--cache` 参数，则默认情况下将使用缓存 `auto` 配置。

## <a name="name"></a>“属性”

`--name`（字符串）

创建的输出项目或解决方案的名称。 如果未指定名称，则使用名称 `sample-{mltask}`。

ML.NET 模型文件（.zip 文件）也将获得相同的名称。

## <a name="output-path"></a>输出路径

`--output | -o`（字符串）

用于放置生成的输出的根位置/文件夹。 默认为当前目录。

## <a name="verbosity"></a>详细级别

`--verbosity | -v`（字符串）

设置标准输出的详细级别。

允许的值包括：

- `q[uiet]`
- `m[inimal]`（默认情况）
- `diag[nostic]`（日志记录信息级别）

默认情况下，CLI 工具应在运行时显示一些最低限度的反馈 (`minimal`)，例如提及它正在运行以及剩余多少时间或已完成时间的百分比（如果可能）。

## <a name="help"></a>帮助

`-h |--help`

打印命令帮助，其中包含对每个命令的参数的描述。

## <a name="see-also"></a>请参阅

- [如何安装 ML.NET CLI 工具](../how-to-guides/install-ml-net-cli.md)
- [ML.NET CLI 概述](../automate-training-with-cli.md)
- [教程：使用 ML.NET CLI 分析情绪](../tutorials/sentiment-analysis-cli.md)
- [ML.NET CLI 中的遥测](../resources/ml-net-cli-telemetry.md)
