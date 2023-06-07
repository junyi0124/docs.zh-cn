---
title: 如何：复制目录
description: 了解如何使用 I/O 类来复制目录，这些类可将一个目录下的内容同步复制到另一个位置。
ms.date: 12/27/2018
dev_langs:
- csharp
- vb
helpviewer_keywords:
- directory copying
- I/O [.NET], copying directories
- subdirectory copying
- copying directories
- directories [.NET], copying
ms.assetid: 5a969765-e5f8-4b4e-977e-90e2b0a1fe3c
ms.openlocfilehash: dfe45d8529eb927a6b174a7bb411afa8072035f9
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95679060"
---
# <a name="how-to-copy-directories"></a>如何：复制目录

本文演示如何使用 I/O 类将目录下的内容同步复制到另一个位置。

有关异步文件复制的示例，请参阅[异步文件 I/O](asynchronous-file-i-o.md)。

此示例通过将 `DirectoryCopy` 方法的 `copySubDirs` 设置为 `true` 来复制子目录。 `DirectoryCopy` 方法通过对每个子目录调用其自身的方法来递归复制它们，直到再也没有子目录可以复制为止。  
  
## <a name="example"></a>示例  

 [!code-csharp[System.IO.Directory_Copy#1](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.IO.Directory_Copy/cs/program.cs#1)]
 [!code-vb[System.IO.Directory_Copy#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.IO.Directory_Copy/vb/Program.vb#1)]  
  
[!INCLUDE [localized code comments](../../../includes/code-comments-loc.md)]

## <a name="see-also"></a>请参阅

- <xref:System.IO.FileInfo>
- <xref:System.IO.DirectoryInfo>
- <xref:System.IO.FileStream>
- [文件和流 I/O](index.md)
- [通用 I/O 任务](common-i-o-tasks.md)
- [异步文件 I/O](asynchronous-file-i-o.md)
