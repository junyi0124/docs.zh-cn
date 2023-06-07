---
title: 观察者设计模式
description: 了解 .NET 中的观察程序设计模式。 此模式使订阅者能够从提供程序注册并接收通知。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- IObserver(Of T) interface
- IObservable<T> interface
- IObserver<T> interface
- IObservable(Of T) interface
- observer design pattern [.NET]
ms.assetid: 3680171f-f522-453c-aa4a-54f755a78f88
ms.openlocfilehash: 1fda2b692d7bbaeedcf881801eb224f114da41e8
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94828239"
---
# <a name="observer-design-pattern"></a>观察者设计模式

观察者设计模式使订阅者能够从提供程序注册并接收通知。 它适用于需要基于推送的通知的任何方案。 此模式定义提供程序（亦称为“使用者”或“可观察对象”），以及零个、一个或多个观察者。    观察者注册提供程序，并且每当预定义的条件、事件或状态发生更改时，该提供程序会通过调用其方法之一来自动通知所有观察者。 在此方法调用中，该提供程序还可向观察者提供当前状态信息。 在 .NET 中，通过实现泛型 <xref:System.IObservable%601?displayProperty=nameWithType> 和 <xref:System.IObserver%601?displayProperty=nameWithType> 接口来应用观察者设计模式。 泛型类型参数表示提供通知信息的类型。

## <a name="applying-the-pattern"></a>应用模式

观察者设计模式适用于分布式基于推送的通知，因为它支持两个不同的组件或应用程序层（如数据源（业务逻辑）层和用户界面（显示）层）之间完全分离。 每当提供程序使用回调向其客户端提供当前信息时，均可以实现此模式。

实现此模式要求提供以下内容：

- 提供程序或主题，即将通知发送给观察者的对象。 提供程序是实现 <xref:System.IObservable%601> 接口的类或结构。 提供程序必须实现单个方法 <xref:System.IObservable%601.Subscribe%2A?displayProperty=nameWithType>，该方法由希望从提供程序接收通知的观察者调用。

- 观察者，即从提供程序接收通知的对象。 观察者是实现 <xref:System.IObserver%601> 接口的类或结构。 观察者必须实现以下三个方法，这三个方法均由提供程序调用：

  - <xref:System.IObserver%601.OnNext%2A?displayProperty=nameWithType>，它向观察者提供新信息或当前信息。

  - <xref:System.IObserver%601.OnError%2A?displayProperty=nameWithType>，它通知观察者已发生错误。

  - <xref:System.IObserver%601.OnCompleted%2A?displayProperty=nameWithType>，它指示提供程序已完成发送通知。

- 允许提供程序跟踪观察者的一种机制。 通常情况下，提供程序使用容器对象（如 <xref:System.Collections.Generic.List%601?displayProperty=nameWithType> 对象）来保存对已订阅通知的 <xref:System.IObserver%601> 实现的引用。 将存储容器用于此目的使提供程序能够处理零到无限数量的观察者。 未定义观察者接收通知的顺序；提供程序可以随意使用任何方法来确定顺序。

- <xref:System.IDisposable> 实现，它使提供程序在能够通知完成时删除观察者。 观察者从 <xref:System.IObservable%601.Subscribe%2A> 方法接收对 <xref:System.IDisposable> 实现的引用，因此它们还可以调用 <xref:System.IDisposable.Dispose%2A?displayProperty=nameWithType> 方法，以便在提供程序已完成发送通知之前取消订阅。

- 包含提供程序发送到其观察者的数据的对象。 此对象的类型对应 <xref:System.IObservable%601> 和 <xref:System.IObserver%601> 接口的泛型类型参数。 尽管此对象可与 <xref:System.IObservable%601> 实现相同，但通常情况下，它是一个单独的类型。

> [!NOTE]
> 除实现观察者设计模式外，你还可能对浏览使用 <xref:System.IObservable%601> 和 <xref:System.IObserver%601> 接口构建的库感兴趣。 例如，[Reactive Extensions for .NET (Rx)](/previous-versions/dotnet/reactive-extensions/hh242985(v=vs.103)) 包含一组支持异步编程的扩展方法和 LINQ 标准序列运算符。

## <a name="implementing-the-pattern"></a>实现模式

下面的示例使用观察者设计模式来实现机场行李认领信息系统。 `BaggageInfo` 类提供有关到达航班以及可领取每次航班行李的行李传送带的信息。 如以下示例所示。

