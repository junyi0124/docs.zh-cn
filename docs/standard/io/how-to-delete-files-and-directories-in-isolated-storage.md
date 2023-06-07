---
title: 如何：在独立存储中删除文件和目录
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- data storage using isolated storage, deleting files and directories
- directories [.NET], isolated storage
- files [.NET], isolated storage
- isolated storage, deleting files and directories
- data stores, deleting files and directories
- stores, creating files and directories
- deleting files within isolated stage file
- storing data using isolated storage, deleting files and directories
- deleting directories within isolated stage file
ms.assetid: 8fcc0dea-435b-4d40-ba4d-ba056265c202
ms.openlocfilehash: ea1d7a460a8867f069aa4b4d915e03d6948e23b2
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95688505"
---
# <a name="how-to-delete-files-and-directories-in-isolated-storage"></a>如何：在独立存储中删除文件和目录

可以删除独立存储文件中的目录和文件。 在存储区中，文件名和目录名依赖于操作系统，并指定为虚拟文件系统根目录的相对路径。 在 Windows 操作系统中，它们不区分大小写。  
  
 <xref:System.IO.IsolatedStorage.IsolatedStorageFile?displayProperty=nameWithType> 类提供了两个用于删除目录和文件的方法：<xref:System.IO.IsolatedStorage.IsolatedStorageFile.DeleteDirectory%2A> 和 <xref:System.IO.IsolatedStorage.IsolatedStorageFile.DeleteFile%2A>。 如果尝试删除并不存在的文件和目录，则会引发 <xref:System.IO.IsolatedStorage.IsolatedStorageException> 异常。 如果在名称中包含通配符，<xref:System.IO.IsolatedStorage.IsolatedStorageFile.DeleteDirectory%2A> 将引发 <xref:System.IO.IsolatedStorage.IsolatedStorageException> 异常，并且 <xref:System.IO.IsolatedStorage.IsolatedStorageFile.DeleteFile%2A> 会引发 <xref:System.ArgumentException> 异常。  
  
 如果目录中包含任何文件或子目录，<xref:System.IO.IsolatedStorage.IsolatedStorageFile.DeleteDirectory%2A> 方法将会失败。 可以使用 <xref:System.IO.IsolatedStorage.IsolatedStorageFile.GetFileNames%2A> 和 <xref:System.IO.IsolatedStorage.IsolatedStorageFile.GetDirectoryNames%2A> 方法检索现有文件和目录。 若要详细了解如何搜索存储的虚拟文件系统，请参阅[如何：在独立存储中查找现有文件和目录](how-to-find-existing-files-and-directories-in-isolated-storage.md)。  
  
## <a name="example"></a>示例  

 下面的代码示例创建并删除多个目录和文件。  
  
 [!code-cpp[Conceptual.IsolatedStorage#4](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.isolatedstorage/cpp/source4.cpp#4)]
 [!code-csharp[Conceptual.IsolatedStorage#4](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.isolatedstorage/cs/source4.cs#4)]
 [!code-vb[Conceptual.IsolatedStorage#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.isolatedstorage/vb/source4.vb#4)]  
  
## <a name="see-also"></a>另请参阅

- <xref:System.IO.IsolatedStorage.IsolatedStorageFile?displayProperty=nameWithType>
- [独立存储](isolated-storage.md)
