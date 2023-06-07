---
title: 用排列特征重要性解释 ML.NET 模型
description: 在 ML.NET 中使用排列功能的重要性来理解模型功能的重要性
ms.date: 01/30/2020
author: luisquintanilla
ms.author: luquinta
ms.custom: mvc,how-to
ms.openlocfilehash: ed0d6736f1f2e988d96a397cad77a7fc743489da
ms.sourcegitcommit: cb27c01a8b0b4630148374638aff4e2221f90b22
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/09/2020
ms.locfileid: "86174226"
---
# <a name="interpret-model-predictions-using-permutation-feature-importance"></a>使用排列特征重要性解释模型预测

使用排列特征重要性 (PFI)，了解如何解释 ML.NET 机器学习模型预测。 PFI 可表示每个特征对预测的相对贡献。

机器学习模型通常被视为不透明盒，它们会接收输入并生成输出。 人们对影响输出的中间步骤或特征之间的交互了解甚少。 随着机器学习被引入日常生活的更多方面（例如医疗保健），理解机器学习模型为何做出其决策变得至关重要。 例如，如果诊断由机器学习模型做出，则医疗保健专业人员需要查看影响做出诊断的因素的方法。 提供正确的诊断可以对患者是否快速康复产生重大影响。 因此，模型的可解释性水平越高，医疗保健专业人员就越有信心接受或拒绝模型做出的决策。

