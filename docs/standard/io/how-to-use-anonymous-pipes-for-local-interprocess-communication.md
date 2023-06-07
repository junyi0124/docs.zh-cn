---
title: 如何：使用匿名管道进行本地进程间通信
description: 了解如何在 .NET 中的本地计算机上使用匿名管道进行本地进程间通信。 与命名管道相比，匿名管道需要的开销更少。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- anonymous pipes [.NET]
- parent-child communication [.NET]
- pipes [.NET]
- one-way communication [.NET]
- local computer communication [.NET], pipes
ms.assetid: e7773c77-c646-4a01-8a96-a003d59fc4c9
ms.openlocfilehash: 389451d1c6c313ac4a10652c7aa614a5e4e7bf1c
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95734551"
---
# <a name="how-to-use-anonymous-pipes-for-local-interprocess-communication"></a>如何：使用匿名管道进行本地进程间通信

匿名管道在本地计算机上提供进程间通信。 它们提供的功能比命名管道少，但所需要的系统开销也少。 使用匿名管道，可以在本地计算机上更轻松地进行进程间通信。 不能使用匿名管道进行网络通信。  
  
 若要实现匿名管道，请使用 <xref:System.IO.Pipes.AnonymousPipeServerStream> 和 <xref:System.IO.Pipes.AnonymousPipeClientStream> 类。  
  
## <a name="example"></a>示例  

 下面的示例展示了如何使用匿名管道将字符串从父进程发送到子进程。 此示例在父进程中创建 <xref:System.IO.Pipes.AnonymousPipeServerStream> 对象，它的 <xref:System.IO.Pipes.PipeDirection> 值为 <xref:System.IO.Pipes.PipeDirection.Out>。 然后，父进程使用客户端句柄创建 <xref:System.IO.Pipes.AnonymousPipeClientStream> 对象，以创建子进程。 子进程的 <xref:System.IO.Pipes.PipeDirection> 值为 <xref:System.IO.Pipes.PipeDirection.In>。  
  
 接下来，父进程将用户提供的字符串发送给子进程。 字符串在子进程的控制台中显示。  
  
 下面的示例展示了服务器进程。  
  
 [!code-cpp[System.IO.Pipes.AnonymousPipeServerStream_Sample#01](../../../samples/snippets/cpp/VS_Snippets_CLR_System/system.IO.Pipes.AnonymousPipeServerStream_Sample/cpp/program.cpp#01)]
 [!code-csharp[System.IO.Pipes.AnonymousPipeServerStream_Sample#01](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.IO.Pipes.AnonymousPipeServerStream_Sample/cs/Program.cs#01)]
 [!code-vb[System.IO.Pipes.AnonymousPipeServerStream_Sample#01](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.IO.Pipes.AnonymousPipeServerStream_Sample/vb/program.vb#01)]  

[!INCLUDE [localized code comments](../../../includes/code-comments-loc.md)]
  
## <a name="example"></a>示例  

 下面的示例展示了客户端进程。 服务器进程启动客户端进程，并为此进程提供客户端句柄。 客户端代码生成的可执行文件应命名为 `pipeClient.exe`，并在运行服务器进程前复制到服务器可执行文件所在的相同目录中。  
  
 [!code-cpp[System.IO.Pipes.AnonymousPipeClientStream_Sample#01](../../../samples/snippets/cpp/VS_Snippets_CLR_System/system.IO.Pipes.AnonymousPipeClientStream_Sample/cpp/program.cpp#01)]
 [!code-csharp[System.IO.Pipes.AnonymousPipeClientStream_Sample#01](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.IO.Pipes.AnonymousPipeClientStream_Sample/cs/Program.cs#01)]
 [!code-vb[System.IO.Pipes.AnonymousPipeClientStream_Sample#01](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.IO.Pipes.AnonymousPipeClientStream_Sample/vb/program.vb#01)]  
  
## <a name="see-also"></a>请参阅

- [管道](pipe-operations.md)
- [如何：使用命名管道进行网络进程间通信](how-to-use-named-pipes-for-network-interprocess-communication.md)
