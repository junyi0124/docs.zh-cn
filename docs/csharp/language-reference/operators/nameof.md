---
description: nameof 表达式 - C# 参考
title: nameof 表达式 - C# 参考
ms.date: 04/23/2020
f1_keywords:
- nameof_CSharpKeyword
- nameof
helpviewer_keywords:
- nameof expression [C#]
ms.assetid: 33601bf3-cc2c-4496-846d-f9679bccf2a7
ms.openlocfilehash: 04109cde2a1f9146bf9bb44f301272808797ded0
ms.sourcegitcommit: d579fb5e4b46745fd0f1f8874c94c6469ce58604
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/30/2020
ms.locfileid: "89118307"
---
# <a name="nameof-expression-c-reference"></a>nameof 表达式（C# 参考）

`nameof` 表达式可生成变量、类型或成员的名称作为字符串常量：

[!code-csharp-interactive[nameof expression](snippets/shared/NameOfOperator.cs#Examples)]

如前面的示例所示，对于类型和命名空间，生成的名称通常不是[完全限定的](~/_csharplang/spec/basic-concepts.md#fully-qualified-names)名称。

在[逐字标识符](../tokens/verbatim.md)的情况下，`@` 字符不是名称的一部分，如以下示例所示：

[!code-csharp-interactive[nameof verbatim](snippets/shared/NameOfOperator.cs#Verbatim)]

`nameof` 表达式在编译时进行求值，在运行时无效。

可以使用 `nameof` 表达式使参数检查代码更易于维护：

[!code-csharp[nameof and argument check](snippets/shared/NameOfOperator.cs#ExceptionMessage)]

`nameof` 表达式在 C# 6 及更高版本中提供。

## <a name="c-language-specification"></a>C# 语言规范

有关详细信息，请参阅 [C# 语言规范](~/_csharplang/spec/introduction.md)中的 [Nameof 表达式](~/_csharplang/spec/expressions.md#nameof-expressions)部分。

## <a name="see-also"></a>请参阅

- [C# 参考](../index.md)
- [C# 运算符和表达式](index.md)
