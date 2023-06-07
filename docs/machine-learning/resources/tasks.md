---
title: 机器学习任务
description: 浏览 ML.NET 中支持的不同机器学习任务和关联的任务。
ms.date: 12/23/2019
ms.openlocfilehash: 56cdb5f3162614d0bf2fb1e5bd9e774b5548b238
ms.sourcegitcommit: aa6d8a90a4f5d8fe0f6e967980b8c98433f05a44
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/16/2020
ms.locfileid: "90679477"
---
# <a name="machine-learning-tasks-in-mlnet"></a>ML.NET 中的机器学习任务

机器学习任务是根据所询问的问题和可用数据进行的预测或推理的类型。 例如，分类任务将数据分配给类别，聚类分析任务根据相似性对数据进行分组。

机器学习任务依赖于数据中的模式，而不是显式编程。

本文介绍了可以从 ML.NET 中选择的不同机器学习任务，以及一些常见用例。

决定适合场景的任务后，则需要选择最佳算法来训练模型。 本节列出了每个任务的可用算法。

## <a name="binary-classification"></a>二元分类

[监管式机器学习](glossary.md#supervised-machine-learning)任务，用于预测数据实例所属的两个类（类别）。 分类算法输入是一组标记示例，其中每个标签为整数 0 或 1。 二元分类算法的输出是一个分类器，可用于预测未标记的新实例的类。 二元分类方案示例包括：

* [了解 Twitter 评论的情绪](../tutorials/sentiment-analysis.md)，“正面”或“负面”。
* 诊断患者是否患有某种疾病。
* 决定是否要将电子邮件标记为“垃圾邮件”。
* 确定照片是否包含特定项，例如狗或水果。

有关详细信息，请参阅 Wikipedia 上的[二元分类](https://en.wikipedia.org/wiki/Binary_classification)一文。

### <a name="binary-classification-trainers"></a>二元分类训练程序

可以使用以下算法训练二元分类模型：

* <xref:Microsoft.ML.Trainers.AveragedPerceptronTrainer>
* <xref:Microsoft.ML.Trainers.SdcaLogisticRegressionBinaryTrainer>
* <xref:Microsoft.ML.Trainers.SdcaNonCalibratedBinaryTrainer>
* <xref:Microsoft.ML.Trainers.SymbolicSgdLogisticRegressionBinaryTrainer>
* <xref:Microsoft.ML.Trainers.LbfgsLogisticRegressionBinaryTrainer>
* <xref:Microsoft.ML.Trainers.LightGbm.LightGbmBinaryTrainer>
* <xref:Microsoft.ML.Trainers.FastTree.FastTreeBinaryTrainer>
* <xref:Microsoft.ML.Trainers.FastTree.FastForestBinaryTrainer>
* <xref:Microsoft.ML.Trainers.FastTree.GamBinaryTrainer>
* <xref:Microsoft.ML.Trainers.FieldAwareFactorizationMachineTrainer>
* <xref:Microsoft.ML.Trainers.PriorTrainer>
* <xref:Microsoft.ML.Trainers.LinearSvmTrainer>

### <a name="binary-classification-inputs-and-outputs"></a>二元分类输入和输出

为了通过二元分类获得最佳结果，应平衡训练数据（即正训练数据和负训练数据的数量相等）。 应在训练前处理缺失值。

输入标签列数据必须为 <xref:System.Boolean>。
输入特征列数据必须为 <xref:System.Single> 的固定大小向量。

这些训练程序将输出以下列：

| 输出列名称 | 列名称 | 描述|
| -- | -- | -- |
| `Score` | <xref:System.Single> | 由模型计算得出的原始分数|
| `PredictedLabel` | <xref:System.Boolean> | 预测的标签，基于分数符号。 负分数映射到 `false`，正分数映射到 `true`。|

## <a name="multiclass-classification"></a>多类分类

[监管式机器学习](glossary.md#supervised-machine-learning)任务，用于预测数据实例的类（类别）。 分类算法输入是一组标记示例。 每个标签通常以文本形式开始。 然后通过 TermTransform 运行，它可将其转换为 Key（数字）类型。 分类算法的输出是一个分类器，可用于预测未标记的新实例的类。 多类分类方案示例包括：

* 确定狗的品种是“西伯利亚哈士奇”、“金毛寻回犬”、“贵宾犬”等。
* 了解电影评论是“正面”、“中立”还是“负面”。
* 将酒店评语分类为“位置”、“价格”、“整洁度”等。

有关详细信息，请参阅 Wikipedia 上的[多类分类](https://en.wikipedia.org/wiki/Multiclass_classification)一文。

>[!NOTE]
>一个与所有升级任何[二元分类学习器](#binary-classification)，以便对多类数据集进行操作。 有关详细信息，请参阅 [Wikipedia](https://en.wikipedia.org/wiki/Multiclass_classification#One-vs.-rest)。

### <a name="multiclass-classification-trainers"></a>多类分类训练程序

可以使用以下训练算法训练多类分类模型：

* <xref:Microsoft.ML.Trainers.LightGbm.LightGbmMulticlassTrainer>
* <xref:Microsoft.ML.Trainers.SdcaMaximumEntropyMulticlassTrainer>
* <xref:Microsoft.ML.Trainers.SdcaNonCalibratedMulticlassTrainer>
* <xref:Microsoft.ML.Trainers.LbfgsMaximumEntropyMulticlassTrainer>
* <xref:Microsoft.ML.Trainers.NaiveBayesMulticlassTrainer>
* <xref:Microsoft.ML.Trainers.OneVersusAllTrainer>
* <xref:Microsoft.ML.Trainers.PairwiseCouplingTrainer>
* <xref:Microsoft.ML.Vision.ImageClassificationTrainer>

### <a name="multiclass-classification-inputs-and-outputs"></a>多类分类输入和输出

输入标签列数据必须为 [key](xref:Microsoft.ML.Data.KeyDataViewType) 类型。
特征列必须为 <xref:System.Single> 的固定大小向量。

该训练程序输出以下列：

| 输出名称 | 类型 | 描述|
| -- | -- | -- |
| `Score` | <xref:System.Single> 的向量 | 所有类的分数。 值越高意味着落入相关类的概率越高。 如果第 i 个元素具有最大值，则预测的标签索引为 i。 请注意，i 是从零开始的索引。 |
| `PredictedLabel` | [key](xref:Microsoft.ML.Data.KeyDataViewType) 类型 | 预测标签的索引。 如果其值为 i，则实际标签为键值输入标签类型中的第 i 个类别。 |

## <a name="regression"></a>回归测试

[监管式机器学习](glossary.md#supervised-machine-learning)任务，用于从一组相关特征中预测标签值。 标签可以是任何实际值，而不是像在分类任务中那样来自一组有限的值。 回归算法模拟其相关特征上的标签依赖关系，以确定标签将如何随着特征值的变化而变化。 回归算法输入是一组带已知值标签的示例。 回归算法输出是一个函数，可用于预测任何一组新输入特征的标签值。 回归方案示例包括：

* 基于房子特性（如卧室数量、位置或大小）来预测房价。
* 基于历史数据和当前市场趋势预测将来的股票价格。
* 基于广告预算预测产品销售。

### <a name="regression-trainers"></a>回归训练程序

可以使用以下算法训练回归模型：

* <xref:Microsoft.ML.Trainers.LbfgsPoissonRegressionTrainer>
* <xref:Microsoft.ML.Trainers.LightGbm.LightGbmRegressionTrainer>
* <xref:Microsoft.ML.Trainers.SdcaRegressionTrainer>
* <xref:Microsoft.ML.Trainers.OlsTrainer>
* <xref:Microsoft.ML.Trainers.OnlineGradientDescentTrainer>
* <xref:Microsoft.ML.Trainers.FastTree.FastTreeRegressionTrainer>
* <xref:Microsoft.ML.Trainers.FastTree.FastTreeTweedieTrainer>
* <xref:Microsoft.ML.Trainers.FastTree.FastForestRegressionTrainer>
* <xref:Microsoft.ML.Trainers.FastTree.GamRegressionTrainer>

### <a name="regression-inputs-and-outputs"></a>回归输入和输出

输入标签列数据必须为 <xref:System.Single>。

此任务的训练程序输出以下列：

| 输出名称 | 类型 | 描述|
| -- | -- | -- |
| `Score` | <xref:System.Single> | 模型预测的原始分数 |

## <a name="clustering"></a>聚类分析

[非监管式机器学习](glossary.md#unsupervised-machine-learning)任务，用于将数据实例分组到包含类似特性的群集。 聚类分析还可用来识别可能无法通过浏览或简单的观察以逻辑方式推导出的数据集中的关系。 聚类分析算法的输入和输出取决于选择的方法。 可以采取分发、质心、连接或基于密度的方法。 ML.NET 当前支持使用 K 平均值聚类分析的基于质心的方法。 聚类分析方案示例包括：

* 基于酒店选择的习惯和特征来了解酒店来宾群。
* 确定客户群和人口统计信息来帮助生成目标广告活动。
* 基于生产指标对清单进行分类。

### <a name="clustering-trainer"></a>聚类分析训练程序

可以使用以下算法训练聚类分析模型：

* <xref:Microsoft.ML.Trainers.KMeansTrainer>

### <a name="clustering-inputs-and-outputs"></a>聚类分析输入和输出

输入特征数据必须为 <xref:System.Single>。 无需标签。

该训练程序输出以下列：

| 输出名称 | 类型 | 描述|
| -- | -- | -- |
| `Score` | <xref:System.Single> 的向量 | 给定数据点到所有群集的质心的距离 |
| `PredictedLabel` | [key](xref:Microsoft.ML.Data.KeyDataViewType) 类型 | 模型预测的最接近的群集的索引。 |

## <a name="anomaly-detection"></a>异常情况检测

此任务使用主体组件分析 (PCA) 创建异常情况检测模型。 基于 PCA 的异常情况检测有助于在以下场景中构建模型：可以很轻松地从一个类中获得定型数据（例如有效事务），但难以获得目标异常的足够示例。

PCA 是机器学习中已建立的一种技术，由于它揭示了数据的内部结构，并解释了数据中的差异，因此经常被用于探索性数据分析。 PCA 的工作方式是通过分析包含多个变量的数据。 它查找变量之间的关联性，并确定最能捕捉结果差异的值的组合。 这些组合的特性值用于创建一个更紧凑的特性空间，称为主体组件。

异常情况检测包含机器学习中的许多重要任务：

* 识别潜在的欺诈交易。
* 指示发生了网络入侵的学习模式。
* 发现异常的患者群集。
* 检查输入系统的值。

根据定义，异常情况属于罕见事件，因此很难收集具有代表性的数据样本用于建模。 此类别中包含的算法是专门设计用来解决使用不平衡数据集建立和定型模型的核心挑战。

### <a name="anomaly-detection-trainer"></a>异常情况检测训练程序

可以使用以下算法训练异常情况检测模型：

* <xref:Microsoft.ML.Trainers.RandomizedPcaTrainer>

### <a name="anomaly-detection-inputs-and-outputs"></a>异常情况检测输入和输出

输入特征必须为 <xref:System.Single> 的固定大小向量。

该训练程序输出以下列：

| 输出名称 | 类型 | 描述|
| -- | -- | -- |
| `Score` | <xref:System.Single> | 由异常情况检测模型计算得出的非负无界分数 |
| `PredictedLabel` | <xref:System.Boolean> | true/false 值表示输入是否异常 (PredictedLabel=true) 或 (PredictedLabel=false) |

## <a name="ranking"></a>排名

排名任务从一组标记的示例构建排名程序。 该示例集由实例组组成，这些实例组可以使用给定的标准进行评分。 每个实例的排名标签是 { 0, 1, 2, 3, 4 }。  排名程序定型为用每个实例的未知分数对新实例组进行排名。 ML.NET 排名学习器基于[机器已学习的排名](https://en.wikipedia.org/wiki/Learning_to_rank)。

### <a name="ranking-training-algorithms"></a>排名训练算法

可以使用以下算法训练排名模型：

* <xref:Microsoft.ML.Trainers.LightGbm.LightGbmRankingTrainer>
* <xref:Microsoft.ML.Trainers.FastTree.FastTreeRankingTrainer>

### <a name="ranking-input-and-outputs"></a>排名输入和输出

输入标签数据类型必须为 [key](xref:Microsoft.ML.Data.KeyDataViewType) 类型或 <xref:System.Single>。 标签的值决定相关性，其中较高的值表示较高的相关性。 如果标签为 [key](xref:Microsoft.ML.Data.KeyDataViewType) 类型，则键索引为相关性值，其中最小索引是最不相关的。 如果标签为 <xref:System.Single>，则较大的值表示较高的相关性。

特征数据必须为 <xref:System.Single> 的固定大小向量，输入行组列必须为 [key](xref:Microsoft.ML.Data.KeyDataViewType) 类型。

该训练程序输出以下列：

| 输出名称 | 类型 | 描述|
| -- | -- | -- |
| `Score` | <xref:System.Single> | 由模型计算以确定预测的无界分数 |

## <a name="recommendation"></a>建议

推荐任务支持生成推荐产品或服务的列表。 ML.NET 使用[矩阵因子分解 (MF)](https://en.wikipedia.org/wiki/Matrix_factorization_%28recommender_systems%29)，这是一种[协作筛选](https://en.wikipedia.org/wiki/Collaborative_filtering)算法，当目录中有历史产品评级数据时，推荐使用该算法。 例如，你为用户提供历史电影评级数据，并希望向他们推荐接下来可能观看的其他电影。

### <a name="recommendation-training-algorithms"></a>建议训练算法

可以使用以下算法训练建议模型：

* <xref:Microsoft.ML.Trainers.MatrixFactorizationTrainer>

## <a name="forecasting"></a>预测

预测任务使用过去的时序数据来预测将来的行为。 适用于预测的场景包括天气预测、季节性销售预测和预测维护。

### <a name="forecasting-trainers"></a>预测训练器

可以使用以下算法训练预测模型：

<xref:Microsoft.ML.TimeSeriesCatalog.ForecastBySsa%2A>
