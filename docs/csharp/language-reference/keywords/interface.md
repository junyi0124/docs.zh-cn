---
description: :::no-loc text=interface:::（C#参考）
title: 接口 - C# 参考
ms.date: 01/17/2020
f1_keywords:
- interface_CSharpKeyword
helpviewer_keywords:
- interface keyword [C#]
ms.assetid: 7da38e81-4f99-4bc5-b07d-c986b687eeba
ms.openlocfilehash: 24f95e828522f467c519c0c8a7ba9410aa97af4e
ms.sourcegitcommit: 0802ac583585110022beb6af8ea0b39188b77c43
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "89134583"
---
# <a name="no-loc-textinterface-c-reference"></a>:::no-loc text="interface":::（C#参考）

接口定义协定。 实现该协定的任何 [`class`](class.md) 或 [`struct`](../builtin-types/struct.md) 必须提供接口中定义的成员的实现。 从 C# 8.0 开始，接口可为成员定义默认实现。 它还可以定义 [`static`](static.md) 成员，以便提供常见功能的单个实现。

在以下示例中，类 `ImplementationClass` 必须实现一个不含参数但返回 `void` 的名为 `SampleMethod` 的方法。

有关详细信息和示例，请参阅[接口](../../programming-guide/interfaces/index.md)。

## <a name="example"></a>示例

[!code-csharp[csrefKeywordsTypes#14](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csrefKeywordsTypes/CS/keywordsTypes.cs#14)]

接口可以是命名空间或类的成员。 接口声明可以包含以下成员的声明（没有任何实现的签名）：

- [方法](../../programming-guide/classes-and-structs/methods.md)
- [属性](../../programming-guide/classes-and-structs/using-properties.md)
- [索引器](../../programming-guide/indexers/using-indexers.md)
- [事件](event.md)

上述成员声明通常不包含主体。 从 C# 8.0 开始，接口成员可以声明主体。 这称为“默认实现”。 具有主体的成员允许接口为不提供重写实现的类和结构提供“默认”实现。 此外，从 C# 8.0 开始，接口可以包括：

- [常量](const.md)
- [运算符](../operators/operator-overloading.md)
- [静态构造函数](../../programming-guide/classes-and-structs/constructors.md#static-constructors)。
- [嵌套类型](../../programming-guide/classes-and-structs/nested-types.md)
- [静态字段、方法、属性、索引和事件](static.md)
- 使用显式接口实现语法的成员声明。
- 显式访问修饰符（默认访问权限为 [`public`](access-modifiers.md)）。

接口不能包含实例状态。 虽然现在允许使用静态字段，但接口中不允许使用实例字段。 接口中不支持[实例自动属性](../../programming-guide/classes-and-structs/auto-implemented-properties.md)，因为它们将隐式声明隐藏的字段。 此规则对属性声明有细微影响。 在接口声明中，以下代码不会像在 `class` 或 `struct` 中一样声明自动实现的属性。 相反，它会声明一个属性，该属性没有默认实现，而必须在该实现接口的任何类型中实现它：

```csharp
public interface INamed
{
  public string Name {get; set;}
}
```

一个接口可从一个或多个基接口继承。 当接口 [重写基接口中的方法实现](override.md)时，必须使用[显式接口实现](../../programming-guide/interfaces/explicit-interface-implementation.md)语法。

基类型列表包含基类和接口时，基类必须是列表中的第 1 项。

实现接口的类可以显式实现该接口的成员。 显式实现的成员不能通过类实例访问，而只能通过接口实例访问。 此外，只能通过接口实例访问默认接口成员。

有关显式接口实现的详细信息，请参阅[显式接口实现](../../programming-guide/interfaces/explicit-interface-implementation.md)。

## <a name="example"></a>示例

下例演示了接口实现。 在此示例中，接口包含属性声明，类包含实现。 实现 `IPoint` 的类的任何实例都具有整数属性 `x` 和 `y`。

[!code-csharp[csrefKeywordsTypes#15](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csrefKeywordsTypes/CS/keywordsTypes.cs#15)]

## <a name="c-language-specification"></a>C# 语言规范

有关详细信息，请参阅 [C# 语言规范](~/_csharplang/spec/introduction.md)的[接口](~/_csharplang/spec/interfaces.md)部分和 [默认接口成员 - C# 8.0](~/_csharplang/proposals/csharp-8.0/default-interface-methods.md) 的功能规范

## <a name="see-also"></a>请参阅

- [C# 参考](../index.md)
- [C# 编程指南](../../programming-guide/index.md)
- [C# 关键字](index.md)
- [引用类型](reference-types.md)
- [接口](../../programming-guide/interfaces/index.md)
- [使用属性](../../programming-guide/classes-and-structs/using-properties.md)
- [使用索引器](../../programming-guide/indexers/using-indexers.md)
