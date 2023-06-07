---
title: 如何调用事件处理程序
ms.date: 07/20/2015
helpviewer_keywords:
- Visual Basic code, procedures
- event handlers [Visual Basic], calling
- event handlers
- procedures [Visual Basic], event handlers
- procedures [Visual Basic], calling
no-loc:
- WithEvents
ms.assetid: 72e18ef8-144e-40df-a1f4-066a57271e28
ms.openlocfilehash: 3762c79dd3d883ae2ccfe76b335cf98ac87d4246
ms.sourcegitcommit: e7acba36517134238065e4d50bb4a1cfe47ebd06
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/04/2020
ms.locfileid: "89464956"
---
# <a name="how-to-call-an-event-handler-in-visual-basic"></a>如何在 Visual Basic 中调用事件处理程序

事件是指由某些程序组件识别的操作或 *事件* （例如鼠标单击或信用限制），可以为其编写代码来做出响应。 *事件处理程序*是为响应事件而编写的代码。

Visual Basic 中的事件处理程序是一个 `Sub` 过程。 不过，通常不会像其他过程那样调用它 `Sub` 。 而是将该过程标识为事件的处理程序。 可以通过 [`Handles`](../../../language-reference/statements/handles-clause.md) 子句和 [`WithEvents`](../../../language-reference/modifiers/withevents.md) 变量或使用 [AddHandler 语句](../../../language-reference/statements/addhandler-statement.md)来执行此操作。 使用 `Handles` 子句是在 Visual Basic 中声明事件处理程序的默认方式。 当你在集成开发环境中 (IDE) 编程时，这就是设计器编写事件处理程序的方式。 `AddHandler`语句适用于在运行时动态引发事件。

事件发生时，Visual Basic 会自动调用事件处理程序过程。 有权访问事件的任何代码都可以通过执行 [RaiseEvent 语句](../../../language-reference/statements/raiseevent-statement.md)来导致事件发生。

可以将多个事件处理程序与同一事件关联。 在某些情况下，您可以取消关联事件的处理程序。 有关详细信息，请参阅[事件](../events/index.md)。

## <a name="call-an-event-handler-using-no-loc-texthandles-and-no-locwithevents"></a>使用和调用事件处理程序 :::no-loc text="Handles":::WithEvents

1. 请确保事件是用 [事件语句](../../../language-reference/statements/event-statement.md)声明的。

2. 使用关键字在模块或类级别声明对象变量 [`WithEvents`](../../../language-reference/modifiers/withevents.md) 。 `As`此变量的子句必须指定引发事件的类。

3. 在事件处理过程的声明中 `Sub` ，添加一个 [`Handles`](../../../language-reference/statements/handles-clause.md) 指定 `WithEvents` 变量和事件名称的子句。

4. 事件发生时，Visual Basic 自动调用 `Sub` 过程。 你的代码可以使用 `RaiseEvent` 语句来使事件发生。

    下面的示例定义了一个事件和一个 `WithEvents` 引用引发事件的类的变量。 事件处理 `Sub` 过程使用 `Handles` 子句来指定它处理的类和事件。

    :::code language="vb" source="snippets/how-to-call-an-event-handler/SpecialForm.vb" id="4":::

## <a name="call-an-event-handler-using-addhandler"></a>使用 AddHandler 调用事件处理程序

1. 请确保用语句声明了该事件 `Event` 。

2. 执行 [AddHandler 语句](../../../language-reference/statements/addhandler-statement.md) 以使用事件动态连接事件处理 `Sub` 过程。

3. 事件发生时，Visual Basic 自动调用 `Sub` 过程。 你的代码可以使用 `RaiseEvent` 语句来使事件发生。

    下面的示例使用构造函数中的 [AddHandler 语句](../../../language-reference/statements/addhandler-statement.md) 将过程与的 `OnFormClosing` 事件处理程序相关联 <xref:System.Windows.Forms.Form.FormClosing> 。

    :::code language="vb" source="snippets/how-to-call-an-event-handler/SpecialForm.vb" id="5":::

    可以通过执行 [RemoveHandler 语句](../../../language-reference/statements/removehandler-statement.md)，将事件处理程序与事件取消关联。

## <a name="see-also"></a>另请参阅

- [过程](index.md)
- [Sub 过程](sub-procedures.md)
- [Sub 语句](../../../language-reference/statements/sub-statement.md)
- [AddressOf 运算符](../../../language-reference/operators/addressof-operator.md)
- [如何：创建过程](how-to-create-a-procedure.md)
- [如何：调用不返回值的过程](how-to-call-a-procedure-that-does-not-return-a-value.md)
