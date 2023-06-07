---
title: 如何：使用命名管道进行网络进程间通信
description: 查看两个示例，了解如何使用命名管道在网络中的管道服务器和一个或多个管道客户端之间提供进程间通信。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- message-based communication [.NET], named pipes
- named pipes [.NET]
- pipes [.NET]
- multiple connections via named pipes
- network communications [.NET], named pipes
- impersonation [.NET], named pipes
- full duplex communication [.NET], named pipes
ms.assetid: 4e4d7e64-9f1b-4026-98f7-20488ac7b42b
ms.openlocfilehash: 421fe06ce24fe8d78c7f8306db6a32ae83da694a
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95734538"
---
# <a name="how-to-use-named-pipes-for-network-interprocess-communication"></a>如何：使用命名管道进行网络进程间通信

命名管道在管道服务器和一个或多个管道客户端之间提供进程间通信。 它们比匿名管道（用于在本地计算机上提供进程间的通信）提供更多的功能。 命名管道支持跨网络和多个服务器实例的全双工通信、基于消息的通信以及客户端模拟，这样连接进程便可在远程服务器上使用自己的权限集。  
  
 若要实现名称管道，请使用 <xref:System.IO.Pipes.NamedPipeServerStream> 和 <xref:System.IO.Pipes.NamedPipeClientStream> 类。  
  
## <a name="example"></a>示例  

 下面的示例展示了如何使用 <xref:System.IO.Pipes.NamedPipeServerStream> 类创建命名管道。 在此示例中，服务器进程创建四个线程。 每个线程都可以接受客户端连接。 然后，连接的客户端进程向服务器提供文件名。 如果客户端拥有足够的权限，服务器进程就会打开文件，并将它的内容发送回客户端。  
  
 [!code-cpp[System.IO.Pipes.NamedPipeServerStream_ImpersonationSample1#01](../../../samples/snippets/cpp/VS_Snippets_CLR_System/system.IO.Pipes.NamedPipeServerStream_ImpersonationSample1/cpp/program.cpp#01)]
 [!code-csharp[System.IO.Pipes.NamedPipeServerStream_ImpersonationSample1#01](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.IO.Pipes.NamedPipeServerStream_ImpersonationSample1/cs/Program.cs#01)]
 [!code-vb[System.IO.Pipes.NamedPipeServerStream_ImpersonationSample1#01](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.IO.Pipes.NamedPipeServerStream_ImpersonationSample1/vb/program.vb#01)]  
  
## <a name="example"></a>示例  

 下面的示例展示了使用 <xref:System.IO.Pipes.NamedPipeClientStream> 类的客户端进程。 客户端连接到服务器进程，并将文件名发送到服务器。 因为此示例使用模拟，所以运行客户端应用的标识必须有权访问文件。 然后，服务器将文件内容发送回客户端。 接下来，文件内容在控制台中显示。  
  
 [!code-csharp[System.IO.Pipes.NamedPipeClientStream_ImpersonationSample1#01](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.IO.Pipes.NamedPipeClientStream_ImpersonationSample1/cs/Program.cs#01)]
 [!code-vb[System.IO.Pipes.NamedPipeClientStream_ImpersonationSample1#01](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.IO.Pipes.NamedPipeClientStream_ImpersonationSample1/vb/program.vb#01)]  
  
## <a name="robust-programming"></a>可靠编程  

 因为此示例中的客户端进程和服务器进程可以在同一台计算机上运行，所以提供给 <xref:System.IO.Pipes.NamedPipeClientStream> 对象的服务器名称为 `"."`。 如果客户端进程和服务器进程在不同的计算机上运行，`"."` 会被替换为运行服务器进程的计算机的网络名称。  
  
## <a name="see-also"></a>请参阅

- <xref:System.Security.Principal.TokenImpersonationLevel>
- <xref:System.IO.Pipes.NamedPipeServerStream.GetImpersonationUserName%2A>
- [管道](pipe-operations.md)
- [如何：使用匿名管道进行本地进程间通信](how-to-use-anonymous-pipes-for-local-interprocess-communication.md)
