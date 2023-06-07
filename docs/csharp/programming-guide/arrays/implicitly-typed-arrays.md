---
title: 隐式类型数组 - C# 编程指南
description: C# 中隐式类型数组中的类型通过数组初始值设定项中的元素来推断。 在查询表达式中使用隐式类型数组。
ms.date: 07/20/2015
helpviewer_keywords:
- arrays [C#], implicitly-typed
- implicitly-typed arrays [C#]
- C# language, implicitly typed arrays
ms.assetid: e05be95c-6732-403d-ae42-b35f057cbbea
ms.openlocfilehash: 1f14f68207dfb79c92eaa01ac2a8ffaa08facc03
ms.sourcegitcommit: cf5a800a33de64d0aad6d115ffcc935f32375164
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/20/2020
ms.locfileid: "86474704"
---
# <a name="implicitly-typed-arrays-c-programming-guide"></a>隐式类型的数组（C# 编程指南）

可以创建隐式类型化的数组，其中数组实例的类型通过数组初始值设定项中指定的元素来推断。 针对隐式类型化变量的任何规则也适用于隐式类型化数组。 有关详细信息，请参阅[隐式类型局部变量](../classes-and-structs/implicitly-typed-local-variables.md)。

隐式类型化数组通常用于查询表达式、匿名类型、对象和集合初始值设定项。

下列示例演示如何创建隐式类型化数组：

[!code-csharp[csProgGuideLINQ#37](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideLINQ/CS/csRef30LangFeatures_2.cs#37)]

在上个示例中，请注意对于隐式类型化数组，初始化语句的左侧没有使用方括号。 另请注意，和一维数组一样，通过使用 `new []` 来初始化交错数组。

## <a name="implicitly-typed-arrays-in-object-initializers"></a>对象初始值设定项中隐式类型化数组

创建包含数组的匿名类型时，必须在类型的对象初始值设定项中隐式类型化数组。 在下列示例中，`contacts` 是匿名类型的隐式类型化数组，每个类型都包含名为 `PhoneNumbers` 的数组。 请注意，不可在对象初始值设定项中使用 `var` 关键字。

[!code-csharp[csProgGuideLINQ#38](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideLINQ/CS/csRef30LangFeatures_2.cs#38)]

## <a name="see-also"></a>请参阅

- [C# 编程指南](../index.md)
- [隐式类型的局部变量](../classes-and-structs/implicitly-typed-local-variables.md)
- [数组](./index.md)
- [匿名类型](../classes-and-structs/anonymous-types.md)
- [对象和集合初始值设定项](../classes-and-structs/object-and-collection-initializers.md)
- [var](../../language-reference/keywords/var.md)
- [C# 中的 LINQ](../../linq/index.md)
