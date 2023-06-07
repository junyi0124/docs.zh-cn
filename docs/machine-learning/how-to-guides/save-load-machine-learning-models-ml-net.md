---
title: 保存和加载经过训练的模型
description: 了解如何保存和加载经过训练的模型
ms.date: 05/03/2019
author: luisquintanilla
ms.author: luquinta
ms.custom: mvc, how-to
ms.openlocfilehash: 681a35956a8959e2f1cbb5a7023e0ef29b67097e
ms.sourcegitcommit: aa6d8a90a4f5d8fe0f6e967980b8c98433f05a44
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/16/2020
ms.locfileid: "90679529"
---
# <a name="save-and-load-trained-models"></a>保存和加载经过训练的模型

了解如何在应用程序中保存和加载经过训练的模型。

在整个模型生成过程中，模型位于内存中，并且可以在整个应用程序生命周期中访问。 但是，一旦应用程序停止运行，而模型未在本地或远程的某个位置保存，则无法再访问该模型。 通常情况下，在其他应用程序中训练模型之后，某些时候会使用模型进行推理或重新训练。 因此，存储模型很重要。 使用类似下文详述的数据准备和模型训练管道时，请使用本文档后续部分中介绍的步骤保存和加载模型。 虽然此示例使用线性回归模型，但相同的过程适用于其他 ML.NET 算法。

```csharp
HousingData[] housingData = new HousingData[]
{
    new HousingData
    {
        Size = 600f,
        HistoricalPrices = new float[] { 100000f, 125000f, 122000f },
        CurrentPrice = 170000f
    },
    new HousingData
    {
        Size = 1000f,
        HistoricalPrices = new float[] { 200000f, 250000f, 230000f },
        CurrentPrice = 225000f
    },
    new HousingData
    {
        Size = 1000f,
        HistoricalPrices = new float[] { 126000f, 130000f, 200000f },
        CurrentPrice = 195000f
    }
};

// Create MLContext
MLContext mlContext = new MLContext();

// Load Data
IDataView data = mlContext.Data.LoadFromEnumerable<HousingData>(housingData);

// Define data preparation estimator
EstimatorChain<RegressionPredictionTransformer<LinearRegressionModelParameters>> pipelineEstimator =
    mlContext.Transforms.Concatenate("Features", new string[] { "Size", "HistoricalPrices" })
        .Append(mlContext.Transforms.NormalizeMinMax("Features"))
        .Append(mlContext.Regression.Trainers.Sdca());

// Train model
ITransformer trainedModel = pipelineEstimator.Fit(data);

// Save model
mlContext.Model.Save(trainedModel, data.Schema, "model.zip");
```

由于大部分模型和数据准备管道都继承自同一组类，这些组件的保存和加载方法签名相同。 根据用例，可以将数据准备管道和模型合并为单个 [`EstimatorChain`](xref:Microsoft.ML.Data.TransformerChain%601)（输出单个 [`ITransformer`](xref:Microsoft.ML.ITransformer)），也可将它们分隔，从而为其各自创建单独的 [`ITransformer`](xref:Microsoft.ML.ITransformer)。

## <a name="save-a-model-locally"></a>在本地保存模型

保存模型时，需要以下两项：

1. 模型的 [`ITransformer`](xref:Microsoft.ML.ITransformer)。
2. [`DataViewSchema`](xref:Microsoft.ML.DataViewSchema) 预期输入的 [`ITransformer`](xref:Microsoft.ML.ITransformer)。

训练模型后，通过 [`Save`](xref:Microsoft.ML.ModelOperationsCatalog.Save%2A) 方法使用输入数据的 `DataViewSchema` 将经过训练的模型保存到名为 `model.zip` 的文件中。

```csharp
// Save Trained Model
mlContext.Model.Save(trainedModel, data.Schema, "model.zip");
```

## <a name="load-a-model-stored-locally"></a>加载本地存储的模型

