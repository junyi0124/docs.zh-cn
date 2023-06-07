---
title: 如何：声明自定义事件以避免阻止
ms.date: 07/20/2015
helpviewer_keywords:
- declaring events [Visual Basic], custom
- events [Visual Basic], custom
- custom events [Visual Basic]
ms.assetid: 998b6a90-67c5-4d2c-8b11-366d3e355505
ms.openlocfilehash: a9350470836652f65532068402c78375b4c5495c
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91077093"
---
# <a name="how-to-declare-custom-events-to-avoid-blocking-visual-basic"></a>如何：声明自定义事件以避免阻止 (Visual Basic)

在某些情况下，很重要的一点是，一个事件处理程序不会阻止后续事件处理程序。 自定义事件允许事件异步调用其事件处理程序。  
  
 默认情况下，事件声明的 "备份存储" 字段为一个多播委托，该委托按顺序合并所有事件处理程序。 这意味着，如果某个处理程序需要很长时间才能完成，则会阻止其他处理程序完成。  (操作良好的事件处理程序不应执行冗长或可能阻塞的操作。 )   
  
 您可以使用自定义事件异步执行事件处理程序，而不是使用 Visual Basic 提供的事件的默认实现。  
  
## <a name="example"></a>示例  

 在此示例中， `AddHandler` 访问器将事件的每个处理程序的委托添加 `Click` 到 <xref:System.Collections.ArrayList> 存储在 `EventHandlerList` 字段中的。  
  
 当代码引发 `Click` 事件时， `RaiseEvent` 访问器使用方法以异步方式调用所有事件处理程序委托 <xref:System.Web.Services.Protocols.LogicalMethodInfo.BeginInvoke%2A> 。 该方法调用工作线程上的每个处理程序并立即返回，因此处理程序不能彼此阻止。  
  
 [!code-vb[VbVbalrEvents#27](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrEvents/VB/Class1.vb#27)]  
  
## <a name="see-also"></a>请参阅

- <xref:System.Collections.ArrayList>
- <xref:System.Web.Services.Protocols.LogicalMethodInfo.BeginInvoke%2A>
- [事件](index.md)
- [如何：声明自定义事件以节省内存](how-to-declare-custom-events-to-conserve-memory.md)
