---
title: 处理和引发事件
description: 了解如何处理并引发基于委托模型的 .NET 事件。 此模型允许订阅者向提供商注册或接收通知。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- delegate model for events
- application development [.NET], events
- application development [.NET], events
- application development [.NET Core], events
- events [.NET]
- events [.NET Core]
- events [.NET]
ms.assetid: b6f65241-e0ad-4590-a99f-200ce741bb1f
ms.openlocfilehash: e0b8de574475490cd3b15383b6ebc2ace4b74663
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94828265"
---
# <a name="handle-and-raising-events"></a>处理和引发事件

.NET 中的事件基于委托模型。 委托模型遵循[观察者设计模式](observer-design-pattern.md)，使订阅者能够向提供方注册并接收相关通知。 事件发送方推送事件发生的通知，事件接收器接收该通知并定义对它的响应。 本文介绍委托模型的主要组件、如何在应用程序中使用事件以及如何在你的代码中实现事件。
  
## <a name="events"></a>事件

事件是由对象发送的用于发出操作信号的消息。 该操作可能是由用户交互引起，例如单击按钮；也可能是由某些其他程序的逻辑导致，例如更改属性值。 引发事件的对象称为“事件发送方”。 事件发送方不知道哪个对象或方法将接收（处理）它引发的事件。 事件通常是事件发送方的成员，例如 <xref:System.Web.UI.WebControls.Button.Click> 事件是 <xref:System.Web.UI.WebControls.Button> 类的成员，<xref:System.ComponentModel.INotifyPropertyChanged.PropertyChanged> 事件是实现  <xref:System.ComponentModel.INotifyPropertyChanged> 接口的类的成员。  
  
若要定义一个事件，可以在事件类签名中使用 [`event`](../../csharp/language-reference/keywords/event.md)（在 C# 中）或 [`Event`](../../visual-basic/language-reference/statements/event-statement.md)（在 Visual Basic 中）关键字，并指定事件的委托类型。 委托在下一节中介绍。  
  
通常，为了引发事件，您可以在 C# 中添加一个标记为 `protected` 和 `virtual` 或在 Visual Basic 中标记为 `Protected` 和 `Overridable` 的方法。 将此方法命名为 `On`*EventName*；例如，`OnDataReceived`。 此方法应接受一个指定事件数据对象（<xref:System.EventArgs> 类型或派生类型）的参数。 您提供此方法以允许派生类重写引发事件的逻辑。 派生类应始终调用基类的 `On`*EventName* 方法，以确保注册的委托接收事件。  