本地存储的模型可用于其他进程或应用程序，如 `ASP.NET Core` 和 `Serverless Web Applications`。 若要了解详细信息，请参阅[在 Web API 中使用 ML.NET](./serve-model-web-api-ml-net.md) 和[部署 ML.NET 无服务器 Web 应用](./serve-model-serverless-azure-functions-ml-net.md)操作说明文章。

在单独的应用程序或进程中，配合使用 [`Load`](xref:Microsoft.ML.ModelOperationsCatalog.Load%2A) 方法和文件路径将经过训练的模型载入应用程序。

```csharp
//Define DataViewSchema for data preparation pipeline and trained model
DataViewSchema modelSchema;

// Load trained model
ITransformer trainedModel = mlContext.Model.Load("model.zip", out modelSchema);
```

## <a name="load-a-model-stored-remotely"></a>加载远程存储的模型

若要将存储在远程位置的数据准备管道和模型加载到应用程序中，请使用 [`Stream`](xref:System.IO.Stream)，而不要使用 [`Load`](xref:Microsoft.ML.ModelOperationsCatalog.Load%2A) 方法中的文件路径。

```csharp
// Create MLContext
MLContext mlContext = new MLContext();

// Define DataViewSchema and ITransformers
DataViewSchema modelSchema;
ITransformer trainedModel;

// Load data prep pipeline and trained model
using (HttpClient client = new HttpClient())
{
    Stream modelFile = await client.GetStreamAsync("<YOUR-REMOTE-FILE-LOCATION>");

    trainedModel = mlContext.Model.Load(modelFile, out modelSchema);
}
```

## <a name="working-with-separate-data-preparation-and-model-pipelines"></a>使用单独的数据准备和模型管道

> [!NOTE]
> 使用单独的数据准备和模型训练管道是可选方案。 管道的分离使得用户能够更轻松地检查已学习的模型参数。 对于预测，保存和加载包含数据准备和模型训练操作的单个管道更轻松。

使用单独的数据准备管道和模型时，流程与单个管道相同；但现在需要同时保存和加载两个管道。

给定单独的数据准备和模型训练管道：

```csharp
// Define data preparation estimator
IEstimator<ITransformer> dataPrepEstimator =
    mlContext.Transforms.Concatenate("Features", new string[] { "Size", "HistoricalPrices" })
        .Append(mlContext.Transforms.NormalizeMinMax("Features"));

// Create data preparation transformer
ITransformer dataPrepTransformer = dataPrepEstimator.Fit(data);

// Define StochasticDualCoordinateAscent regression algorithm estimator
var sdcaEstimator = mlContext.Regression.Trainers.Sdca();

// Pre-process data using data prep operations
IDataView transformedData = dataPrepTransformer.Transform(data);

// Train regression model
RegressionPredictionTransformer<LinearRegressionModelParameters> trainedModel = sdcaEstimator.Fit(transformedData);
```

### <a name="save-data-preparation-pipeline-and-trained-model"></a>保存数据准备管道和经过训练的模型

若要保存数据准备管道和经过训练的模型，请使用以下命令：

```csharp
// Save Data Prep transformer
mlContext.Model.Save(dataPrepTransformer, data.Schema, "data_preparation_pipeline.zip");

// Save Trained Model
mlContext.Model.Save(trainedModel, transformedData.Schema, "model.zip");
```

### <a name="load-data-preparation-pipeline-and-trained-model"></a>加载数据准备管道和经过训练的模型

在单独的进程或应用程序中，同时加载数据准备管道和经过训练的模型，如下所示：

```csharp
// Create MLContext
MLContext mlContext = new MLContext();

// Define data preparation and trained model schemas
DataViewSchema dataPrepPipelineSchema, modelSchema;

// Load data preparation pipeline and trained model
ITransformer dataPrepPipeline = mlContext.Model.Load("data_preparation_pipeline.zip",out dataPrepPipelineSchema);
ITransformer trainedModel = mlContext.Model.Load("model.zip", out modelSchema);
```
