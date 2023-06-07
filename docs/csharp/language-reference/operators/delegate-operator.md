---
description: delegate 运算符 - C# 参考
title: delegate 运算符 - C# 参考
ms.date: 09/25/2020
helpviewer_keywords:
- delegate [C#]
- anonymous method [C#]
ms.openlocfilehash: db2bf673db12e4a10741a26112820726a4b8aaee
ms.sourcegitcommit: c04535ad05e374fb269fcfc6509217755fbc0d54
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/25/2020
ms.locfileid: "91247652"
---
# <a name="delegate-operator-c-reference"></a>delegate 运算符 -（C# 参考）

`delegate` 运算符创建一个可以转换为委托类型的匿名方法：

[!code-csharp-interactive[anonymous method](snippets/shared/DelegateOperator.cs#AnonymousMethod)]

> [!NOTE]
> 从 C# 3 开始，lambda 表达式提供了一种更简洁和富有表现力的方式来创建匿名函数。 使用 [=> 运算符](lambda-operator.md)构造 lambda 表达式：
>
> [!code-csharp-interactive[lambda expression](snippets/shared/DelegateOperator.cs#Lambda)]
>
> 有关 lambda 表达式功能的更多信息（例如，如何捕获外部变量），请参阅 [lambda 表达式](lambda-expressions.md)。

使用 `delegate` 运算符时，可以省略参数列表。 如果这样做，可以将创建的匿名方法转换为具有任何参数列表的委托类型，如以下示例所示：

[!code-csharp-interactive[no parameter list](snippets/shared/DelegateOperator.cs#WithoutParameterList)]

这是 lambda 表达式不支持的匿名方法的唯一功能。 在所有其他情况下，lambda 表达式是编写内联代码的首选方法。

从 C# 9.0 开始，可以使用[弃元](../../discards.md)指定该方法不使用的两个或更多个匿名方法输入参数：

:::code language="csharp" source="snippets/shared/DelegateOperator.cs" id="SnippetDiscards" :::

为实现向后兼容性，如果只有一个参数名为 `_`，则将 `_` 视为匿名方法中该参数的名称。

从 C# 9.0 开始，可以在匿名方法的声明中使用 `static` 修饰符：

:::code language="csharp" source="snippets/shared/DelegateOperator.cs" id="SnippetStatic" :::

静态匿名方法无法从封闭范围捕获局部变量或实例状态。

还可以使用 `delegate` 关键字声明[委托类型](../builtin-types/reference-types.md#the-delegate-type)。

## <a name="c-language-specification"></a>C# 语言规范

有关详细信息，请参阅 [C# 语言规范](~/_csharplang/spec/introduction.md)中的 [匿名函数表达式](~/_csharplang/spec/expressions.md#anonymous-function-expressions)部分。

## <a name="see-also"></a>另请参阅

- [C# 参考](../index.md)
- [C# 运算符和表达式](index.md)
- [=> 运算符](lambda-operator.md)
