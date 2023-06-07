---
title: 如何：实现监视程序
description: 在 .NET 中实现监视程序。 观察程序设计模式要求区分观察程序（注册获取通知）和提供程序。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- observers [.NET], observer design pattern
- observer design pattern [.NET], implementing observers
ms.assetid: 8ecfa9f5-b500-473d-bcf0-5652ffb1e53d
ms.openlocfilehash: 73f6d2c25c727717394c79268b71b44ea9ff7800
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95697475"
---
# <a name="how-to-implement-an-observer"></a>如何：实现监视程序

观察程序设计模式要求区分观察程序（注册获取通知）和提供程序（监视数据并将通知发送到一个或多个观察程序）。 本主题介绍了如何创建观察程序。 相关主题[如何：实现提供程序](how-to-implement-a-provider.md)介绍了如何创建提供程序。  
  
### <a name="to-create-an-observer"></a>创建观察程序的具体步骤  
  
1. 定义观察程序，即实现 <xref:System.IObserver%601?displayProperty=nameWithType> 接口的类型。 例如，下面的代码定义了 `TemperatureReporter` 类型，即使用 `Temperature` 泛型类型参数的构造 <xref:System.IObserver%601?displayProperty=nameWithType> 实现。  
  
     [!code-csharp[Conceptual.ObserverDesign.HowTo#8](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.observerdesign.howto/cs/observer.cs#8)]
     [!code-vb[Conceptual.ObserverDesign.HowTo#8](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.observerdesign.howto/vb/observer.vb#8)]  
  
2. 如果观察程序可以在提供程序调用 <xref:System.IObserver%601.OnCompleted%2A?displayProperty=nameWithType> 实现前停止接收通知，请定义专用变量，用于保留提供程序 <xref:System.IObservable%601.Subscribe%2A?displayProperty=nameWithType> 方法返回的 <xref:System.IDisposable> 实现。 还应定义订阅方法，用于调用提供程序的 <xref:System.IObservable%601.Subscribe%2A> 方法，并存储返回的 <xref:System.IDisposable> 对象。 例如，下面的代码定义 `unsubscriber` 专用变量，并定义 `Subscribe` 方法，用于调用提供程序的 <xref:System.IObservable%601.Subscribe%2A> 方法，并将返回的对象分配给 `unsubscriber` 变量。  
  
     [!code-csharp[Conceptual.ObserverDesign.HowTo#9](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.observerdesign.howto/cs/observer.cs#9)]
     [!code-vb[Conceptual.ObserverDesign.HowTo#9](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.observerdesign.howto/vb/observer.vb#9)]  
  
3. 定义一个方法，以便观察程序可以在提供程序调用 <xref:System.IObserver%601.OnCompleted%2A?displayProperty=nameWithType> 实现前停止接收通知（如果需要此功能的话）。 下面的示例定义了 `Unsubscribe` 方法。  
  
     [!code-csharp[Conceptual.ObserverDesign.HowTo#10](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.observerdesign.howto/cs/observer.cs#10)]
     [!code-vb[Conceptual.ObserverDesign.HowTo#10](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.observerdesign.howto/vb/observer.vb#10)]  
  
4. 实现 <xref:System.IObserver%601> 接口定义的三个方法：<xref:System.IObserver%601.OnNext%2A?displayProperty=nameWithType>、<xref:System.IObserver%601.OnError%2A?displayProperty=nameWithType> 和 <xref:System.IObserver%601.OnCompleted%2A?displayProperty=nameWithType>。 根据提供程序和应用需求，<xref:System.IObserver%601.OnError%2A> 和 <xref:System.IObserver%601.OnCompleted%2A> 方法可以是存根实现。 请注意，<xref:System.IObserver%601.OnError%2A> 方法不得将传入的 <xref:System.Exception> 对象处理为异常，<xref:System.IObserver%601.OnCompleted%2A> 方法可以随意调用提供程序的 <xref:System.IDisposable.Dispose%2A?displayProperty=nameWithType> 实现。 下面的示例展示了 `TemperatureReporter` 类的 <xref:System.IObserver%601> 实现。  
  
     [!code-csharp[Conceptual.ObserverDesign.HowTo#11](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.observerdesign.howto/cs/observer.cs#11)]
     [!code-vb[Conceptual.ObserverDesign.HowTo#11](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.observerdesign.howto/vb/observer.vb#11)]  
  
## <a name="example"></a>示例  

 下面的示例包含 `TemperatureReporter` 类的完整源代码，提供了温度监视应用的 <xref:System.IObserver%601> 实现。  
  
 [!code-csharp[Conceptual.ObserverDesign.HowTo#12](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.observerdesign.howto/cs/observer.cs#12)]
 [!code-vb[Conceptual.ObserverDesign.HowTo#12](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.observerdesign.howto/vb/observer.vb#12)]  
  
## <a name="see-also"></a>请参阅

- <xref:System.IObserver%601>
- [观察程序设计模式](observer-design-pattern.md)
- [如何：实现提供程序](how-to-implement-a-provider.md)
- [监视程序设计模式最佳做法](observer-design-pattern-best-practices.md)
