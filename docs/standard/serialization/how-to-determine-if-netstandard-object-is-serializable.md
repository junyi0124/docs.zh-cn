---
title: 如何确定 .NET Standard 对象是否可序列化
description: 演示如何在运行时确定是否可以序列化 .NET Standard 类型。
ms.date: 10/20/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- serializing objects
- objects, serializing steps
ms.openlocfilehash: 9f7ab8a824b9687f68382a5edc342536289c5d09
ms.sourcegitcommit: 74d05613d6c57106f83f82ce8ee71176874ea3f0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/03/2020
ms.locfileid: "93282322"
---
# <a name="how-to-determine-if-a-net-standard-object-is-serializable"></a>如何确定 .NET Standard 对象是否可序列化

.NET Standard 是一种规范，用于定义符合该标准版本的特定 .NET 实现上必须存在的类型和成员。 但 .NET Standard 未定义某个类型是否可序列化。 .NET Standard 库中定义的类型未使用 <xref:System.SerializableAttribute> 属性进行标记。 相反，特定 .NET 实现（如 .NET Framework 和 .NET Core）可随意确定特定类型是否可序列化。

如果你开发了面向 .NET Standard 的库，则支持 .NET Standard 的任何 .NET 实现都可以使用你的库。 这意味着你无法提前知道特定类型是否可序列化；只能在运行时确定它是否可序列化。

可以通过检索表示对象类型的 <xref:System.Type> 对象的 <xref:System.Type.IsSerializable> 属性值，在运行时确定对象是否可序列化。 以下示例提供了一个实现。 它定义一个 `IsSerializable(Object)` 扩展方法，用于指示任何 <xref:System.Object> 实例是否可以序列化。

[!code-csharp[is-a-type-serializable](~/samples/snippets/standard/serialization/is-serializable/csharp/program.cs#2)]
[!code-vb[is-a-type-serializable](~/samples/snippets/standard/serialization/is-serializable/vb/library.vb#2)]

随后可以将任何对象传递给该方法，以确定它是否可以在当前 .NET 实现上进行序列化和反序列化，如以下示例所示：

[!code-csharp[test-is-a-type-serializable](~/samples/snippets/standard/serialization/is-serializable/csharp/program.cs#1)]
[!code-vb[test-is-a-type-serializable](~/samples/snippets/standard/serialization/is-serializable/vb/program.vb#1)]

## <a name="see-also"></a>请参阅

- [二进制序列化](binary-serialization.md)
- <xref:System.SerializableAttribute?displayProperty=nameWithType>
- <xref:System.Type.IsSerializable?displayProperty=nameWithType>
