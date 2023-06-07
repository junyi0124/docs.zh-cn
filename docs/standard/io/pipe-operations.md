---
title: .NET 中的管道操作
description: 了解 .NET 中的管道操作。 管道为进程间通信提供了平台。 有两种类型的管道：匿名管道和命名管道。
ms.date: 03/30/2017
helpviewer_keywords:
- pipes [.NET]
- pipe operations [.NET]
- interprocess communication [.NET], pipes
- I/O [.NET], pipes
ms.assetid: 7b964ebd-7a4f-4d28-8194-7841f9e4c702
ms.openlocfilehash: bb8804c32b9f2b54b05298779bddae117c10dcf5
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95734798"
---
# <a name="pipe-operations-in-net"></a>.NET 中的管道操作

管道为进程间通信提供了平台。 管道分为两种类型：  
  
- 匿名管道。  
  
     匿名管道在本地计算机上提供进程间通信。 与命名管道相比，虽然匿名管道需要的开销更少，但提供的服务有限。 匿名管道是单向的，不能通过网络使用。 仅支持一个服务器实例。 匿名管道可用于线程间通信，也可用于父进程和子进程之间的通信，因为管道句柄可以轻松传递给所创建的子进程。  
  
     在 .NET 中，可通过使用 <xref:System.IO.Pipes.AnonymousPipeServerStream> 和 <xref:System.IO.Pipes.AnonymousPipeClientStream> 类来实现匿名管道。  
  
     请参阅[如何：使用匿名管道进行本地进程间通信](how-to-use-anonymous-pipes-for-local-interprocess-communication.md)。  
  
- 命名管道。  
  
     命名管道在管道服务器和一个或多个管道客户端之间提供进程间通信。 命名管道可以是单向的，也可以是双向的。 它们支持基于消息的通信，并允许多个客户端使用相同的管道名称同时连接到服务器进程。 命名管道还支持模拟，这样连接进程就可以在远程服务器上使用自己的权限。  
  
     在 .NET 中，可通过使用 <xref:System.IO.Pipes.NamedPipeServerStream> 和 <xref:System.IO.Pipes.NamedPipeClientStream> 类来实现命名管道。  
  
     请参阅[如何：使用命名管道进行网络进程间通信](how-to-use-named-pipes-for-network-interprocess-communication.md)。  
  
## <a name="see-also"></a>请参阅

- [文件和流 I/O](index.md)
- [如何：使用匿名管道进行本地进程间通信](how-to-use-anonymous-pipes-for-local-interprocess-communication.md)
- [如何：使用命名管道进行网络进程间通信](how-to-use-named-pipes-for-network-interprocess-communication.md)
