---
title: 版本 3.5 中的套接字性能增强
description: 了解 .NET Framework 版本 3.5 中的 System.Net.Sockets.Socket 类的性能改进。
ms.date: 03/30/2017
ms.assetid: 225aa5f9-c54b-4620-ab64-5cd100cfd54c
ms.openlocfilehash: 5bd7c97d6a6edd5f914d6fe3118b6d81b64544e0
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96263134"
---
# <a name="socket-performance-enhancements-in-version-35"></a>版本 3.5 中的套接字性能增强

版本 3.5 中增强了 <xref:System.Net.Sockets.Socket?displayProperty=nameWithType> 类，以供使用异步网络 I/O 实现最高性能的应用程序使用。 已添加了一系列新类，作为 <xref:System.Net.Sockets.Socket> 类的一组增强功能的一部分，这些增强功能提供了一种可供专用高性能套接字应用程序使用的替代异步模式。 这些增强功能专为需要高性能的网络服务器应用程序而设计。 应用程序可以独占方式使用增强型异步模式，或仅在其应用程序的目标热区域（例如，接收大量数据时）使用。  
  
## <a name="class-enhancements"></a>类增强功能  

 这些增强功能的主要功能是避免在大容量异步套接字 I/O 期间重复分配和同步对象。 当前由异步套接字 I/O 的 <xref:System.Net.Sockets.Socket> 类实现的 Begin/End 设计模式需要为每个异步套接字操作分配一个 <xref:System.IAsyncResult?displayProperty=nameWithType> 对象。  
  
 在新的 <xref:System.Net.Sockets.Socket> 类增强功能中，异步套接字操作由应用程序分配和维护的可重用 <xref:System.Net.Sockets.SocketAsyncEventArgs?displayProperty=nameWithType> 类对象描述。 高性能套接字应用程序非常清楚必须维持的重叠套接字操作的数量。 该应用程序可创建所需的 <xref:System.Net.Sockets.SocketAsyncEventArgs> 对象数量。 例如，如果服务器应用程序始终需要 15 个套接字接受操作，以支持传入的客户端连接速率，则可以预先为此分配 15 个可重用的 <xref:System.Net.Sockets.SocketAsyncEventArgs> 对象。  
  
 使用此类执行异步套接字操作的模式包括以下步骤：  
  
1. 分配一个新的 <xref:System.Net.Sockets.SocketAsyncEventArgs> 上下文对象，或从应用程序池中获取一个空闲对象。  
  
2. 将上下文对象的属性设置为要执行的操作（例如，回调代理方法和数据缓冲区）。  
  
3. 调用适当的套接字方法 (xxxAsync) 以启动异步操作。  
  
4. 如果异步套接字方法 (xxxAsync) 在回调中返回 true，请查询完成状态的上下文属性。  
  
5. 如果异步套接字方法 (xxxAsync) 在回调中返回 false，则已同步完成该操作。 可查询上下文属性获取操作结果。  
  
6. 重新使用上下文进行另一项操作，将其放回池中，或放弃它。  
  
 新的异步套接字操作上下文对象的生存期由应用程序代码中的引用和异步 I/O 引用确定。 作为参数提交给异步套接字操作方法之一后，应用程序不必保留对异步套接字操作上下文对象的引用。 完成回调返回之前，应用程序会继续引用它。 然而，应用程序保留对上下文对象的引用是有利的，这样可将其重新用于将来的异步套接字操作。  
  
## <a name="see-also"></a>请参阅

- <xref:System.Net.Sockets.Socket?displayProperty=nameWithType>
- <xref:System.Net.Sockets.SendPacketsElement?displayProperty=nameWithType>
- <xref:System.Net.Sockets.SocketAsyncEventArgs?displayProperty=nameWithType>
- <xref:System.Net.Sockets.SocketAsyncOperation?displayProperty=nameWithType>
- [网络编程示例](network-programming-samples.md)
- [Socket 代码示例](socket-code-examples.md)