[!code-csharp[Conceptual.ObserverDesignPattern#1](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.observerdesignpattern/cs/provider.cs#1)]
[!code-vb[Conceptual.ObserverDesignPattern#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.observerdesignpattern/vb/provider.vb#1)]

`BaggageHandler` 类负责接收有关到达航班和行李认领传送带的信息。 在内部，它维护两个集合：

- `observers` - 将接收更新的信息的客户端集合。

- `flights` - 航班及其指定行李传送带的集合。

这两个集合都由在 `BaggageHandler` 类构造函数中实例化的泛型 <xref:System.Collections.Generic.List%601> 对象表示。 下面的示例演示了 `BaggageHandler` 类的源代码。

[!code-csharp[Conceptual.ObserverDesignPattern#2](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.observerdesignpattern/cs/provider.cs#2)]
[!code-vb[Conceptual.ObserverDesignPattern#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.observerdesignpattern/vb/provider.vb#2)]

想要接收更新的信息的客户端调用 `BaggageHandler.Subscribe` 方法。 如果客户端以前未订阅通知，则将对客户端的 <xref:System.IObserver%601> 实现的引用添加到 `observers` 集合中。

可调用重载的 `BaggageHandler.BaggageStatus` 方法来指示是正在卸载航班行李还是不再卸载航班行李。 在第一种情况下，该方法传递航班号、航班起飞机场和正在卸载行李的传送带。 在第二种情况下，该方法仅传递航班号。 对于正在卸载的行李，该方法检查传递到方法的 `BaggageInfo` 信息是否存在于 `flights` 集合中。 如果不存在，该方法将添加信息，并调用每个观察者的 `OnNext` 方法。 对于不再卸载其行李的航班，该方法检查此航班的相关信息是否存储在 `flights` 集合中。 如果是，则该方法调用每个观察者的 `OnNext` 方法，并从 `flights` 集合中删除 `BaggageInfo` 对象。

当一天中的最后一个航班已着陆并且已处理了其行李时，调用 `BaggageHandler.LastBaggageClaimed` 方法。 此方法调用每个观察者的 `OnCompleted` 方法来指示所有通知均已完成，然后再清除 `observers` 集合。

提供程序的 <xref:System.IObservable%601.Subscribe%2A> 方法将返回 <xref:System.IDisposable> 实现，该实现可使观察者在调用 <xref:System.IObserver%601.OnCompleted%2A> 方法之前停止接收通知。 下面的示例演示了 `Unsubscriber(Of BaggageInfo)` 类的源代码。 此类在 `BaggageHandler.Subscribe` 方法中实例化时，它将传递对 `observers` 集合的引用以及对添加到集合中的观察者的引用。 这些引用被分配给局部变量。 调用对象的 `Dispose` 方法时，它会检查观察者是否仍存在于 `observers` 集合中，如果存在，则删除观察者。

[!code-csharp[Conceptual.ObserverDesignPattern#3](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.observerdesignpattern/cs/provider.cs#3)]
[!code-vb[Conceptual.ObserverDesignPattern#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.observerdesignpattern/vb/provider.vb#3)]

下面的示例提供了名为 `ArrivalsMonitor` 的 <xref:System.IObserver%601> 实现，它是一个显示行李认领信息的基类。 根据始发城市的名称，按字母顺序显示信息。 将 `ArrivalsMonitor` 的方法标记为 `overridable`（在 Visual Basic 中）或 `virtual`（在 C# 中），因此它们都可由派生类重写。

[!code-csharp[Conceptual.ObserverDesignPattern#4](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.observerdesignpattern/cs/observer.cs#4)]
[!code-vb[Conceptual.ObserverDesignPattern#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.observerdesignpattern/vb/observer.vb#4)]

`ArrivalsMonitor` 类包括 `Subscribe` 和 `Unsubscribe` 方法。 `Subscribe` 方法使类可将由对 <xref:System.IObservable%601.Subscribe%2A> 的调用返回的 <xref:System.IDisposable> 实现保存到私有变量中。 `Unsubscribe` 方法使类可以通过调用提供程序的 <xref:System.IDisposable.Dispose%2A> 实现来取消订阅通知。 `ArrivalsMonitor` 也提供 <xref:System.IObserver%601.OnNext%2A>、<xref:System.IObserver%601.OnError%2A> 和 <xref:System.IObserver%601.OnCompleted%2A> 方法的实现。 仅 <xref:System.IObserver%601.OnNext%2A> 实现包含大量的代码。 该方法处理私有的、已排序的泛型 <xref:System.Collections.Generic.List%601> 对象，该对象维护有关抵港航班的始发机场以及可提取行李的传送带的信息。 如果 `BaggageHandler` 类报告新的航班抵达，则 <xref:System.IObserver%601.OnNext%2A> 方法实现将该航班的相关信息添加到列表中。 如果 `BaggageHandler` 类报告已卸载该航班的行李，则 <xref:System.IObserver%601.OnNext%2A> 方法从列表中移除该航班。 每当进行了更改，就会对列表进行排序并向控制台显示。

下面的示例包含对 `BaggageHandler` 类以及对 `ArrivalsMonitor` 类的两个实例进行实例化的应用程序入口点，并使用 `BaggageHandler.BaggageStatus` 方法来添加和删除有关抵港航班的信息。 在每种情况下，观察者均接收更新，并且正确显示行李认领信息。

[!code-csharp[Conceptual.ObserverDesignPattern#5](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.observerdesignpattern/cs/program.cs#5)]
[!code-vb[Conceptual.ObserverDesignPattern#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.observerdesignpattern/vb/module1.vb#5)]

## <a name="related-topics"></a>相关主题

|Title|描述|
|-----------|-----------------|
|[监视程序设计模式最佳做法](observer-design-pattern-best-practices.md)|描述开发实现观察者设计模式的应用程序时要采用的最佳做法。|
|[如何：实现提供程序](how-to-implement-a-provider.md)|为温度监控应用程序提供一个提供程序的分步实现。|
|[如何：实现监视程序](how-to-implement-an-observer.md)|为温度监控应用程序提供一个观察者的分步实现。|
