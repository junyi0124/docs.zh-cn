---
title: 在 ML.NET 处理期间检查中间数据
description: 学习如何在 ML.NET 中的 ML.NET 机器学习管道加载步骤、处理步骤和模型培训步骤期间检查中间数据。
ms.date: 06/25/2019
author: luisquintanilla
ms.author: luquinta
ms.custom: mvc, how-to, title-hack-0625
ms.openlocfilehash: 4f168653297594a604e6f381947f31cba5376178
ms.sourcegitcommit: aa6d8a90a4f5d8fe0f6e967980b8c98433f05a44
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/16/2020
ms.locfileid: "90679620"
---
# <a name="inspect-intermediate-data-during-processing"></a>在处理期间检查中间数据

学习如何在 ML.NET 中的加载步骤、处理步骤和模型培训步骤期间检查中间数据。 中间数据是机器学习管道中每个阶段的输出。

可以在 ML.NET 中以各种方式检查加载到 [`IDataView`](xref:Microsoft.ML.IDataView) 中的如下所示的中间数据。

```csharp
HousingData[] housingData = new HousingData[]
{
    new HousingData
    {
        Size = 600f,
        HistoricalPrices = new float[] { 100000f ,125000f ,122000f },
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
    },
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
```

## <a name="convert-idataview-to-ienumerable"></a>将 IDataView 转换为 IEnumerable

检查 [`IDataView`](xref:Microsoft.ML.IDataView) 的最快方法之一是将其转换为 [`IEnumerable`](xref:System.Collections.Generic.IEnumerable%601)。 若要将 [`IDataView`](xref:Microsoft.ML.IDataView) 转换为 [`IEnumerable`](xref:System.Collections.Generic.IEnumerable%601)，请使用 [`CreateEnumerable`](xref:Microsoft.ML.DataOperationsCatalog.CreateEnumerable%2A) 方法。

若要优化性能，请将 `reuseRowObject` 设置为 `true`。 如果这样做，将在评估当前行的数据时延迟填充相同的对象，而不是为数据集中的每一行创建一个新对象。

```csharp
// Create an IEnumerable of HousingData objects from IDataView
IEnumerable<HousingData> housingDataEnumerable =
    mlContext.Data.CreateEnumerable<HousingData>(data, reuseRowObject: true);

// Iterate over each row
foreach (HousingData row in housingDataEnumerable)
{
    // Do something (print out Size property) with current Housing Data object being evaluated
    Console.WriteLine(row.Size);
}
```

## <a name="accessing-specific-indices-with-ienumerable"></a>使用 IEnumerable 访问特定索引

如果只需要访问部分数据或特定索引，请使用 [`CreateEnumerable`](xref:Microsoft.ML.DataOperationsCatalog.CreateEnumerable%2A) 并将 `reuseRowObject` 参数值设置为 `false`，以便为数据集中每个请求的行创建一个新对象。 然后，将 [`IEnumerable`](xref:System.Collections.Generic.IEnumerable%601) 转换为数组或列表。

> [!WARNING]
> 将 [`CreateEnumerable`](xref:Microsoft.ML.DataOperationsCatalog.CreateEnumerable%2A) 的结果转换为数组或列表会将所有请求的 [`IDataView`](xref:Microsoft.ML.IDataView) 行加载到内存中，这可能会影响性能。

创建集合后，可以对数据执行操作。 下面的代码片段获取数据集中的前三行并计算当前的平均价格。

```csharp
// Create an Array of HousingData objects from IDataView
HousingData[] housingDataArray =
    mlContext.Data.CreateEnumerable<HousingData>(data, reuseRowObject: false)
        .Take(3)
        .ToArray();

// Calculate Average CurrentPrice of First Three Elements
HousingData firstRow = housingDataArray[0];
HousingData secondRow = housingDataArray[1];
HousingData thirdRow = housingDataArray[2];
float averageCurrentPrice = (firstRow.CurrentPrice + secondRow.CurrentPrice + thirdRow.CurrentPrice) / 3;
```

## <a name="inspect-values-in-a-single-column"></a>检查单个列中的值

