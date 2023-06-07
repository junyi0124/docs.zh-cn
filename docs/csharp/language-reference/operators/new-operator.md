---
description: new 运算符 - C# 参考
title: new 运算符 - C# 参考
ms.date: 10/02/2020
f1_keywords:
- new_CSharpKeyword
helpviewer_keywords:
- new operator keyword [C#]
ms.assetid: a212b697-a79b-4105-9923-1f7b108036e8
ms.openlocfilehash: 3125f3d2c694dcfc5682ee482f3f76072ac3726d
ms.sourcegitcommit: 97405ed212f69b0a32faa66a5d5fae7e76628b68
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/01/2020
ms.locfileid: "91609377"
---
# <a name="new-operator-c-reference"></a>new 运算符（C# 参考）

`new` 运算符创建类型的新实例。

`new` 关键字还可用作[成员声明修饰符](../keywords/new-modifier.md)或[泛型类型约束](../keywords/new-constraint.md)。

## <a name="constructor-invocation"></a>构造函数调用

要创建类型的新实例，通常使用 `new` 运算符调用该类型的某个[构造函数](../../programming-guide/classes-and-structs/constructors.md)：

[!code-csharp-interactive[invoke constructor](snippets/shared/NewOperator.cs#Constructor)]

可以使用带有 `new` 运算符的[对象或集合初始值设定项](../../programming-guide/classes-and-structs/object-and-collection-initializers.md)实例化和初始化一个语句中的对象，如下例所示：

[!code-csharp-interactive[constructor with initializer](snippets/shared/NewOperator.cs#ConstructorWithInitializer)]

从 C# 9.0 开始，构造调用表达式由目标确定类型。 也就是说，如果已知表达式的目标类型，则可以省略类型名称，如下面的示例所示：

:::code language="csharp" source="snippets/shared/NewOperator.cs" id="SnippetTargetTyped":::

如前面的示例所示，在由目标确定类型的 `new` 表达式中始终使用括号。

如果 `new` 表达式的目标类型未知（例如使用 [`var`](../keywords/var.md) 关键字时），必须指定类型名称。

## <a name="array-creation"></a>数组创建

还可以使用 `new` 运算符创建数组实例，如下例所示：

[!code-csharp-interactive[create array](snippets/shared/NewOperator.cs#Array)]

使用数组初始化语法创建数组实例，并在一个语句中使用元素填充该实例。 以下示例显示可以执行该操作的各种方法：

[!code-csharp-interactive[initialize array](snippets/shared/NewOperator.cs#ArrayInitialization)]

有关数组的详细信息，请参阅[数组](../../programming-guide/arrays/index.md)。

## <a name="instantiation-of-anonymous-types"></a>匿名类型的实例化

要创建[匿名类型](../../programming-guide/classes-and-structs/anonymous-types.md)的实例，请使用 `new` 运算符和对象初始值设定项语法：

[!code-csharp-interactive[anonymous type](snippets/shared/NewOperator.cs#AnonymousType)]

## <a name="destruction-of-type-instances"></a>类型实例的析构

无需销毁此前创建的类型实例。 引用和值类型的实例将自动销毁。 包含值类型的上下文销毁后，值类型的实例随之销毁。 在引用类型的最后一次引用被删除后，[垃圾回收器](../../../standard/garbage-collection/index.md)会在某个非指定的时间销毁其实例。

对于包含非托管资源的类型实例（例如，文件句柄），建议采用确定性的清理来确保尽快释放其包含的资源。 有关详细信息，请参阅 <xref:System.IDisposable?displayProperty=nameWithType> API 参考和 [using 语句](../keywords/using-statement.md)一文。

## <a name="operator-overloadability"></a>运算符可重载性

用户定义的类型不能重载 `new` 运算符。

## <a name="c-language-specification"></a>C# 语言规范

有关详细信息，请参阅 [C# 语言规范](~/_csharplang/spec/introduction.md)的 [new 运算符](~/_csharplang/spec/expressions.md#the-new-operator)部分。

有关条件由目标确定类型的 `new` 表达式的详细信息，请参阅[功能建议说明](~/_csharplang/proposals/csharp-9.0/target-typed-new.md)。

## <a name="see-also"></a>另请参阅

- [C# 参考](../index.md)
- [C# 运算符和表达式](index.md)
- [对象和集合初始值设定项](../../programming-guide/classes-and-structs/object-and-collection-initializers.md)
