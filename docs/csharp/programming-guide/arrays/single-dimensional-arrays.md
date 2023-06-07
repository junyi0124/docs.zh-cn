---
title: 一维数组 - C# 编程指南
description: 使用 C# 中的 new 运算符创建一维数组，该运算符指定数组元素类型和元素数目。
ms.date: 06/03/2020
helpviewer_keywords:
- single-dimensional arrays [C#]
- arrays [C#], single-dimensional
ms.assetid: 2cec1196-1de0-49d2-baf2-c607c33310e8
ms.openlocfilehash: ada01262d57cbfebc8bfa1a5fee0639a10db5a4b
ms.sourcegitcommit: cf5a800a33de64d0aad6d115ffcc935f32375164
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/20/2020
ms.locfileid: "86474587"
---
# <a name="single-dimensional-arrays-c-programming-guide"></a>一维数组（C# 编程指南）

使用 [new](../../language-reference/operators/new-operator.md) 运算符创建一维数组，该运算符指定数组元素类型和元素数目。 以下示例声明一个包含五个整数的数组：

:::code language="csharp" source="snippets/SingleDimensionArrays.cs" id="IntDeclaration":::

此数组包含从 `array[0]` 到 `array[4]` 的元素。 数组元素将初始化为元素类型的默认值，`0` 代表整数。

数组可以存储指定的任何元素类型，如声明字符串数组的下例所示：

:::code language="csharp" source="snippets/SingleDimensionArrays.cs" id="StringDeclaration":::

## <a name="array-initialization"></a>数组初始化

可以在声明数组时初始化数组的元素。 不需要长度说明符，因为可以根据初始化列表中的元素数量推断得出。 例如：

:::code language="csharp" source="snippets/SingleDimensionArrays.cs" id="IntInitialization":::

下面的代码显示一个字符串数组的声明，其中每个数组元素都由一天的名称初始化：

:::code language="csharp" source="snippets/SingleDimensionArrays.cs" id="StringInitialization":::
  
在声明时初始化数组时，可以避免使用 `new` 表达式和数组类型，如以下代码所示。 这称为[隐式类型化数组](implicitly-typed-arrays.md)：

:::code language="csharp" source="snippets/SingleDimensionArrays.cs" id="ShorthandInitialization":::

可以在尚未创建数组变量的情况下声明数组变量，但必须使用 `new` 运算符向此变量分配新数组。 例如：

:::code language="csharp" source="snippets/SingleDimensionArrays.cs" id="DeclareAllocate":::

## <a name="value-type-and-reference-type-arrays"></a>值类型和引用类型数组

请考虑以下数组声明：  

:::code language="csharp" source="snippets/SingleDimensionArrays.cs" id="FinalInstantiation":::

此语句的结果取决于 `SomeType` 是值类型还是引用类型。 如果它是值类型，该语句将创建一个 10 个元素的数组，其中每个元素的类型都为 `SomeType`。 如果 `SomeType` 是引用类型，该语句将创建一个 10 个元素的数组，其中每个元素都将被初始化为空引用。 在两个实例中，元素均初始化为元素类型的默认值。 有关值类型和引用类型的详细信息，请参阅[值类型](../../language-reference/builtin-types/value-types.md)和[引用类型](../../language-reference/keywords/reference-types.md)。
  
## <a name="see-also"></a>请参阅

- <xref:System.Array>
- [数组](./index.md)
- [多维数组](./multidimensional-arrays.md)
- [交错数组](./jagged-arrays.md)
