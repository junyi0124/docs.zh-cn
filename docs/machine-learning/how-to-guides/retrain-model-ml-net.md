---
title: 重新训练模型
description: 了解如何在 ML.NET 中重新训练模型
ms.date: 05/03/2019
author: luisquintanilla
ms.author: luquinta
ms.custom: mvc, how-to
ms.openlocfilehash: 50f35e3511acc344339b1e150b47d7ce6de94254
ms.sourcegitcommit: aa6d8a90a4f5d8fe0f6e967980b8c98433f05a44
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/16/2020
ms.locfileid: "90679555"
---
# <a name="re-train-a-model"></a>重新训练模型

了解如何在 ML.NET 中重新训练机器学习模型。

这个世界和它周围的数据在不断变化。 因此，模型也需要更改和更新。 借助 ML.NET 提供的功能，可以将已学习的模型参数作为起点并不断汲取以往经验来重新训练模型，而不必每次都从头开始。

以下算法可在 ML.NET 中重新训练：

- [AveragedPerceptronTrainer](xref:Microsoft.ML.Trainers.AveragedPerceptronTrainer)
- [FieldAwareFactorizationMachineTrainer](xref:Microsoft.ML.Trainers.FieldAwareFactorizationMachineTrainer)
- [LbfgsLogisticRegressionBinaryTrainer](xref:Microsoft.ML.Trainers.LbfgsLogisticRegressionBinaryTrainer)
- [LbfgsMaximumEntropyMulticlassTrainer](xref:Microsoft.ML.Trainers.LbfgsMaximumEntropyMulticlassTrainer)
- [LbfgsPoissonRegressionTrainer](xref:Microsoft.ML.Trainers.LbfgsPoissonRegressionTrainer)
- [LinearSvmTrainer](xref:Microsoft.ML.Trainers.LinearSvmTrainer)
- [OnlineGradientDescentTrainer](xref:Microsoft.ML.Trainers.OnlineGradientDescentTrainer)
- [SgdCalibratedTrainer](xref:Microsoft.ML.Trainers.SgdCalibratedTrainer)
- [SgdNonCalibratedTrainer](xref:Microsoft.ML.Trainers.SgdNonCalibratedTrainer)
- [SymbolicSgdLogisticRegressionBinaryTrainer](xref:Microsoft.ML.Trainers.SymbolicSgdLogisticRegressionBinaryTrainer)

## <a name="load-pre-trained-model"></a>加载预先训练的模型

首先，将预先训练的模型加载到应用程序中。 若要了解有关加载训练管道和模型的详细信息，请参阅[保存和加载已定型模型](save-load-machine-learning-models-ml-net.md)。

```csharp
// Create MLContext
MLContext mlContext = new MLContext();

// Define DataViewSchema of data prep pipeline and trained model
DataViewSchema dataPrepPipelineSchema, modelSchema;

// Load data preparation pipeline
ITransformer dataPrepPipeline = mlContext.Model.Load("data_preparation_pipeline.zip", out dataPrepPipelineSchema);

// Load trained model
ITransformer trainedModel = mlContext.Model.Load("ogd_model.zip", out modelSchema);
```

## <a name="extract-pre-trained-model-parameters"></a>提取预先训练的模型参数

加载模型后，通过访问预先训练模型的 [`Model`](xref:Microsoft.ML.Data.PredictionTransformerBase%601.Model%2A) 属性来提取已学习的模型参数。 使用线性回归模型 [`OnlineGradientDescentTrainer`](xref:Microsoft.ML.Trainers.OnlineGradientDescentTrainer) 训练了预先训练的模型，该线性回归模型可创建输出 [`LinearRegressionModelParameters`](xref:Microsoft.ML.Trainers.LinearRegressionModelParameters) 的 [`RegressionPredictionTransformer`](xref:Microsoft.ML.Data.RegressionPredictionTransformer%601)。 这些线性回归模型参数包含模型已学习的偏差和权重或系数。 这些值将用作新的重新训练模型的起点。

```csharp
// Extract trained model parameters
LinearRegressionModelParameters originalModelParameters =
    ((ISingleFeaturePredictionTransformer<object>)trainedModel).Model as LinearRegressionModelParameters;
```

## <a name="re-train-model"></a>重新训练模型

重新训练模型的过程与训练模型的过程没有什么不同。 唯一的区别是，除了数据之外，[`Fit`](xref:Microsoft.ML.Trainers.OnlineLinearTrainer%602.Fit%2A) 方法还将原始学习模型参数作为输入，并将它们用作重新训练过程的起点。

```csharp
// New Data
HousingData[] housingData = new HousingData[]
{
    new HousingData
    {
        Size = 850f,
        HistoricalPrices = new float[] { 150000f,175000f,210000f },
        CurrentPrice = 205000f
    },
    new HousingData
    {
        Size = 900f,
        HistoricalPrices = new float[] { 155000f, 190000f, 220000f },
        CurrentPrice = 210000f
    },
    new HousingData
    {
        Size = 550f,
        HistoricalPrices = new float[] { 99000f, 98000f, 130000f },
        CurrentPrice = 180000f
    }
};

//Load New Data
IDataView newData = mlContext.Data.LoadFromEnumerable<HousingData>(housingData);

// Preprocess Data
IDataView transformedNewData = dataPrepPipeline.Transform(newData);

// Retrain model
RegressionPredictionTransformer<LinearRegressionModelParameters> retrainedModel =
    mlContext.Regression.Trainers.OnlineGradientDescent()
        .Fit(transformedNewData, originalModelParameters);
```

## <a name="compare-model-parameters"></a>比较模型参数

如何知道是否真的进行了重新训练？ 一种方法是比较重新训练模型的参数是否与原始模型的参数不同。 下面的代码示例将原始模型与重新训练模型的权重进行比较，并将它们输出到控制台。

```csharp
// Extract Model Parameters of re-trained model
LinearRegressionModelParameters retrainedModelParameters = retrainedModel.Model as LinearRegressionModelParameters;

// Inspect Change in Weights
var weightDiffs =
    originalModelParameters.Weights.Zip(
        retrainedModelParameters.Weights, (original, retrained) => original - retrained).ToArray();

Console.WriteLine("Original | Retrained | Difference");
for(int i=0;i < weightDiffs.Count();i++)
{
    Console.WriteLine($"{originalModelParameters.Weights[i]} | {retrainedModelParameters.Weights[i]} | {weightDiffs[i]}");
}
```

下表显示了可能的输出。

|原始 | 重新训练后 | 差值 |
|---|---|---|
| 33039.86 | 56293.76 | -23253.9 |
| 29099.14 | 49586.03 | -20486.89 |
| 28938.38 | 48609.23 | -19670.85 |
| 30484.02 | 53745.43 | -23261.41 |
