---
title: 启动时创建线程并传递数据
description: 了解如何在 .NET 中的操作系统进程开始时创建线程并传递数据。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- threading [.NET], creating
- threading [.NET], passing data to threads
- threading [.NET], retrieving data from threads
ms.assetid: 52b32222-e185-4f42-91a7-eaca65c0ab6d
ms.openlocfilehash: 3f44eb9dabc9145cd0e6ec9bfd3d484c9079e803
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94819924"
---
# <a name="creating-threads-and-passing-data-at-start-time"></a>启动时创建线程并传递数据

在操作系统进程创建后，操作系统会注入线程，用于执行相应进程中的代码，包括所有原始应用域。 自此时起，可以创建和销毁应用域，而无需创建或销毁任何操作系统线程。 如果要执行的代码是托管代码，可以检索 <xref:System.Threading.Thread> 类型的静态 <xref:System.Threading.Thread.CurrentThread%2A> 属性，以获取在当前应用域中执行的线程的 <xref:System.Threading.Thread> 对象。 本主题介绍了如何创建线程，以及向线程过程传递数据的替换方法。  
  
## <a name="creating-a-thread"></a>创建线程

 新建 <xref:System.Threading.Thread> 对象会新建托管线程。 <xref:System.Threading.Thread> 类包含需要使用 <xref:System.Threading.ThreadStart> 委托或 <xref:System.Threading.ParameterizedThreadStart> 委托的构造函数；委托包装在调用 <xref:System.Threading.Thread.Start%2A> 方法时由新线程调用的方法。 多次调用 <xref:System.Threading.Thread.Start%2A> 会导致 <xref:System.Threading.ThreadStateException> 抛出。  
  
 通常情况下，在 <xref:System.Threading.Thread.Start%2A> 方法返回结果后，新线程其实紧接着启动。 可以使用 <xref:System.Threading.Thread.ThreadState%2A> 和 <xref:System.Threading.Thread.IsAlive%2A> 属性，以确定任意时刻的线程状态，但不得将这些属性用于同步线程活动。  
  
> [!NOTE]
> 线程一旦启动，就无需保留对 <xref:System.Threading.Thread> 对象的引用。 线程可以继续执行到线程过程结束。  
  
 下面的代码示例新建两个线程，以对另一个对象调用实例和静态方法。  
  
 [!code-cpp[System.Threading.ThreadStart2#2](../../../samples/snippets/cpp/VS_Snippets_CLR_System/system.Threading.ThreadStart2/CPP/source2.cpp#2)]
 [!code-csharp[System.Threading.ThreadStart2#2](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.Threading.ThreadStart2/CS/source2.cs#2)]
 [!code-vb[System.Threading.ThreadStart2#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.Threading.ThreadStart2/VB/source2.vb#2)]  
  
## <a name="passing-data-to-threads"></a>将数据传递到线程

调用 <xref:System.Threading.Thread.Start(System.Object)?displayProperty=nameWithType> 时，使用 <xref:System.Threading.ParameterizedThreadStart> 委托可将包含数据的对象轻松传递给线程。 有关代码示例，请参阅 <xref:System.Threading.ParameterizedThreadStart>。
  
 使用 <xref:System.Threading.ParameterizedThreadStart> 委托不是传递数据的类型安全方式，因为 <xref:System.Threading.Thread.Start(System.Object)?displayProperty=nameWithType> 方法接受任何对象。 替换方法是，将线程过程和数据封装到帮助程序类中，并使用 <xref:System.Threading.ThreadStart> 委托执行线程过程。 下面的示例演示这一方法：

 [!code-cpp[System.Threading.ThreadStart2#3](../../../samples/snippets/cpp/VS_Snippets_CLR_System/system.Threading.ThreadStart2/CPP/source3.cpp#3)]
 [!code-csharp[System.Threading.ThreadStart2#3](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.Threading.ThreadStart2/CS/source3.cs#3)]
 [!code-vb[System.Threading.ThreadStart2#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.Threading.ThreadStart2/VB/source3.vb#3)]  

<xref:System.Threading.ThreadStart> 和 <xref:System.Threading.ParameterizedThreadStart> 委托都没有返回值，因为没有从异步调用返回数据的位置。 若要检索线程方法的结果，可以使用回叫方法，如下一节所示。
  
## <a name="retrieving-data-from-threads-with-callback-methods"></a>使用回叫方法检索线程中的数据

 下面的示例展示了从线程检索数据的回调方法。 包含数据和线程方法的类构造函数还接受表示回调方法的委托；在线程方法结束前，它调用回调委托。  
  
 [!code-cpp[System.Threading.ThreadStart2#4](../../../samples/snippets/cpp/VS_Snippets_CLR_System/system.Threading.ThreadStart2/CPP/source4.cpp#4)]
 [!code-csharp[System.Threading.ThreadStart2#4](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.Threading.ThreadStart2/CS/source4.cs#4)]
 [!code-vb[System.Threading.ThreadStart2#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.Threading.ThreadStart2/VB/source4.vb#4)]  
  
## <a name="see-also"></a>请参阅

- <xref:System.Threading.Thread>
- <xref:System.Threading.ThreadStart>
- <xref:System.Threading.ParameterizedThreadStart>
- <xref:System.Threading.Thread.Start%2A?displayProperty=nameWithType>
- [线程处理](index.md)
- [使用线程和线程处理](using-threads-and-threading.md)
