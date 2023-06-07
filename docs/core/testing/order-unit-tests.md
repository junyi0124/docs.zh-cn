---
title: 对单元测试排序
description: 了解如何通过 .NET Core 对单元测试排序。
author: IEvangelist
ms.author: dapine
ms.date: 05/18/2020
zone_pivot_groups: unit-testing-framework-set-one
ms.openlocfilehash: eb426b790e0623b0cf233a763e93d2bd501b8034
ms.sourcegitcommit: 4ad2f8920251f3744240c3b42a443ffbe0a46577
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/08/2020
ms.locfileid: "86100816"
---
# <a name="order-unit-tests"></a>对单元测试排序

有时，你可能希望按特定顺序运行单元测试。 理想情况下，单元测试的运行顺序__ 不重要，[最佳做法](unit-testing-best-practices.md)是避免对单元测试排序。 但无论如何，可能会有需要这样做。 为此，本文将演示如何对测试运行进行排序。

如果你更喜欢浏览源代码，请参阅[对 .NET Core 单元测试排序](/samples/dotnet/samples/order-unit-tests-cs)示例存储库。

> [!TIP]
> 除了本文中所述的排序功能，还应考虑将[使用 Visual Studio 创建自定义播放列表](/visualstudio/test/run-unit-tests-with-test-explorer?view=vs-2019#create-custom-playlists)作为替代方法。

:::zone pivot="mstest"

## <a name="order-alphabetically"></a>按字母顺序排序

使用 MSTest，测试将按其测试名称自动排序。

> [!NOTE]
> 名为 `Test14` 的测试将在 `Test2` 之前运行，即使数字 `2` 小于 `14` 也是如此。 这是因为，测试名称排序使用的是测试的文本名称。

:::code language="csharp" source="~/dotnet-samples/csharp/unit-testing/MSTest.Project/ByAlphabeticalOrder.cs":::

:::zone-end
:::zone pivot="xunit"

xUnit 测试框架允许对测试运行顺序进行更细致的控制。 可以实现 `ITestCaseOrderer` 和 `ITestCollectionOrderer` 接口，以控制类或测试集合的测试用例的顺序。

## <a name="order-by-test-case-alphabetically"></a>按测试用例的字母顺序排序

若要按其方法名称对测试用例排序，可以实现 `ITestCaseOrderer` 并提供排序机制。

:::code language="csharp" source="~/dotnet-samples/csharp/unit-testing/XUnit.TestProject/Orderers/AlphabeticalOrderer.cs":::

然后，在测试类中，使用 `TestCaseOrdererAttribute` 设置测试用例的顺序。

:::code language="csharp" source="~/dotnet-samples/csharp/unit-testing/XUnit.TestProject/ByAlphabeticalOrder.cs":::

## <a name="order-by-collection-alphabetically"></a>按集合的字母顺序排序

若要按其显示名称对测试集合排序，可以实现 `ITestCollectionOrderer` 并提供排序机制。

:::code language="csharp" source="~/dotnet-samples/csharp/unit-testing/XUnit.TestProject/Orderers/DisplayNameOrderer.cs":::

由于测试集合可能会并行运行，因此必须使用 `CollectionBehaviorAttribute` 显式禁用集合的测试并行化。 然后，将实现指定到 `TestCollectionOrdererAttribute`。

:::code language="csharp" source="~/dotnet-samples/csharp/unit-testing/XUnit.TestProject/ByDisplayName.cs":::

## <a name="order-by-custom-attribute"></a>按自定义属性排序

若要使用自定义属性对 xUnit 测试排序，首先需要一个要依赖的属性。 按如下所示定义 `TestPriorityAttribute`：

:::code language="csharp" source="~/dotnet-samples/csharp/unit-testing/XUnit.TestProject/Attributes/TestPriorityAttribute.cs":::

接下来，请考虑 `ITestCaseOrderer` 接口的以下 `PriorityOrderer` 实现。

:::code language="csharp" source="~/dotnet-samples/csharp/unit-testing/XUnit.TestProject/Orderers/PriorityOrderer.cs":::

然后，在测试类中，使用 `TestCaseOrdererAttribute` 将测试用例的顺序设置为 `PriorityOrderer`。

:::code language="csharp" source="~/dotnet-samples/csharp/unit-testing/XUnit.TestProject/ByPriorityOrder.cs":::

:::zone-end
:::zone pivot="nunit"

## <a name="order-by-priority"></a>按优先级排序

为了显式对测试排序，NUnit 提供了 [`OrderAttribute`](https://github.com/nunit/docs/wiki/Order-Attribute)。 具有此属性的测试先于没有此属性的测试启动。 顺序值用于确定运行单元测试的顺序。

:::code language="csharp" source="~/dotnet-samples/csharp/unit-testing/NUnit.TestProject/ByOrder.cs":::

:::zone-end

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [单元测试代码覆盖率](unit-testing-code-coverage.md)
