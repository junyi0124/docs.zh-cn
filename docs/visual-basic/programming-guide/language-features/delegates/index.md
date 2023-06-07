---
title: 委托
ms.date: 07/20/2015
helpviewer_keywords:
- delegates [Visual Basic]
- Visual Basic code, delegates
ms.assetid: 410b60dc-5e60-4ec0-bfae-426755a2ee28
ms.openlocfilehash: 1f161248fa04f8fab0e5335413e69ca565732f71
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84410678"
---
# <a name="delegates-visual-basic"></a>委托 (Visual Basic)

委托是引用方法的对象。 有时亦称为*类型安全函数指针*，因为它们与其他编程语言中使用的函数指针类似。 但与函数指针不同的是，Visual Basic 委托是基于类的引用类型 <xref:System.Delegate?displayProperty=nameWithType> 。 委托既可以引用共享方法（无需特定类实例即可调用的方法），也可以引用实例方法。

## <a name="delegates-and-events"></a>委托和事件

在过程调用方和被调用的过程之间需要中介的情况下，委托非常有用。 例如，你希望引发事件的对象能够在不同的情况下调用不同的事件处理程序。 遗憾的是，引发事件的对象无法提前确定用于处理特定事件的事件处理程序。 使用 Visual Basic，可以在使用语句时，通过创建委托，动态地将事件处理程序与事件关联 `AddHandler` 。 在运行时，委托会将调用转接到相应的事件处理程序。

虽然您可以创建自己的委托，但在大多数情况下 Visual Basic 会创建委托并处理详细信息。 例如，`Event` 语句将 `<EventName>EventHandler` 委托类隐式定义为包含 `Event` 语句的类的嵌套类，并且具有与事件相同的签名。 `AddressOf` 语句隐式创建引用特定过程的委托的实例。 下面两行代码是等同的。 第一行代码显式创建 `EventHandler` 的实例，将对方法 `Button1_Click` 的引用作为自变量发送。 第二行代码是执行相同操作的更简便方式。

[!code-vb[VbVbalrDelegates#6](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrDelegates/VB/Class1.vb#6)]

只要编译器可通过上下文确定委托类型，就可以采用更简便的方式来创建委托。

## <a name="declaring-events-that-use-an-existing-delegate-type"></a>声明使用现有委托类型的事件

在某些情况下，你可能希望将事件声明为使用现有委托类型作为其基础委托。 下面的语法展示了具体做法：

[!code-vb[VbVbalrDelegates#7](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrDelegates/VB/Class1.vb#7)]

如果要将多个事件路由到同一处理程序，就会发现这非常有用。

## <a name="delegate-variables-and-parameters"></a>委托变量和参数

可以将委托用于与事件无关的其他任务（如自由线程），或将委托与需要在运行时调用不同版本函数的过程结合使用。

例如，假设你有一个分类广告应用程序，其中包含一个显示汽车名称的列表框。 广告按标题（通常是汽车品牌）进行排序。 如果一些汽车品牌前有汽车出厂年份，可能就会遇到问题。 即列表框的内置排序功能只能按字符代码排序；它会先列出标题以日期开头的所有广告，然后列出标题以品牌开头的广告。

若要解决此问题，可以在类中创建以下排序过程：对大多数列表框使用标准的按字母顺序排序，但可在运行时为汽车广告切换到自定义排序过程。 为此，可以使用委托在运行时将自定义排序过程传递给排序类。

## <a name="addressof-and-lambda-expressions"></a>AddressOf 和 Lambda 表达式

每个委托类均可定义将对象方法指定传递到的构造函数。 委托构造函数的自变量必须是对方法的引用或 lambda 表达式。

若要指定对方法的引用，请使用以下语法：

`AddressOf` [`expression`.]`methodName`

`expression` 的编译时类型必须是类或接口的名称，此类或接口包含指定名称的方法，其签名与委托类的签名一致。 `methodName` 可以是共享方法，也可以是实例方法。 即使为类的默认方法创建委托，`methodName` 也不是可选的。

若要指定 lambda 表达式，请使用以下语法：

`Function` ([`parm` As `type`, `parm2` As `type2`, ...]) `expression`

下面的示例展示了用于为委托指定引用的 `AddressOf` 和 lambda 表达式。

[!code-vb[VbVbalrDelegates#15](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrDelegates/VB/Class2.vb#15)]

函数的签名必须与委托类型的签名一致。 有关 lambda 表达式的详细信息，请参阅[Lambda 表达式](../procedures/lambda-expressions.md)。 有关委托的 lambda 表达式和 `AddressOf` 赋值的更多示例，请参阅[宽松委托转换](relaxed-delegate-conversion.md)。

## <a name="related-topics"></a>相关主题

|Title|说明|
|-----------|-----------------|
|[如何：调用委托方法](how-to-invoke-a-delegate-method.md)|通过示例展示了如何将方法与委托相关联，然后通过委托调用相应的方法。|
|[如何：在 Visual Basic 中将过程传递给另一过程](how-to-pass-procedures-to-another-procedure.md)|介绍了如何使用委托将一个过程传递给另一个过程。|
|[宽松委托转换](relaxed-delegate-conversion.md)|介绍了如何向委托或处理程序分配 Sub 和函数，即使是在它们的签名不一致时|
|[事件](../events/index.md)|概述了 Visual Basic 中的事件。|
