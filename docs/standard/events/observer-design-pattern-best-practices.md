---
title: 观察程序设计模式最佳做法
ms.date: 03/30/2017
helpviewer_keywords:
- observer design pattern [.NET], best practices
- best practices [.NET], observer design pattern
ms.assetid: c834760f-ddd4-417f-abb7-a059679d5b8c
ms.openlocfilehash: d88b092e681258566da685e3bb130b3d6131be08
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95697579"
---
# <a name="observer-design-pattern-best-practices"></a>观察程序设计模式最佳做法

在 .NET 中，将观察者设计模式作为一组接口实现。 <xref:System.IObservable%601?displayProperty=nameWithType> 接口表示数据提供程序，也负责提供允许观察者取消订阅通知的 <xref:System.IDisposable> 实现。 <xref:System.IObserver%601?displayProperty=nameWithType> 接口表示观察者。 本主题描述使用这些接口实现观察者设计模式时开发人员应遵循的最佳做法。  
  
## <a name="threading"></a>线程  

 提供程序通常通过向由一些集合对象表示的订阅者列表添加特定观察者来实现 <xref:System.IObservable%601.Subscribe%2A?displayProperty=nameWithType> 方法，并通过从订阅者列表中删除特定观察者来实现 <xref:System.IDisposable.Dispose%2A?displayProperty=nameWithType> 方法。 观察者可在任何时候调用这些方法。 此外，由于提供程序/观察者协定未指定由谁负责在 <xref:System.IObserver%601.OnCompleted%2A?displayProperty=nameWithType> 回调方法后取消订阅，因此提供程序和观察者都可能尝试从列表中删除相同成员。 由于这种可能性，<xref:System.IObservable%601.Subscribe%2A> 和 <xref:System.IDisposable.Dispose%2A> 方法都应该是线程安全的。 这通常需要使用[并发回收](../parallel-programming/data-structures-for-parallel-programming.md)或锁。 非线程安全的实现应显式注明它们非线程安全。  
  
 任何其他保证均须在提供程序/观察者协定之上指定。 实施者应清楚地调出何时施加其他需求，从而避免用户对观察者协定产生混淆。  
  
## <a name="handling-exceptions"></a>处理异常  

 由于数据提供程序和观察者之间的松耦合，观察者设计模式中的异常旨在提供信息。 这会影响提供程序和观察者处理观察者设计模式中的异常的方式。  
  
### <a name="the-provider----calling-the-onerror-method"></a>提供者 -- 调用 OnError 方法  

 <xref:System.IObserver%601.OnError%2A> 方法提供旨在给观察者提供信息性消息，正如 <xref:System.IObserver%601.OnNext%2A?displayProperty=nameWithType> 方法一样。 然而，<xref:System.IObserver%601.OnNext%2A> 方法旨在为观察者提供当前数据或已更新数据，而 <xref:System.IObserver%601.OnError%2A> 方法旨在表明该提供程序不能提供有效的数据。  
  
 在处理异常和调用 <xref:System.IObserver%601.OnError%2A> 方法时，提供程序应遵循以下最佳做法并：  
  
- 如果提供程序有任何具体需求，则它必须处理自己的异常。  
  
- 提供程序不应期望或要求观察者以任何特定方式处理异常。  
  
- 提供程序应在处理削弱其提供更新的能力的异常时调用 <xref:System.IObserver%601.OnError%2A> 方法。 可将有关这种异常的信息传递给观察者。 在其他情况下，则无需通知观察器有异常。  
  
 只要提供程序调用了 <xref:System.IObserver%601.OnError%2A> 或 <xref:System.IObserver%601.OnCompleted%2A?displayProperty=nameWithType> 方法，就不应有进一步的通知，并且提供程序可以取消订阅其观察者。 然而，观察者也可以在任何时候取消订阅他们自己，包括在他们收到 <xref:System.IObserver%601.OnError%2A> 或<xref:System.IObserver%601.OnCompleted%2A?displayProperty=nameWithType> 通知前后。 观察者设计模式并未规定负责取消订阅的提供程序还是观察者；因此，可能这两者都试图取消订阅。 通常情况下，当观察者取消订阅时，会从订阅者集合中把他们删除。 在单线程应用程序中，<xref:System.IDisposable.Dispose%2A?displayProperty=nameWithType> 实现应确保对象引用有效，并在尝试删除对象前确保该对象是订阅者集合的成员。 在多线程应用程序中，应使用诸如 <xref:System.Collections.Concurrent.BlockingCollection%601?displayProperty=nameWithType> 对象的线程安全集合对象。  
  
### <a name="the-observer----implementing-the-onerror-method"></a>观察者 -- 实现 OnError 方法  

 当观察者收到来自提供程序的错误通知时，应将异常视为信息，且不需采取任何特殊操作。  
  
 在回应从提供程序调用的 <xref:System.IObserver%601.OnError%2A> 方法时，观察者应遵循以下最佳做法：  
  
- 观察者不应从其接口实现引发异常，如 <xref:System.IObserver%601.OnNext%2A> 或 <xref:System.IObserver%601.OnError%2A>。 但如果观察者确实引发了异常，应预计到这些异常不会得到处理。  
  
- 若要保留调用堆栈，希望引发传递到 <xref:System.Exception> 方法的 <xref:System.IObserver%601.OnError%2A> 对象的观察者应在引发之前包装该异常。 为此，应使用标准异常对象。  
  
## <a name="additional-best-practices"></a>其他最佳做法  

 尝试在 <xref:System.IObservable%601.Subscribe%2A?displayProperty=nameWithType> 方法中取消注册可能会导致空引用。 因此，我们建议你避免这种做法。  
  
 虽然可将观察者附加到多个提供程序到，建议的模式是将 <xref:System.IObserver%601> 实例附加到唯一的 <xref:System.IObservable%601> 实例。  
  
## <a name="see-also"></a>另请参阅

- [观察程序设计模式](observer-design-pattern.md)
- [如何：实现监视程序](how-to-implement-an-observer.md)
- [如何：实现提供程序](how-to-implement-a-provider.md)
