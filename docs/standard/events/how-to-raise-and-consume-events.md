---
title: 如何：引发事件和使用事件
description: 在 .NET 中引发和使用事件。 请参阅使用 EventHandler 委托、EventHandler<TEventArgs> 委托以及自定义委托的示例。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- events [.NET], raising
- raising events
- events [.NET], samples
ms.assetid: 42afade7-3a02-4f2e-868b-95845f302f8f
ms.openlocfilehash: 22e62dd8e14696273923873353b1bd19d8507ab7
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95697449"
---
# <a name="how-to-raise-and-consume-events"></a>如何：引发事件和使用事件

本主题中的示例演示如何处理事件。 它们包含 <xref:System.EventHandler>、<xref:System.EventHandler%601> 委托和自定义委托的示例，用于说明包含数据和不包含数据的事件。  
  
 这些示例使用[事件](index.md)一文中介绍的概念。  
  
## <a name="example"></a>示例  

 第一个示例演示如何引发和使用一个没有数据的事件。 它包含一个名为 `Counter` 类，该类具有一个名为 `ThresholdReached` 的事件。 当计数器值等于或者超过阈值时，将引发此事件。 <xref:System.EventHandler> 委托与此事件关联，因为没有提供任何事件数据。  
  
 [!code-csharp[EventsOverview#5](../../../samples/snippets/csharp/VS_Snippets_CLR/eventsoverview/cs/programnodata.cs#5)]
 [!code-vb[EventsOverview#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/eventsoverview/vb/module1nodata.vb#5)]  
  
## <a name="example"></a>示例  

 下一个示例演示如何引发和使用提供数据的事件。 <xref:System.EventHandler%601> 委托与此事件关联，示例还提供了一个自定义事件数据对象的实例。  
  
 [!code-cpp[EventsOverview#6](../../../samples/snippets/cpp/VS_Snippets_CLR/eventsoverview/cpp/programwithdata.cpp#6)]
 [!code-csharp[EventsOverview#6](../../../samples/snippets/csharp/VS_Snippets_CLR/eventsoverview/cs/programwithdata.cs#6)]
 [!code-vb[EventsOverview#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR/eventsoverview/vb/module1withdata.vb#6)]  
  
## <a name="example"></a>示例  

 下一个示例演示如何声明事件的委托。 该委托名为 `ThresholdReachedEventHandler`。 这只是一个示例。 通常不需要为事件声名委托，因为可以使用 <xref:System.EventHandler> 或者 <xref:System.EventHandler%601> 委托。 只有在极少数情况下才应声明委托，例如，在向无法使用泛型的旧代码提供类时，就需要如此。  
  
 [!code-csharp[EventsOverview#7](../../../samples/snippets/csharp/VS_Snippets_CLR/eventsoverview/cs/programwithdelegate.cs#7)]
 [!code-vb[EventsOverview#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR/eventsoverview/vb/module1withdelegate.vb#7)]  
  
## <a name="see-also"></a>请参阅

- [事件](index.md)
