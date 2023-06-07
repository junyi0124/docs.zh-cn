---
description: event - C# 参考
title: event - C# 参考
ms.date: 07/20/2015
f1_keywords:
- event
- remove
- event_CSharpKeyword
- add
helpviewer_keywords:
- event keyword [C#]
ms.assetid: 7858fd85-153b-4259-85d0-6aa13c35f174
ms.openlocfilehash: dff93496dfeceaf67777abe0b02ee2d620b3a5ca
ms.sourcegitcommit: e7acba36517134238065e4d50bb4a1cfe47ebd06
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/04/2020
ms.locfileid: "89466035"
---
# <a name="event-c-reference"></a>event (C# 参考)

`event` 关键字用于声明发布服务器类中的事件。

## <a name="example"></a>示例

下面的示例演示如何声明和引发使用 <xref:System.EventHandler> 作为基础委托类型的事件。 要查看完整代码示例了解如何使用泛型 <xref:System.EventHandler%601> 委托类型以及如何订阅事件并创建事件处理程序方法，请参阅[如何发布符合 .NET 准则的事件](../../programming-guide/events/how-to-publish-events-that-conform-to-net-framework-guidelines.md)。

[!code-csharp[csrefKeywordsModifiers#7](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csrefKeywordsModifiers/CS/csrefKeywordsModifiers.cs#7)]

事件是一种特殊的多播委托，仅可以从声明事件的类或结构（发布服务器类）中对其进行调用。 如果其他类或结构订阅该事件，则在发布服务器类引发该事件时，将调用其事件处理程序方法。 有关详细信息和代码示例，请参阅[事件](../../programming-guide/events/index.md)和[委托](../../programming-guide/delegates/index.md)。

可以将事件标记为[public](./public.md)、[private](./private.md)、[protected](./protected.md)、[internal](./internal.md)、[protected internal](./protected-internal.md) 或 [private protected](./private-protected.md)。 这些访问修饰符定义该类的用户访问该事件的方式。 有关详细信息，请参阅[访问修饰符](../../programming-guide/classes-and-structs/access-modifiers.md)。

## <a name="keywords-and-events"></a>关键字和事件

下列关键字应用于事件。

|关键字|说明|更多信息|
|-------------|-----------------|--------------------------|
|[static](./static.md)|使事件可供调用方在任何时候进行调用，即使不存在类的实例。|[静态类和静态类成员](../../programming-guide/classes-and-structs/static-classes-and-static-class-members.md)|
|[virtual](./virtual.md)|允许派生类使用[重写](./override.md)关键字重写事件行为。|[继承](../../programming-guide/classes-and-structs/inheritance.md)|
|[sealed](./sealed.md)|指定对于派生类，它不再是虚拟的。||
|[abstract](./abstract.md)|编译器不会生成 `add` 和 `remove` 事件访问器块，因此派生类必须提供其自己的实现。||

可以通过使用[静态](./static.md)关键字将事件声明为静态事件。 这可使事件可供调用方在任何时候进行调用，即使不存在类的实例。 有关详细信息，请参阅[静态类和静态类成员](../../programming-guide/classes-and-structs/static-classes-and-static-class-members.md)。

可以通过使用[虚拟](./virtual.md)关键字将事件标记为虚事件。 这可使派生类使用[重写](./override.md)关键字重写事件行为。 有关详细信息，请参阅[继承](../../programming-guide/classes-and-structs/inheritance.md)。 重写虚拟事件的事件也可以为[密封](./sealed.md)，指定对于派生类，它不再是虚拟的。 最后，可以声明事件为[抽象](./abstract.md)，这意味着编译器将不会生成 `add` 和 `remove` 事件访问器块。 因此，派生类必须提供其自己的实现。

## <a name="c-language-specification"></a>C# 语言规范

[!INCLUDE[CSharplangspec](~/includes/csharplangspec-md.md)]

## <a name="see-also"></a>请参阅

- [C# 参考](../index.md)
- [C# 编程指南](../../programming-guide/index.md)
- [C# 关键字](./index.md)
- [add](./add.md)
- [remove](./remove.md)
- [修饰符](index.md)
- [如何合并委托（多播委托）](../../programming-guide/delegates/how-to-combine-delegates-multicast-delegates.md)
