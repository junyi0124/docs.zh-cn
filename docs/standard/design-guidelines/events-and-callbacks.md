---
title: 事件和回调
ms.date: 10/22/2008
helpviewer_keywords:
- events [.NET Framework], extensibility
- methods [.NET Framework], callback
- callback methods
- callbacks
ms.assetid: 48b55c60-495f-4089-9396-97f9122bba7c
ms.openlocfilehash: c63a88cb4e500504f993352a03478f40cad58400
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95734722"
---
# <a name="events-and-callbacks"></a>事件和回调

回调是允许框架通过委托回调到用户代码的扩展点。 通常通过方法的参数将这些委托传递到框架。

 事件是回调的一种特殊情况，它支持将委托 (事件处理程序) 提供便利且一致的语法。 此外，Visual Studio 的语句完成和设计器还提供了有关使用基于事件的 Api 的帮助。  (参阅 [事件设计](event.md)。 ) 

 ✔️考虑使用回调来允许用户提供要由框架执行的自定义代码。

 ✔️考虑使用事件来允许用户自定义框架的行为，而无需了解面向对象的设计。

 ✔️确实比普通回调更喜欢事件，因为它们对更广泛的开发人员更熟悉，并与 Visual Studio 语句完成集成。

 ❌ 避免在性能敏感的 Api 中使用回调。

 ✔️在使用回调定义 Api 时，请使用新的 `Func<...>` 、 `Action<...>` 或 `Expression<...>` 类型而不是自定义委托。

 `Func<...>` 和 `Action<...>` 表示泛型委托。 `Expression<...>` 表示可以编译并随后在运行时调用的函数定义，但也可以进行序列化并传递到远程进程。

 ✔️使用而不 `Expression<...>` 是使用和委托来度量和了解性能的含义 `Func<...>` `Action<...>` 。

 `Expression<...>` 类型在逻辑上与 `Func<...>` 和 `Action<...>` 委托等效。 它们之间的主要区别在于委托旨在用于本地处理方案;表达式适用于在远程进程或计算机中计算表达式的好处。

 ✔️可以通过调用委托来了解这一点，因为你正在执行任意代码，这可能会对安全性、正确性和兼容性产生影响。

 *部分 &copy; 2005，2009 Microsoft Corporation。保留所有权利。*

 *经许可重印皮尔逊教育，Inc. 的作者 [：从框架设计指导原则：用于可重复使用的 .Net 库的约定、惯例和模式; 第2版](https://www.informit.com/store/framework-design-guidelines-conventions-idioms-and-9780321545619) By Krzysztof Cwalina，Brad Abrams，通过 Addison-Wesley Professional 作为 Microsoft Windows 开发系列的一部分2008发布。*

## <a name="see-also"></a>另请参阅

- [扩展性设计](designing-for-extensibility.md)
- [框架设计准则](index.md)