下面的示例显示如何声明名为 `ThresholdReached` 事件。 该事件与 <xref:System.EventHandler> 委托相关联并且由 `OnThresholdReached` 方法引发。  
  
 [!code-csharp[EventsOverview#1](~/samples/snippets/csharp/VS_Snippets_CLR/eventsoverview/cs/programtruncated.cs#1)]
 [!code-vb[EventsOverview#1](~/samples/snippets/visualbasic/VS_Snippets_CLR/eventsoverview/vb/module1truncated.vb#1)]  
  
## <a name="delegates"></a>委托

委托是一种保存对方法的引用的类型。 委托是通过显示所引用方法的返回类型和参数的签名来声明的，并可以仅保存与其签名匹配的方法的引用。 因此，委托等同于类型安全函数指针或回叫。 委托声明足以定义委托类。  
  
委托在 .NET 中有许多用途。 在事件上下文中，委托是事件源和处理事件的代码之间的媒介（或类似指针的机制）。 如上一节的例子所示，可以通过在事件声明中包括委托类型来将委托和事件相关联。 有关委托的详细信息，请参阅 <xref:System.Delegate> 类。  
  
.NET 提供了 <xref:System.EventHandler> 和 <xref:System.EventHandler%601> 委托来支持大部分事件场景。 使用 <xref:System.EventHandler> 委托处理不包含事件数据的所有事件。 使用 <xref:System.EventHandler%601> 委托处理包含事件相关数据的事件。 这些委托没有返回类型值，并且接受两个参数（事件源的对象和事件数据的对象）。  
  
委托是[多播](xref:System.MulticastDelegate)，这意味着它们可以保存对多个事件处理方法的引用。 有关详细信息，请参阅 <xref:System.Delegate> 参考页。 委托提供了事件处理中的灵活性和精确控制。 委托人通过维护事件的已注册事件处理程序列表来充当引发事件的类的事件调度程序。  
  
在 <xref:System.EventHandler> 和 <xref:System.EventHandler%601> 委托不可用的场景下，您可以定义一个委托。 要求你定义委托的场景非常少见的，例如，当你必须处理无法识别泛型的代码时。 在声明中使用 [`delegate`](../../csharp/language-reference/builtin-types/reference-types.md#the-delegate-type)（在 C# 中）和 [`Delegate`](../../visual-basic/language-reference/statements/delegate-statement.md)（在 Visual Basic 中）关键字标记委托。 下面的示例说明如何声明 `ThresholdReachedEventHandler` 委托。  
  
[!code-csharp[EventsOverview#4](~/samples/snippets/csharp/VS_Snippets_CLR/eventsoverview/cs/programtruncated.cs#4)]
[!code-vb[EventsOverview#4](~/samples/snippets/visualbasic/VS_Snippets_CLR/eventsoverview/vb/module1truncated.vb#4)]  
  
## <a name="event-data"></a>事件数据

与事件相关的数据可以通过事件数据类提供。 .NET 提供了许多事件数据类，用户可以在自己的应用程序中使用它们。 例如，<xref:System.IO.Ports.SerialDataReceivedEventArgs> 类是 <xref:System.IO.Ports.SerialPort.DataReceived?displayProperty=nameWithType> 事件的事件数据类。 .NET 遵循所有事件数据类以 `EventArgs` 结尾的命名模式。 您通过查看事件的委托来确定哪个事件数据类与事件相关联。 例如，<xref:System.IO.Ports.SerialDataReceivedEventHandler> 委托包含 <xref:System.IO.Ports.SerialDataReceivedEventArgs> 类作为它的一个参数。  
  
<xref:System.EventArgs> 类是所有事件数据类的基类型。 当一个事件没有任何与其相关联的数据时，您也会用到 <xref:System.EventArgs> 类。 当您创建一个事件仅用来通知其他类出问题了，不需要传递任何数据时，请包括 <xref:System.EventArgs> 类作为委托中的第二个参数。 当没有数据提供时，您可以传递 <xref:System.EventArgs.Empty?displayProperty=nameWithType> 值。 <xref:System.EventHandler> 委托包括 <xref:System.EventArgs> 类作为一个参数。  
  
当您想创建一个自定义的事件数据类时，请创建一个派生自 <xref:System.EventArgs> 的类，然后提供所需的所有成员，来传递与该事件相关的数据。 通常，应使用与 .NET 相同的命名模式，并且事件数据类名称应以 `EventArgs` 结尾。  
  
下面的示例演示了一个名为 `ThresholdReachedEventArgs` 的事件数据类。 它包含特定于引发事件的属性。  
  
[!code-csharp[EventsOverview#3](~/samples/snippets/csharp/VS_Snippets_CLR/eventsoverview/cs/programtruncated.cs#3)]
[!code-vb[EventsOverview#3](~/samples/snippets/visualbasic/VS_Snippets_CLR/eventsoverview/vb/module1truncated.vb#3)]  
  
## <a name="event-handlers"></a>事件处理程序

若要响应事件，您需要在事件接收器中定义一个事件处理程序方法。 此方法必须与您正处理的事件的委托签名匹配。 在事件处理程序中，当事件引发时会执行所需操作，例如在用户单击按钮之后收集用户输入。 若当事件发生时收到通知，您的事件处理程序方法必须订阅该事件。  
  
下面的示例演示与 `c_ThresholdReached` 委托的签名匹配的 <xref:System.EventHandler> 事件处理程序方法。 该方法订阅 `ThresholdReached` 事件。  
  
[!code-csharp[EventsOverview#2](~/samples/snippets/csharp/VS_Snippets_CLR/eventsoverview/cs/programtruncated.cs#2)]
[!code-vb[EventsOverview#2](~/samples/snippets/visualbasic/VS_Snippets_CLR/eventsoverview/vb/module1truncated.vb#2)]  
  
## <a name="static-and-dynamic-event-handlers"></a>静态和动态事件处理程序  

借助 .NET，订阅者可以进行静态或动态注册以获得事件通知。 对于其事件由静态事件处理程序进行处理的类，静态事件处理程序对其整个生命周期有效。 通常为响应某些条件程序逻辑，会在程序执行期间显式激活和停用动态事件处理程序。 例如，如果仅在特定条件下需要事件通知，或如果应用程序提供多个事件处理程序且由运行时条件定义要使用的适当事件处理程序，则可以使用动态事件处理程序。 上一节中的示例演示如何动态添加事件处理程序。 有关详细信息，请查看 Visual Basic 中的[事件](../../visual-basic/programming-guide/language-features/events/index.md)和 C# 中的[事件](../../csharp/programming-guide/events/index.md)。  
  
## <a name="raising-multiple-events"></a>引发多个事件

 如果您的类引发多个事件，编译器会为每一个事件委托实例生成一个字段。 如果事件数量很大，则可能无法接受按一个委托计算一个字段的存储成本。 对于这些情况，.NET 提供一个事件属性，可以将其与选择的另一数据结构一起用于存储事件委托。  
  
 事件属性由事件声明和事件访问器组成。 事件访问器是您定义的方法，用来从存储数据结构添加和移除事件委托实例。 请注意，事件属性要比事件字段慢，这是因为必须先检索每个事件委托，然后才能调用它。 需在内存和速度之间进行权衡。 如果类定义许多不常引发的事件，那么需要实现事件属性。 有关详细信息，请参阅[如何：使用事件属性处理多个事件](how-to-handle-multiple-events-using-event-properties.md)。  
  
## <a name="related-articles"></a>相关文章
  
|Title|描述|  
|-----------|-----------------|  
|[如何：抛出和使用事件](how-to-raise-and-consume-events.md)|包含引发和使用事件的示例。|  
|[如何：使用事件属性处理多个事件](how-to-handle-multiple-events-using-event-properties.md)|演示如何使用事件属性处理多个事件。|  
|[观察程序设计模式](observer-design-pattern.md)|描述允许订阅者向提供方注册和接收通知的设计模式。|
  
## <a name="see-also"></a>请参阅

- <xref:System.EventHandler>
- <xref:System.EventHandler%601>
- <xref:System.EventArgs>
- <xref:System.Delegate>
- [事件 (Visual Basic)](../../visual-basic/programming-guide/language-features/events/index.md)
- [事件（C# 编程指南）](../../csharp/programming-guide/events/index.md)
- [事件和路由事件概述（UWP 应用）](/windows/uwp/xaml-platform/events-and-routed-events-overview)
- [Windows 应用商店 8.x 应用中的事件](/previous-versions/windows/apps/hh758286(v=win.10))
