---
title: 事件设计
ms.date: 10/22/2008
helpviewer_keywords:
- pre-events
- events [.NET Framework], design guidelines
- member design guidelines, events
- event handlers [.NET Framework], event design guidelines
- post-events
- signatures, event handling
ms.assetid: 67b3c6e2-6a8f-480d-a78f-ebeeaca1b95a
ms.openlocfilehash: d04ffd2cab21177f1342a13259a81df22b65723a
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95734408"
---
# <a name="event-design"></a>事件设计

事件是最常用的回调形式 (构造，使框架可以调入用户代码) 。 其他回调机制包括采用委托、虚拟成员和基于接口的插件的成员。可用性研究中的数据表明，大多数开发人员比使用其他回调机制更喜欢使用事件。 事件与 Visual Studio 和多种语言完美集成。

 需要注意的是，有两组事件：在系统状态发生更改之前引发的事件（称为前期事件）和状态更改后引发的事件（称为后期事件）。 预事件的一个示例是 `Form.Closing` ，在关闭窗体前引发。 后事件的一个示例是 `Form.Closed` ，它在窗体关闭后引发。

 ✔️对事件（而不是 "火灾" 或 "trigger"）使用术语 "引发"。

 ✔️使用 <xref:System.EventHandler%601?displayProperty=nameWithType> 而不是手动创建要用作事件处理程序的新委托。

 ✔️考虑使用 <xref:System.EventArgs> 作为事件参数的子类，除非你完全确保该事件从不需要将任何数据传递到事件处理方法，在这种情况下，你可以直接使用该 `EventArgs` 类型。

 如果直接使用发送 API `EventArgs` ，则永远不能添加与事件一起使用的任何数据，而不会破坏兼容性。 如果使用子类（即使最初完全为空），则在需要时可以将属性添加到子类。

 ✔️使用受保护的虚拟方法引发每个事件。 这仅适用于未密封的类上的非静态事件，而不适用于结构、密封类或静态事件。

 此方法的目的是为派生类提供一种使用替代来处理事件的方法。 重写是处理派生类中的基类事件的更灵活、更快、更自然的方式。 按照约定，方法的名称应以 "On" 开头，后面跟有事件的名称。

 派生类可以选择不在其重写中调用方法的基实现。 为此，请不要在基类正常工作所需的方法中包括任何处理。

 ✔️会对引发事件的受保护方法执行一个参数。

 应将参数命名为 `e` ，并将其类型化为事件参数类。

 ❌ 引发非静态事件时，不要将 null 作为发件人传递。

 ✔️在引发静态事件时将 null 作为发件人传递。

 ❌ 引发事件时，请勿传递 null 作为事件数据参数。

 `EventArgs.Empty`如果不想将任何数据传递到事件处理方法，则应传递。 开发人员希望此参数不为 null。

 ✔️考虑引发最终用户可以取消的事件。 这仅适用于预事件。

 使用 <xref:System.ComponentModel.CancelEventArgs?displayProperty=nameWithType> 或其子类作为事件参数，以允许最终用户取消事件。

### <a name="custom-event-handler-design"></a>自定义事件处理程序设计

 有些情况下 `EventHandler<T>` ，不能使用，例如当框架需要使用早期版本的 CLR 时，这种情况不支持泛型。 在这种情况下，可能需要设计和开发自定义事件处理程序委托。

 ✔️使用 void 的返回类型作为事件处理程序。

 事件处理程序可以调用多个事件处理方法（可能在多个对象上）。 如果允许事件处理方法返回值，则每个事件调用都有多个返回值。

 ✔️使用 `object` 作为事件处理程序的第一个参数的类型，并调用它 `sender` 。

 ✔️使用 <xref:System.EventArgs?displayProperty=nameWithType> 或其子类作为事件处理程序的第二个参数的类型，并调用它 `e` 。

 ❌ 在事件处理程序中，不能有两个以上的参数。

 *部分©2005，2009 Microsoft Corporation。保留所有权利。*

 *经许可重印皮尔逊教育，Inc. 的作者 [：从框架设计指导原则：用于可重复使用的 .Net 库的约定、惯例和模式; 第2版](https://www.informit.com/store/framework-design-guidelines-conventions-idioms-and-9780321545619) By Krzysztof Cwalina，Brad Abrams，通过 Addison-Wesley Professional 作为 Microsoft Windows 开发系列的一部分2008发布。*

## <a name="see-also"></a>另请参阅

- [成员设计准则](member.md)
- [框架设计准则](index.md)
