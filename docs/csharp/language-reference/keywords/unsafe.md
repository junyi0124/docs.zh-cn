---
description: unsafe 关键字 - C# 参考
title: unsafe 关键字 - C# 参考
ms.date: 07/20/2015
f1_keywords:
- unsafe_CSharpKeyword
- unsafe
helpviewer_keywords:
- unsafe keyword [C#]
ms.assetid: 7e818009-1c6e-4b9e-b769-3728a01586a0
ms.openlocfilehash: 2e047a4cff77877862c5cbbb5e49eb1a75b42499
ms.sourcegitcommit: d579fb5e4b46745fd0f1f8874c94c6469ce58604
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/30/2020
ms.locfileid: "89141954"
---
# <a name="unsafe-c-reference"></a>unsafe（C# 参考）

`unsafe` 关键字表示不安全上下文，该上下文是任何涉及指针的操作所必需的。 有关详细信息，请参阅[不安全代码和指针](../../programming-guide/unsafe-code-pointers/index.md)。

可在类型或成员的声明中使用 `unsafe` 修饰符。 因此，类型或成员的整个正文范围均被视为不安全上下文。 以下面使用 `unsafe` 修饰符声明的方法为例：

```csharp
unsafe static void FastCopy(byte[] src, byte[] dst, int count)
{
    // Unsafe context: can use pointers here.
}
```

不安全上下文的范围从参数列表扩展到方法的结尾，因此也可在以下参数列表中使用指针：

```csharp
unsafe static void FastCopy ( byte* ps, byte* pd, int count ) {...}
```

还可以使用不安全块从而能够使用该块内的不安全代码。 例如：

```csharp
unsafe
{
    // Unsafe context: can use pointers here.
}
```

要编译不安全代码，必须指定 [`-unsafe`](../compiler-options/unsafe-compiler-option.md) 编译器选项。 不能通过公共语言运行时验证不安全代码。

## <a name="example"></a>示例

[!code-csharp[csrefKeywordsModifiers#22](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csrefKeywordsModifiers/CS/csrefKeywordsModifiers.cs#22)]

## <a name="c-language-specification"></a>C# 语言规范

有关详细信息，请参阅 [C# 语言规范](/dotnet/csharp/language-reference/language-specification/introduction)中的[不安全代码](~/_csharplang/spec/unsafe-code.md)。 该语言规范是 C# 语法和用法的权威资料。

## <a name="see-also"></a>请参阅

- [C# 参考](../index.md)
- [C# 编程指南](../../programming-guide/index.md)
- [C# 关键字](index.md)
- [fixed 语句](fixed-statement.md)
- [不安全代码和指针](../../programming-guide/unsafe-code-pointers/index.md)
- [固定大小的缓冲区](../../programming-guide/unsafe-code-pointers/fixed-size-buffers.md)