在模型生成过程中的任何时候，都可以使用 [`GetColumn`](xref:Microsoft.ML.Data.ColumnCursorExtensions.GetColumn%2A) 方法访问 [`IDataView`](xref:Microsoft.ML.IDataView) 的单个列中的值。 [`GetColumn`](xref:Microsoft.ML.Data.ColumnCursorExtensions.GetColumn%2A) 方法将单个列中的所有值都返回为 [`IEnumerable`](xref:System.Collections.Generic.IEnumerable%601)。

```csharp
IEnumerable<float> sizeColumn = data.GetColumn<float>("Size").ToList();
```

## <a name="inspect-idataview-values-one-row-at-a-time"></a>一次检查一行 IDataView 值

[`IDataView`](xref:Microsoft.ML.IDataView) 延迟求值。 若要循环访问 [`IDataView`](xref:Microsoft.ML.IDataView) 的各行，而不按本文档前面部分所示转换为 [`IEnumerable`](xref:System.Collections.Generic.IEnumerable%601)，请通过使用 [`GetRowCursor`](xref:Microsoft.ML.IDataView.GetRowCursor%2A) 方法并传入 [`IDataView`](xref:Microsoft.ML.IDataView) 的 [DataViewSchema](xref:Microsoft.ML.DataViewSchema) 作为参数来创建 [`DataViewRowCursor`](xref:Microsoft.ML.DataViewRowCursor)。 然后，若要循环访问各行，请使用 [`MoveNext`](xref:Microsoft.ML.DataViewRowCursor.MoveNext%2A) 游标方法以及 [`ValueGetter`](xref:Microsoft.ML.ValueGetter%601) 委托从每个列中提取相应的值。

> [!IMPORTANT]
> 出于性能考虑，ML.NET 中的向量使用 [`VBuffer`](xref:Microsoft.ML.Data.VBuffer%601) 而不是本机集合类型（即 `Vector`,`float[]`）。

```csharp
// Get DataViewSchema of IDataView
DataViewSchema columns = data.Schema;

// Create DataViewCursor
using (DataViewRowCursor cursor = data.GetRowCursor(columns))
{
    // Define variables where extracted values will be stored to
    float size = default;
    VBuffer<float> historicalPrices = default;
    float currentPrice = default;

    // Define delegates for extracting values from columns
    ValueGetter<float> sizeDelegate = cursor.GetGetter<float>(columns[0]);
    ValueGetter<VBuffer<float>> historicalPriceDelegate = cursor.GetGetter<VBuffer<float>>(columns[1]);
    ValueGetter<float> currentPriceDelegate = cursor.GetGetter<float>(columns[2]);

    // Iterate over each row
    while (cursor.MoveNext())
    {
        //Get values from respective columns
        sizeDelegate.Invoke(ref size);
        historicalPriceDelegate.Invoke(ref historicalPrices);
        currentPriceDelegate.Invoke(ref currentPrice);
    }
}
```

## <a name="preview-result-of-pre-processing-or-training-on-a-subset-of-the-data"></a>预览数据子集的预处理或训练结果

> [!WARNING]
> 请勿在生产代码中使用 `Preview`，因为它专用于调试，可能会降低性能。

模型生成过程是实验性的和迭代的。 若要预览对数据子集预处理或训练机器学习模型后的数据，请使用可返回 [`DataDebuggerPreview`](xref:Microsoft.ML.Data.DataDebuggerPreview) 的 [`Preview`](xref:Microsoft.ML.DebuggerExtensions.Preview%2A) 方法。 其结果为一个具有 `ColumnView` 和 `RowView` 属性的对象，这两个属性都是 [`IEnumerable`](xref:System.Collections.Generic.IEnumerable%601) 并包含特定列或行中的值。 使用 `maxRows` 参数指定要应用转换的行数。

![数据调试器预览对象](./media/inspect-intermediate-data-ml-net/data-debugger-preview-01.png)

检查 [`IDataView`](xref:Microsoft.ML.IDataView) 的结果将与下面类似：

![数据调试器预览行视图](./media/inspect-intermediate-data-ml-net/data-debugger-preview-02.png)
