---
title: 如何：实现提供程序
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- observer design pattern [.NET], implementing providers
- providers [.NET], in observer design pattern
- observables [.NET], in observer design pattern
ms.assetid: 790b5d8b-d546-40a6-beeb-151b574e5ee5
ms.openlocfilehash: b63666a581959f7a6c6a30ca8763f9c22067f32a
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95734330"
---
# <a name="how-to-implement-a-provider"></a>如何：实现提供程序

观察程序设计模式要求区分提供程序（监视数据并发送通知）和一个或多个观察程序（通过提供程序接收通知（回调））。 本主题介绍了如何创建提供程序。 相关主题[如何：实现观察程序](how-to-implement-an-observer.md)介绍了如何创建观察程序。  
  
### <a name="to-create-a-provider"></a>创建提供程序的具体步骤  
  
1. 定义提供程序负责发送给观察程序的数据。 尽管提供程序和发送给观察程序的数据可能是一种类型，但它们通常由不同类型表示。 例如，在温度监视应用中，`Temperature` 结构定义提供程序（由下一步中定义的 `TemperatureMonitor` 类表示）监视和观察程序订阅的数据。  
  
     [!code-csharp[Conceptual.ObserverDesign.HowTo#1](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.observerdesign.howto/cs/data.cs#1)]
     [!code-vb[Conceptual.ObserverDesign.HowTo#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.observerdesign.howto/vb/data.vb#1)]  
  
2. 定义数据提供程序，即实现 <xref:System.IObservable%601?displayProperty=nameWithType> 接口的类型。 提供程序的泛型类型参数是提供程序发送给观察程序的类型。 下面的示例定义了 `TemperatureMonitor` 类，即使用 `Temperature` 泛型类型参数的构造 <xref:System.IObservable%601?displayProperty=nameWithType> 实现。  
  
     [!code-csharp[Conceptual.ObserverDesign.HowTo#2](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.observerdesign.howto/cs/provider.cs#2)]
     [!code-vb[Conceptual.ObserverDesign.HowTo#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.observerdesign.howto/vb/provider.vb#2)]  
  
3. 确定提供程序如何存储对观察程序的引用，以便视情况通知每个观察程序。 最常见的情况是，集合对象（如泛型 <xref:System.Collections.Generic.List%601> 对象）用于实现此目的。 下面的示例定义了在 `TemperatureMonitor` 类构造函数中实例化的专用 <xref:System.Collections.Generic.List%601> 对象。  
  
     [!code-csharp[Conceptual.ObserverDesign.HowTo#3](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.observerdesign.howto/cs/provider.cs#3)]
     [!code-vb[Conceptual.ObserverDesign.HowTo#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.observerdesign.howto/vb/provider.vb#3)]  
  
4. 定义提供程序可以返回给订阅者的 <xref:System.IDisposable> 实现，以便订阅者能够随时停止接收通知。 下面的示例定义了嵌套类 `Unsubscriber`，在类实例化时向其中传递对订阅者集合和订阅者的引用。 使用此代码，订阅者可以调用对象的 <xref:System.IDisposable.Dispose%2A?displayProperty=nameWithType> 实现，将自身从订阅者集合中删除。  
  
     [!code-csharp[Conceptual.ObserverDesign.HowTo#4](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.observerdesign.howto/cs/provider.cs#4)]
     [!code-vb[Conceptual.ObserverDesign.HowTo#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.observerdesign.howto/vb/provider.vb#4)]  
  
5. 实现 <xref:System.IObservable%601.Subscribe%2A?displayProperty=nameWithType> 方法。 向此方法传递对 <xref:System.IObserver%601?displayProperty=nameWithType> 接口的引用，此方法应存储在第 3 步中为实现相应目的而设计的对象中。 然后，此方法应返回在第 4 步中开发的 <xref:System.IDisposable> 实现。 下面的示例展示了 `TemperatureMonitor` 类中 <xref:System.IObservable%601.Subscribe%2A> 方法的实现。  
  
     [!code-csharp[Conceptual.ObserverDesign.HowTo#5](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.observerdesign.howto/cs/provider.cs#5)]
     [!code-vb[Conceptual.ObserverDesign.HowTo#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.observerdesign.howto/vb/provider.vb#5)]  
  
6. 通过调用 <xref:System.IObserver%601.OnNext%2A?displayProperty=nameWithType>、<xref:System.IObserver%601.OnError%2A?displayProperty=nameWithType> 和 <xref:System.IObserver%601.OnCompleted%2A?displayProperty=nameWithType> 实现，视情况通知观察程序。 在某些情况下，如果出错，提供程序可能不会调用 <xref:System.IObserver%601.OnError%2A> 方法。 例如，下面的 `GetTemperature` 方法模拟监视器，每 5 秒读取一次温度数据，如果温度自从上一次读数以来出现至少 0.1 度的变化，就会通知观察程序。 如果设备不报告温度（即如果值为 null），提供程序便会通知观察程序传输已完成。 请注意，除了调用每个观察程序的 <xref:System.IObserver%601.OnCompleted%2A> 方法外，`GetTemperature` 方法还可以清除 <xref:System.Collections.Generic.List%601> 集合。 在这种情况下，提供程序不调用观察程序的 <xref:System.IObserver%601.OnError%2A> 方法。  
  
     [!code-csharp[Conceptual.ObserverDesign.HowTo#6](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.observerdesign.howto/cs/provider.cs#6)]
     [!code-vb[Conceptual.ObserverDesign.HowTo#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.observerdesign.howto/vb/provider.vb#6)]  
  
## <a name="example"></a>示例  

 下面的示例包含完整的源代码，用于定义温度监视应用的 <xref:System.IObservable%601> 实现。 它包括 `Temperature` 结构（即发送给观察程序的数据）和 `TemperatureMonitor` 类（即 <xref:System.IObservable%601> 实现）。  
  
 [!code-csharp[Conceptual.ObserverDesign.HowTo#7](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.observerdesign.howto/cs/provider.cs#7)]
 [!code-vb[Conceptual.ObserverDesign.HowTo#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.observerdesign.howto/vb/provider.vb#7)]  
  
## <a name="see-also"></a>另请参阅

- <xref:System.IObservable%601>
- [观察程序设计模式](observer-design-pattern.md)
- [如何：实现监视程序](how-to-implement-an-observer.md)
- [监视程序设计模式最佳做法](observer-design-pattern-best-practices.md)
