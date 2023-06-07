---
description: override 修饰符 - C# 参考
title: override 修饰符 - C# 参考
ms.date: 10/22/2020
f1_keywords:
- override
- override_CSharpKeyword
helpviewer_keywords:
- override keyword [C#]
ms.assetid: dd1907a8-acf8-46d3-80b9-c2ca4febada8
ms.openlocfilehash: 618200183348e68a4600adb9592a635f61f6a875
ms.sourcegitcommit: 98d20cb038669dca4a195eb39af37d22ea9d008e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/22/2020
ms.locfileid: "92434873"
---
# <a name="override-c-reference"></a>override（C# 参考）

扩展或修改继承的方法、属性、索引器或事件的抽象或虚拟实现需要 `override` 修饰符。

在以下示例中，`Square` 类必须提供 `GetArea` 的重写实现，因为 `GetArea` 继承自抽象 `Shape` 类：

[!code-csharp[csrefKeywordsModifiers#1](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csrefKeywordsModifiers/CS/csrefKeywordsModifiers.cs#1)]

`override` 方法提供从基类继承的方法的新实现。 通过 `override` 声明重写的方法称为重写基方法。 `override` 方法必须具有与重写基方法相同的签名。 从 C# 9.0 开始，`override` 方法支持协变返回类型。 具体而言，`override` 方法的返回类型可从相应基方法的返回类型派生。 在 C# 8.0 和更早版本中，`override` 方法和重写基方法的返回类型必须相同。

不能重写非虚方法或静态方法。 重写基方法必须是 `virtual`、`abstract` 或 `override`。

`override` 声明不能更改 `virtual` 方法的可访问性。 `override` 方法和 `virtual` 方法必须具有相同[级别访问修饰符](access-modifiers.md)。

不能使用 `new`、`static` 或 `virtual` 修饰符修改 `override` 方法。

重写属性声明必须指定与继承的属性完全相同的访问修饰符、类型和名称。 从 C# 9.0 开始，只读重写属性支持协变返回类型。 重写属性必须为 `virtual`、`abstract` 或 `override`。

有关如何使用 `override` 关键字的详细信息，请参阅[使用 Override 和 New 关键字进行版本控制](../../programming-guide/classes-and-structs/versioning-with-the-override-and-new-keywords.md)和[了解何时使用 Override 和 New 关键字](../../programming-guide/classes-and-structs/knowing-when-to-use-override-and-new-keywords.md)。 有关继承的信息，请参阅[继承](../../programming-guide/classes-and-structs/inheritance.md)。

## <a name="example"></a>示例

此示例定义一个名为 `Employee` 的基类和一个名为 `SalesEmployee` 的派生类。 `SalesEmployee` 类包含一个额外字段 `salesbonus`，并且重写方法 `CalculatePay` 以将它考虑在内。

[!code-csharp[csrefKeywordsModifiers#9](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csrefKeywordsModifiers/CS/csrefKeywordsModifiers.cs#9)]

## <a name="c-language-specification"></a>C# 语言规范

有关详细信息，请参阅 [C# 语言规范](~/_csharplang/spec/introduction.md)中的[重写方法](~/_csharplang/spec/classes.md#override-methods)部分。

有关协变返回类型的详细信息，请参阅[功能建议说明](~/_csharplang/proposals/csharp-9.0/covariant-returns.md)。

## <a name="see-also"></a>另请参阅

- [C# 参考](../index.md)
- [继承](../../programming-guide/classes-and-structs/inheritance.md)
- [C# 关键字](index.md)
- [修饰符](index.md)
- [abstract](abstract.md)
- [virtual](virtual.md)
- [new（修饰符）](new-modifier.md)
- [多态性](../../programming-guide/classes-and-structs/polymorphism.md)
