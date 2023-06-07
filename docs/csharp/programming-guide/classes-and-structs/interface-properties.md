---
title: 接口属性 - C# 编程指南
description: 可在 C# 中的接口上声明属性。 此示例声明一个接口属性访问器。
ms.date: 01/31/2020
helpviewer_keywords:
- properties [C#], on interfaces
- interfaces [C#], properties
ms.assetid: 6503e9ed-33d7-44ec-b4c1-cc16c084b795
ms.openlocfilehash: 920381ae804a6a968bfd667171269377f3d7e448
ms.sourcegitcommit: 3d84eac0818099c9949035feb96bbe0346358504
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/21/2020
ms.locfileid: "86864964"
---
# <a name="interface-properties-c-programming-guide"></a>接口属性（C# 编程指南）

可以在[接口](../../language-reference/keywords/interface.md)上声明属性。 下面的示例声明一个接口属性访问器：

[!code-csharp[DeclareProperties](~/samples/snippets/csharp/interfaces/properties.cs#DeclareInterfaceProperties)]

接口属性通常没有主体。 访问器指示属性是读写、只读还是只写。 与在类和结构中不同，在没有主体的情况下声明访问器不会声明[自动实现的属性](auto-implemented-properties.md)。 从 C# 8.0 开始，接口可为成员（包括属性）定义默认实现。 在接口中为属性定义默认实现的情况非常少，因为接口可能不会定义实例数据字段。

## <a name="example"></a>示例

在此示例中，接口 `IEmployee` 具有读写属性 `Name` 和只读属性 `Counter`。 类 `Employee` 实现 `IEmployee` 接口，并使用这两个属性。 程序读取新员工的姓名以及当前员工数，并显示员工名称和计算的员工数。

可以使用属性的完全限定名称，它引用其中声明成员的接口。 例如：

[!code-csharp[ExplicitProperties](~/samples/snippets/csharp/interfaces/properties.cs#ExplicitImplementation)]

前面的示例演示了如何[显式接口实现](../interfaces/explicit-interface-implementation.md)。 例如，如果 `Employee` 类正在实现接口 `ICitizen` 和接口 `IEmployee`，而这两个接口都具有 `Name` 属性，则需要用到显式接口成员实现。 即是说下列属性声明：

[!code-csharp[ExplicitProperties](~/samples/snippets/csharp/interfaces/properties.cs#ExplicitImplementation)]

在 `IEmployee` 接口中实现 `Name` 属性，而以下声明：

[!code-csharp[ExplicitProperties](~/samples/snippets/csharp/interfaces/properties.cs#CitizenImplementation)]

在 `ICitizen` 接口中实现 `Name` 属性。

[!code-csharp[Example](~/samples/snippets/csharp/interfaces/properties.cs#PropertyExample)]
[!code-csharp[Example](~/samples/snippets/csharp/interfaces/properties.cs#UseProperty)]

**`210 Hazem Abolrous`**

## <a name="sample-output"></a>示例输出

```console
Enter number of employees: 210
Enter the name of the new employee: Hazem Abolrous
The employee information:
Employee number: 211
Employee name: Hazem Abolrous
```

## <a name="see-also"></a>请参阅

- [C# 编程指南](../index.md)
- [属性](./properties.md)
- [使用属性](./using-properties.md)
- [属性与索引器之间的比较](../indexers/comparison-between-properties-and-indexers.md)
- [索引器](../indexers/index.md)
- [接口](../interfaces/index.md)
