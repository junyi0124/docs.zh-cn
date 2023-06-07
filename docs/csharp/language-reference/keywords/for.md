---
description: for 语句 - C# 参考
title: for 语句 - C# 参考
ms.date: 06/13/2018
f1_keywords:
- for
- for_CSharpKeyword
helpviewer_keywords:
- for keyword [C#]
ms.assetid: 34041a40-2c87-467a-9ffb-a0417d8f67a8
ms.openlocfilehash: be9ecdc08d54c9cde1c49656a16e0d85a6d7084d
ms.sourcegitcommit: d579fb5e4b46745fd0f1f8874c94c6469ce58604
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/30/2020
ms.locfileid: "89126926"
---
# <a name="for-c-reference"></a>for（C# 参考）

在指定的布尔表达式的计算结果为 `true` 时，`for` 语句会执行一条语句或一个语句块。

在 `for` 语句块中的任何点上，可以使用 [break](break.md) 语句中断循环，或者可以使用 [continue](continue.md) 语句继续执行到循环中的下一次迭代。 还可以使用 [goto](goto.md)、[return](return.md) 或 [throw](throw.md) 语句退出 `for` 循环。

## <a name="structure-of-the-for-statement"></a>`for` 语句的结构

`for` 语句定义初始化表达式、条件和迭代器部分    ：

```csharp
for (initializer; condition; iterator)
    body
```

三个部分都是可选的。 循环体是一个语句或一个语句块。

以下示例显示定义了所有部分的 `for`：

[!code-csharp-interactive[for loop example](snippets/IterationKeywordsExamples.cs#5)]

### <a name="the-initializer-section"></a>“初始化表达式”部分 

“初始化表达式”部分的语句仅在进入循环前执行一次  。 “初始化表达式”部分是下列内容之一  ：

- 本地循环变量的声明和初始化，不能从循环外访问。

- 以下列表中显示用逗号分隔的零个或多个语句表达式：

  - [赋值](../operators/assignment-operator.md)语句

  - 方法的调用

  - 为 [increment](../operators/arithmetic-operators.md#increment-operator-) 表达式添加前缀或后缀，如 `++i` 或 `i++`

  - 为 [decrement](../operators/arithmetic-operators.md#decrement-operator---) 表达式添加前缀或后缀，如 `--i` 或 `i--`

  - 通过使用 [new](../operators/new-operator.md) 运算符来创建对象

  - [await](../operators/await.md) 表达式

上例中的“初始化表达式”部分声明和初始化本地循环变量 `i` ：

```csharp
int i = 0
```

### <a name="the-condition-section"></a>“条件”部分 

“条件”部分（如果存在）必须为布尔表达式  。 在每次循环迭代前计算该表达式。 如果“条件”部分不存在或者布尔表达式的计算结果为 `true`，则执行下一个循环迭代；否则退出循环  。

上例中的“条件”部分确定循环是否根据本地循环变量的值终止  ：

```csharp
i < 5
```

### <a name="the-iterator-section"></a>“迭代器”部分 

“迭代器”部分定义循环主体的每次迭代后将执行的操作  。 “迭代器”部分包含用逗号分隔的零个或多个以下语句表达式  ：

- [赋值](../operators/assignment-operator.md)语句

- 方法的调用

- 为 [increment](../operators/arithmetic-operators.md#increment-operator-) 表达式添加前缀或后缀，如 `++i` 或 `i++`

- 为 [decrement](../operators/arithmetic-operators.md#decrement-operator---) 表达式添加前缀或后缀，如 `--i` 或 `i--`

- 通过使用 [new](../operators/new-operator.md) 运算符来创建对象

- [await](../operators/await.md) 表达式

上例中的“迭代器”部分增加本地循环变量  ：

```csharp
i++
```

## <a name="examples"></a>示例

下面的示例阐释了几种不太常见的 `for` 语句部分的使用情况：为“初始化表达式”部分中的外部循环变量赋值、同时在“初始化表达式”部分和“迭代器”部分中调用一种方法，以及更改迭代器部分中的两个变量的值     。 选择“运行”以运行示例代码  。 然后可以修改代码并再次运行它。

[!code-csharp-interactive[not typical for loop example](snippets/IterationKeywordsExamples.cs#6)]

以下示例定义无限 `for` 循环：

[!code-csharp[infinite for loop example](snippets/IterationKeywordsExamples.cs#7)]

## <a name="c-language-specification"></a>C# 语言规范

有关详细信息，请参阅 [C# 语言规范](/dotnet/csharp/language-reference/language-specification/introduction)中的 [for 语句](~/_csharplang/spec/statements.md#the-for-statement)部分。

## <a name="see-also"></a>请参阅

- [C# 参考](../index.md)
- [C# 编程指南](../../programming-guide/index.md)
- [C# 关键字](index.md)
- [foreach, in](foreach-in.md)