有各种技术被用于解释模型，其中之一是 PFI。 PFI 是一种用于解释分类和回归模型的技术，其灵感来自 [Breima 的 Random Forests（随机森林）论文](https://www.stat.berkeley.edu/~breiman/randomforest2001.pdf)（参见第 10 部分）。 概括而言，其工作原理是一次随机为整个数据集随机抽取数据的一个特征，并计算关注性能指标的下降程度。 变化越大，特征就越重要。

此外，通过突出显示最重要的特征，模型生成器可以专注于使用一组更有意义的特征，这可能会减少干扰和训练时间。

## <a name="load-the-data"></a>加载数据

数据集中用于此示例的特征位于列 1-12 中。 目标在于预测 `Price`。

| 列 | 功能 | 描述
| --- | --- | --- |
| 1 | CrimeRate | 人均犯罪率
| 2 | ResidentialZones | 城镇住宅区
| 3 | CommercialZones | 城镇非住宅区
| 4 | NearWater | 距水体的距离
| 5 | ToxicWasteLevels | 毒性程度 (PPM)
| 6 | AverageRoomNumber | 房屋平均房间数量
| 7 | HomeAge | 楼龄
| 8 | BusinessCenterDistance | 与最近商业区的距离
| 9 | HighwayAccess | 距公路的距离
| 10 | TaxRate | 财产税率
| 11 | StudentTeacherRatio | 师生比率
| 12 | PercentPopulationBelowPoverty | 贫困线以下人口百分比
| 13 | Price | 住宅价格

数据集的示例如下所示：

```text
1,24,13,1,0.59,3,96,11,23,608,14,13,32
4,80,18,1,0.37,5,14,7,4,346,19,13,41
2,98,16,1,0.25,10,5,1,8,689,13,36,12
```

此示例中的数据可以通过 `HousingPriceData` 等类进行建模并加载到 [`IDataView`](xref:Microsoft.ML.IDataView) 中。

```csharp
class HousingPriceData
{
    [LoadColumn(0)]
    public float CrimeRate { get; set; }

    [LoadColumn(1)]
    public float ResidentialZones { get; set; }

    [LoadColumn(2)]
    public float CommercialZones { get; set; }

    [LoadColumn(3)]
    public float NearWater { get; set; }

    [LoadColumn(4)]
    public float ToxicWasteLevels { get; set; }

    [LoadColumn(5)]
    public float AverageRoomNumber { get; set; }

    [LoadColumn(6)]
    public float HomeAge { get; set; }

    [LoadColumn(7)]
    public float BusinessCenterDistance { get; set; }

    [LoadColumn(8)]
    public float HighwayAccess { get; set; }

    [LoadColumn(9)]
    public float TaxRate { get; set; }

    [LoadColumn(10)]
    public float StudentTeacherRatio { get; set; }

    [LoadColumn(11)]
    public float PercentPopulationBelowPoverty { get; set; }

    [LoadColumn(12)]
    [ColumnName("Label")]
    public float Price { get; set; }
}
```

## <a name="train-the-model"></a>定型模型

下面的代码示例演示了训练线性回归模型用于预测房屋价格的过程。

```csharp
// 1. Get the column name of input features.
string[] featureColumnNames =
    data.Schema
        .Select(column => column.Name)
        .Where(columnName => columnName != "Label").ToArray();

// 2. Define estimator with data pre-processing steps
IEstimator<ITransformer> dataPrepEstimator =
    mlContext.Transforms.Concatenate("Features", featureColumnNames)
        .Append(mlContext.Transforms.NormalizeMinMax("Features"));

// 3. Create transformer using the data pre-processing estimator
ITransformer dataPrepTransformer = dataPrepEstimator.Fit(data);

// 4. Pre-process the training data
IDataView preprocessedTrainData = dataPrepTransformer.Transform(data);

// 5. Define Stochastic Dual Coordinate Ascent machine learning estimator
var sdcaEstimator = mlContext.Regression.Trainers.Sdca();

// 6. Train machine learning model
var sdcaModel = sdcaEstimator.Fit(preprocessedTrainData);
```

## <a name="explain-the-model-with-permutation-feature-importance-pfi"></a>使用排列特征重要性 (PFI) 解释模型

在 ML.NET 中，为相应的任务使用 [`PermutationFeatureImportance`](xref:Microsoft.ML.PermutationFeatureImportanceExtensions) 方法。

```csharp
ImmutableArray<RegressionMetricsStatistics> permutationFeatureImportance =
    mlContext
        .Regression
        .PermutationFeatureImportance(sdcaModel, preprocessedTrainData, permutationCount:3);
```

在训练数据集上使用 [`PermutationFeatureImportance`](xref:Microsoft.ML.PermutationFeatureImportanceExtensions) 的结果是 [`RegressionMetricsStatistics`](xref:Microsoft.ML.Data.RegressionMetricsStatistics) 对象的 [`ImmutableArray`](xref:System.Collections.Immutable.ImmutableArray)。 [`RegressionMetricsStatistics`](xref:Microsoft.ML.Data.RegressionMetricsStatistics) 提供 [`RegressionMetrics`](xref:Microsoft.ML.Data.RegressionMetrics) 的多个观测值的均值和标准差等摘要统计信息，观测值数量等于 `permutationCount` 参数指定的排列数。

重要性（在本例中，由 [`PermutationFeatureImportance`](xref:Microsoft.ML.PermutationFeatureImportanceExtensions) 计算的 R 平方指标的绝对平均下降）可随后按从最重要到最不重要的顺序排序。

```csharp
// Order features by importance
var featureImportanceMetrics =
    permutationFeatureImportance
        .Select((metric, index) => new { index, metric.RSquared })
        .OrderByDescending(myFeatures => Math.Abs(myFeatures.RSquared.Mean));

Console.WriteLine("Feature\tPFI");

foreach (var feature in featureImportanceMetrics)
{
    Console.WriteLine($"{featureColumnNames[feature.index],-20}|\t{feature.RSquared.Mean:F6}");
}
```

打印 `featureImportanceMetrics` 中每个特征的值将生成类似如下的输出。 请记住，应该预期看到不同的结果，因为这些值根据其获得的数据而有所不同。

| 功能 | R 平方的变化 |
|:--|:--:|
HighwayAccess       |   -0.042731
StudentTeacherRatio |   -0.012730
BusinessCenterDistance| -0.010491
TaxRate             |   -0.008545
AverageRoomNumber   |   -0.003949
CrimeRate           |   -0.003665
CommercialZones     |   0.002749
HomeAge             |   -0.002426
ResidentialZones    |   -0.002319
NearWater           |   0.000203
PercentPopulationLivingBelowPoverty|    0.000031
ToxicWasteLevels    |   -0.000019

查看此数据集最重要的五个特征，此模型预测的房屋价格受其与公路的距离、该区域学校的师生比率、与主要就业中心的距离、资产税率和房屋平均房间数量的影响。
