---
title: 使用交叉验证来训练机器学习模型
description: 了解如何使用交叉验证在 ML.NET 中生成更强大的机器学习模型。 交叉验证是一种训练和模型评估技术，可将数据拆分为多个分区，并利用这些分区训练多个算法。
ms.date: 08/29/2019
author: luisquintanilla
ms.author: luquinta
ms.custom: mvc,how-to,title-hack-0625
ms.openlocfilehash: 02cec3d22588d8f10d36216422bc19faafffe94b
ms.sourcegitcommit: aa6d8a90a4f5d8fe0f6e967980b8c98433f05a44
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/16/2020
ms.locfileid: "90679516"
---
# <a name="train-a-machine-learning-model-using-cross-validation"></a>使用交叉验证来训练机器学习模型

了解如何使用交叉验证在 ML.NET 中训练更强大的机器学习模型。

交叉验证是一种训练和模型评估技术，可将数据拆分为多个分区，并利用这些分区训练多个算法。 此技术通过保留来自训练过程的数据来提高模型的可靠性。 除提高不可见观测的性能之外，在数据受限的环境中，它还可用作使用较小数据集训练模型的有效工具。

## <a name="the-data-and-data-model"></a>数据和数据模型

给定来自具有以下格式的文件的数据：

```text
Size (Sq. ft.), HistoricalPrice1 ($), HistoricalPrice2 ($), HistoricalPrice3 ($), Current Price ($)
620.00, 148330.32, 140913.81, 136686.39, 146105.37
550.00, 557033.46, 529181.78, 513306.33, 548677.95
1127.00, 479320.99, 455354.94, 441694.30, 472131.18
1120.00, 47504.98, 45129.73, 43775.84, 46792.41
```

数据可以通过 `HousingData` 等类进行建模并加载到 [`IDataView`](xref:Microsoft.ML.IDataView) 中。

```csharp
public class HousingData
{
    [LoadColumn(0)]
    public float Size { get; set; }

    [LoadColumn(1, 3)]
    [VectorType(3)]
    public float[] HistoricalPrices { get; set; }

    [LoadColumn(4)]
    [ColumnName("Label")]
    public float CurrentPrice { get; set; }
}
```

## <a name="prepare-the-data"></a>准备数据

在使用数据生成机器学习模型之前预处理数据。 在此示例中，`Size` 和 `HistoricalPrices` 列合并为一个特征向量，其使用 [`Concatenate`](xref:Microsoft.ML.TransformExtensionsCatalog.Concatenate%2A) 方法输出到名为 `Features` 的新列。 除了将数据转换为 ML.NET 算法所期望的格式之外，连接列还通过对连接的列应用一次操作（而不是对每个单独的列应用操作），优化了管道中的后续操作。

将列合并为单个向量后，[`NormalizeMinMax`](xref:Microsoft.ML.NormalizationCatalog.NormalizeMinMax%2A) 将应用于 `Features` 列，以使 `Size` 和 `HistoricalPrices` 处于 0-1 之间的同一范围内。

```csharp
// Define data prep estimator
IEstimator<ITransformer> dataPrepEstimator =
    mlContext.Transforms.Concatenate("Features", new string[] { "Size", "HistoricalPrices" })
        .Append(mlContext.Transforms.NormalizeMinMax("Features"));

// Create data prep transformer
ITransformer dataPrepTransformer = dataPrepEstimator.Fit(data);

// Transform data
IDataView transformedData = dataPrepTransformer.Transform(data);
```

## <a name="train-model-with-cross-validation"></a>使用交叉验证训练模型

预处理数据后，即可开始训练模型。 首先，选择最符合要执行的机器学习任务要求的算法。 因为预测值是数字连续值，所以任务是回归任务。 ML.NET 实现的其中一个回归算法是 [`StochasticDualCoordinateAscentCoordinator`](xref:Microsoft.ML.Trainers.SdcaRegressionTrainer) 算法。 若要使用交叉验证来训练模型，请使用 [`CrossValidate`](xref:Microsoft.ML.RegressionCatalog.CrossValidate%2A) 方法。

> [!NOTE]
> 尽管此示例使用线性回归模型，但 CrossValidate 适用于 ML.NET 中除异常情况检测之外的所有其他机器学习任务。

```csharp
// Define StochasticDualCoordinateAscent algorithm estimator
IEstimator<ITransformer> sdcaEstimator = mlContext.Regression.Trainers.Sdca();

// Apply 5-fold cross validation
var cvResults = mlContext.Regression.CrossValidate(transformedData, sdcaEstimator, numberOfFolds: 5);
```

[`CrossValidate`](xref:Microsoft.ML.RegressionCatalog.CrossValidate%2A) 执行以下操作：

1. 将数据分为多个分区，数量为 `numberOfFolds` 参数中指定的值。 每个分区的结果是一个 [`TrainTestData`](xref:Microsoft.ML.DataOperationsCatalog.TrainTestData) 对象。
1. 使用训练数据集上的指定机器学习算法估算器在每个分区上训练模型。
1. 每个模型的性能在测试数据集上使用 [`Evaluate`](xref:Microsoft.ML.RegressionCatalog.Evaluate%2A) 方法进行评估。
1. 为每个模型返回模型及其指标。

`cvResults` 中存储的结果是 [`CrossValidationResult`](xref:Microsoft.ML.TrainCatalogBase.CrossValidationResult%601) 对象的集合。 此对象包括经过训练的模型以及可分别从 [`Model`](xref:Microsoft.ML.TrainCatalogBase.CrossValidationResult%601.Model) 和 [`Metrics`](xref:Microsoft.ML.TrainCatalogBase.CrossValidationResult%601.Metrics) 属性访问的指标。 在此示例中，`Model` 属性为 [`ITransformer`](xref:Microsoft.ML.ITransformer) 类型，`Metrics` 属性为 [`RegressionMetrics`](xref:Microsoft.ML.Data.RegressionMetrics) 类型。

## <a name="evaluate-the-model"></a>评估模型

可以通过单个 [`CrossValidationResult`](xref:Microsoft.ML.TrainCatalogBase.CrossValidationResult%601) 对象的 `Metrics` 属性访问不同的经过训练的模型的指标。 在本例中，通过变量 `rSquared` 访问和存储 [R 平方指标](https://en.wikipedia.org/wiki/Coefficient_of_determination)。

```csharp
IEnumerable<double> rSquared =
    cvResults
        .Select(fold => fold.Metrics.RSquared);
```

如果检查 `rSquared` 变量的内容，则输出应该是五个 0-1 之间的值，其中最接近 1 的值为最佳值。 使用 R 平方等指标按性能最好到性能最差的顺序选择模型。 然后，选择性能最好的模型来进行预测或执行其他操作。

```csharp
// Select all models
ITransformer[] models =
    cvResults
        .OrderByDescending(fold => fold.Metrics.RSquared)
        .Select(fold => fold.Model)
        .ToArray();

// Get Top Model
ITransformer topModel = models[0];
```
