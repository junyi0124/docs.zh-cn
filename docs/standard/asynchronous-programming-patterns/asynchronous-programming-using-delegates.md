---
title: 使用委托进行异步编程
ms.date: 03/30/2017
helpviewer_keywords:
- BeginInvoke method
- asynchronous programming, delegates
- asynchronous delegates
- calling synchronous methods in asynchronous manner
- EndInvoke method
- calling asynchronous methods
- delegates [.NET], asynchronous
- synchronous calling in asynchronous manner
ms.assetid: 38a345ca-6963-4436-9608-5c9defef9c64
ms.openlocfilehash: 01cdf5acf8f64472c218f35a0b8095aebf8f4571
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95716260"
---
# <a name="asynchronous-programming-using-delegates"></a>使用委托进行异步编程

使用委托可通过异步方式调用同步方法。 如果同步调用委托，`Invoke` 方法将在当前线程上直接调用目标方法。 如果调用 `BeginInvoke` 方法，公共语言运行时 (CLR) 将对请求进行排队并立即返回给调用方。 目标方法将在线程池中的某个线程上异步调用。 提交请求的原始线程可以不受限制地继续与目标方法并行执行。 如果已在对 `BeginInvoke` 方法的调用中指定回叫方法，则目标方法结束时，将调用回叫方法。 在回叫方法中，`EndInvoke` 方法将获取返回值和所有输入/输出或仅输出参数。 如果调用 `BeginInvoke` 时未指定回叫方法，则可能从调用 `BeginInvoke` 的线程上调用 `EndInvoke`。  
  
> [!IMPORTANT]
> 编译器应使用由用户指定的委托签名，发出具有 `Invoke`、`BeginInvoke` 和 `EndInvoke` 方法的委托类。 `BeginInvoke` 和 `EndInvoke` 方法应标记为本机方法。 由于这些方法被标记为本机方法，CLR 将在类加载时自动提供实现。 加载程序可确保其不会被替代。  
  
## <a name="in-this-section"></a>本节内容  

 [使用异步方式调用同步方法](calling-synchronous-methods-asynchronously.md)  
 讨论如何使用委托对普通方法进行异步调用，并提供简单的代码示例演示等待异步调用返回的四种方法。  
  
## <a name="related-sections"></a>相关章节  

 [基于事件的异步模式 (EAP)](event-based-asynchronous-pattern-eap.md)  
 介绍 .NET 中的异步编程。  
  
## <a name="see-also"></a>另请参阅

- <xref:System.Delegate>
