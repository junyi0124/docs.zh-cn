---
title: 如何：使用事件属性处理多个事件
description: 了解如何使用事件属性处理多个事件。 定义委托集合、事件键和事件属性。 实现 add 和 remove 访问器方法。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- event properties [.NET]
- multiple events [.NET]
- event handling [.NET], with multiple events
- events [.NET], multiple
ms.assetid: 30047cba-e2fd-41c6-b9ca-2ad7a49003db
ms.openlocfilehash: 7484ad06e80e6ce131f48431fbdd1e812ce0bfa0
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95734317"
---
# <a name="how-to-handle-multiple-events-using-event-properties"></a>如何：使用事件属性处理多个事件

若要使用事件属性，请在引发事件的类中定义事件属性，然后在处理事件的类中设置事件属性的委托。 若要在类中实现多个事件属性，此类必须在内部存储和维护为每个事件定义的委托。 典型的方法是实现通过事件键索引的委托集合。  
  
 若要存储每个事件的委托，则可以使用 <xref:System.ComponentModel.EventHandlerList> 类，或实现自己的集合。 集合类必须基于事件键提供用于设置、访问和检索事件处理程序委托的方法。 例如，可以使用 <xref:System.Collections.Hashtable> 类，或从 <xref:System.Collections.DictionaryBase> 类中派生一个自定义的类。 委托集合的实现细节不需要在你的类的外部公开。  
  
 类中的每个事件属性都定义了一个 add 访问器方法和一个 remove 访问器方法。 事件属性的 add 访问器将输入委托实例添加到委托集合。 事件属性的 remove 访问器将输入委托实例从委托集合中移除。 事件属性访问器使用的预定义的事件属性键来将实例添加到委托集合或从中将实例移除。  
  
### <a name="to-handle-multiple-events-using-event-properties"></a>使用事件属性处理多个事件  
  
1. 定义引发事件的类中的委托集合。  
  
2. 定义每个事件的键。  
  
3. 定义引发事件的类中的事件属性。  
  
4. 使用委托集合实现添加和移除事件属性的 add 和 remove 访问器方法。  
  
5. 使用公共事件属性来添加和移除处理事件的类中的事件处理程序委托。  
  
## <a name="example"></a>示例  

 下面的 C# 示例实现事件属性 `MouseDown` 和 `MouseUp`，其中使用 <xref:System.ComponentModel.EventHandlerList> 来存储每个事件的委托。 事件属性构造的关键字是粗体类型。  
  
 [!code-cpp[Conceptual.Events.Other#31](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.events.other/cpp/example3.cpp#31)]
 [!code-csharp[Conceptual.Events.Other#31](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.events.other/cs/example3.cs#31)]
 [!code-vb[Conceptual.Events.Other#31](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.events.other/vb/example3.vb#31)]  
  
## <a name="see-also"></a>请参阅

- <xref:System.ComponentModel.EventHandlerList?displayProperty=nameWithType>
- [事件](index.md)
- <xref:System.Web.UI.Control.Events%2A?displayProperty=nameWithType>
- [如何：声明自定义事件以节省内存](../../visual-basic/programming-guide/language-features/events/how-to-declare-custom-events-to-conserve-memory.md)
