---
title: 事件 - C# 编程指南
description: 了解事件。 类或对象可以通过事件向其他类或对象通知发生的相关事情。
ms.date: 07/20/2015
helpviewer_keywords:
- classes [C#], events
- C# language, events
- events [C#]
ms.assetid: a8e51b22-d294-44fb-9539-0072f06c4cb3
ms.openlocfilehash: 14c18006e393dece5d32d30c2a727d797515c779
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91167452"
---
# <a name="events-c-programming-guide"></a>事件（C# 编程指南）

[类](../../language-reference/keywords/class.md) 或对象可以通过事件向其他类或对象通知发生的相关事情。 发送（或引发）事件的类称为“发布者”，接收（或处理）事件的类称为“订阅者”。  
  
在典型的 C# Windows 窗体或 Web 应用程序中，可订阅由按钮和列表框等控件引发的事件。 可以使用 Visual C# 集成开发环境 (IDE) 来浏览控件发布的事件，并选择想要处理的事件。 借助 IDE，可轻松自动添加空白事件处理程序方法以及要订阅该事件的代码。 有关详细信息，请参阅[如何订阅和取消订阅事件](./how-to-subscribe-to-and-unsubscribe-from-events.md)。
  
## <a name="events-overview"></a>事件概述  

 事件具有以下属性：  
  
- 发行者确定何时引发事件；订户确定对事件作出何种响应。  
  
- 一个事件可以有多个订户。 订户可以处理来自多个发行者的多个事件。  
  
- 没有订户的事件永远也不会引发。  
  
- 事件通常用于表示用户操作，例如单击按钮或图形用户界面中的菜单选项。  
  
- 当事件具有多个订户时，引发该事件时会同步调用事件处理程序。 若要异步调用事件，请参阅 “[使用异步方式调用同步方法](../../../standard/asynchronous-programming-patterns/calling-synchronous-methods-asynchronously.md)”。  
  
- 在 .NET 类库中，事件基于 <xref:System.EventHandler> 委托和 <xref:System.EventArgs> 基类。  
  
## <a name="related-sections"></a>相关章节  

 有关详细信息，请参见:  
  
- [如何订阅和取消订阅事件](./how-to-subscribe-to-and-unsubscribe-from-events.md)

- [如何发布符合 .NET 准则的事件](./how-to-publish-events-that-conform-to-net-framework-guidelines.md)

- [如何在派生类中引发基类事件](./how-to-raise-base-class-events-in-derived-classes.md)

- [如何实现接口事件](./how-to-implement-interface-events.md)

- [如何实现自定义事件访问器](./how-to-implement-custom-event-accessors.md)

## <a name="c-language-specification"></a>C# 语言规范  

有关详细信息，请参阅 [C# 语言规范](/dotnet/csharp/language-reference/language-specification/introduction)中的[事件](~/_csharplang/spec/classes.md#events)。 该语言规范是 C# 语法和用法的权威资料。
  
## <a name="featured-book-chapters"></a>重要章节  

 [C# 3.0 手册（第三版）：面向 C# 3.0 程序员的超过 250 个解决方案](/previous-versions/visualstudio/visual-studio-2008/ff518995(v=orm.10))中的[委托、事件和 Lambda 表达式](/previous-versions/visualstudio/visual-studio-2008/ff518994(v=orm.10))  
  
 [学习 C# 3.0：掌握 C# 3.0 基础知识](/previous-versions/visualstudio/visual-studio-2008/ff652493(v=orm.10))中的[委托和事件](/previous-versions/visualstudio/visual-studio-2008/ff652490(v=orm.10))  
  
## <a name="see-also"></a>请参阅

- <xref:System.EventHandler>
- [C# 编程指南](../index.md)
- [委托](../delegates/index.md)
- [在 Windows 窗体中创建事件处理程序](/dotnet/desktop/winforms/creating-event-handlers-in-windows-forms)
