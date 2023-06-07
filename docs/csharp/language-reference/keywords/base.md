---
title: base 关键字 - C# 参考
description: 了解 base 关键字，使用它从 C# 中的派生类中访问基类的成员。
ms.date: 07/20/2015
f1_keywords:
- base
- BaseClass.BaseClass
- base_CSharpKeyword
helpviewer_keywords:
- base keyword [C#]
ms.assetid: 8b645dbe-1a33-49b8-8716-1c401f9a5ea5
ms.openlocfilehash: a4686fc5d4245a50de5d77dc0e71c231772f40ef
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/14/2020
ms.locfileid: "75713773"
---
# <a name="base-c-reference"></a>base（C# 参考）

`base` 关键字用于从派生类中访问基类的成员：

- 调用基类上已被其他方法重写的方法。

- 指定创建派生类实例时应调用的基类构造函数。

仅允许基类访问在构造函数、实例方法或实例属性访问器中进行。

从静态方法中使用 `base` 关键字是错误的。

所访问的基类是类声明中指定的基类。 例如，如果指定 `class ClassB : ClassA`，则从 ClassB 访问 ClassA 的成员，而不考虑 ClassA 的基类。

## <a name="example"></a>示例

在本例中，基类 `Person` 和派生类 `Employee` 都有一个名为 `Getinfo` 的方法。 通过使用 `base` 关键字，可以从派生类中调用基类的 `Getinfo` 方法。

[!code-csharp[csrefKeywordsAccess#1](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csrefKeywordsAccess/CS/csrefKeywordsAccess.cs#1)]

有关其他示例，请参阅 [new](new-modifier.md)、[virtual](virtual.md) 和 [override](override.md)。

## <a name="example"></a>示例

本示例显示如何指定在创建派生类实例时调用的基类构造函数。

[!code-csharp[csrefKeywordsAccess#2](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csrefKeywordsAccess/CS/csrefKeywordsAccess.cs#2)]

## <a name="c-language-specification"></a>C# 语言规范

[!INCLUDE[CSharplangspec](~/includes/csharplangspec-md.md)]

## <a name="see-also"></a>另请参阅

- [C# 参考](../index.md)
- [C# 编程指南](../../programming-guide/index.md)
- [C# 关键字](./index.md)
- [this](./this.md)
