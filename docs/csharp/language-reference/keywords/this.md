---
title: this 关键字 - C# 参考
description: this 关键字（C# 参考）
ms.date: 07/20/2015
f1_keywords:
- this
- this_CSharpKeyword
helpviewer_keywords:
- this keyword [C#]
ms.assetid: d4f827fe-4710-410b-89b8-867dad44b8a3
ms.openlocfilehash: 2a2c487ad93e6fc75ecf95c541e859b8b60bb5b5
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/14/2020
ms.locfileid: "75715111"
---
# <a name="this-c-reference"></a>this（C# 参考）

`this` 关键字指代类的当前实例，还可用作扩展方法的第一个参数的修饰符。

> [!NOTE]
> 本文介绍 `this` 在类实例中的用法。 若要深入了解它在扩展方法中的用法，请参阅[扩展方法](../../programming-guide/classes-and-structs/extension-methods.md)。

以下是 `this` 的常见用法：

- 限定类似名称隐藏的成员，例如：

  [!code-csharp[csrefKeywordsAccess#4](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csrefKeywordsAccess/CS/csrefKeywordsAccess.cs#4)]  

- 将对象作为参数传递给方法，例如：

  ```csharp
  CalcTax(this);
  ```

- 声明索引器，例如：

  [!code-csharp[csrefKeywordsAccess#5](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csrefKeywordsAccess/CS/csrefKeywordsAccess.cs#5)]

静态成员函数，因为它们存在于类级别且不属于对象，不具有 `this` 指针。 在静态方法中引用 `this` 会生成错误。

## <a name="example"></a>示例

在此示例中，`this` 用于限定类似名称隐藏的 `Employee` 类成员、`name` 和 `alias`。 它还用于将某个对象传递给属于其他类的方法 `CalcTax`。

[!code-csharp[csrefKeywordsAccess#3](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csrefKeywordsAccess/CS/csrefKeywordsAccess.cs#3)]

## <a name="c-language-specification"></a>C# 语言规范

[!INCLUDE[CSharplangspec](~/includes/csharplangspec-md.md)]

## <a name="see-also"></a>另请参阅

- [C# 参考](../index.md)
- [C# 编程指南](../../programming-guide/index.md)
- [C# 关键字](index.md)
- [base](base.md)
- [方法](../../programming-guide/classes-and-structs/methods.md)
