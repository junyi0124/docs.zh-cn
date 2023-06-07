---
title: 如何：在独立存储中创建文件和目录
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- directories [.NET], isolated storage
- files [.NET], isolated storage
- isolated storage, creating files and directories
- data stores, creating files and directories
- data storage using isolated storage, creating files and directories
- stores, creating files and directories
- storing data using isolated storage, creating files and directories
ms.assetid: 2ca4d2a4-809b-4f00-bc08-bf4a64d3a5c3
ms.openlocfilehash: 1f6e8e1a048fcf7f8fd278eaac4988fa0e35791d
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95684988"
---
# <a name="how-to-create-files-and-directories-in-isolated-storage"></a>如何：在独立存储中创建文件和目录

获得独立存储区之后，可以创建用于存储数据的目录和文件。 在存储中，文件名和目录名称是相对于虚拟文件系统的根目录进行指定。  
  
 若要创建目录，请使用 <xref:System.IO.IsolatedStorage.IsolatedStorageFile.CreateDirectory%2A?displayProperty=nameWithType> 实例方法。 如果为不存在的目录指定了一个子目录，则会同时创建这两个目录。 如果您指定的目录已存在，该方法将返回而不创建目录，并且不会引发异常。 但是，如果您指定的目录名称包含无效字符，将引发 <xref:System.IO.IsolatedStorage.IsolatedStorageException> 异常。  
  
 若要创建文件，请使用 <xref:System.IO.IsolatedStorage.IsolatedStorageFile.CreateFile%2A?displayProperty=nameWithType> 方法。  
  
 在 Windows 操作系统，独立存储文件和目录名不区分大小写。 这样，如果您创建了一个名为 `ThisFile.txt` 的文件，然后又创建了名为 `THISFILE.TXT` 的另一个文件，实际上只创建了一个文件。 文件名保留原始大小写只为了方便本文演示。  

 如果路径包含的目录不存在，则创建独立存储文件会引发 <xref:System.IO.IsolatedStorage.IsolatedStorageException>。
  
## <a name="example"></a>示例  

 下面的代码示例展示了如何在独立存储中创建文件和目录。  
  
 [!code-csharp[Conceptual.IsolatedStorage#1](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.isolatedstorage/cs/source.cs#1)]
 [!code-vb[Conceptual.IsolatedStorage#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.isolatedstorage/vb/source.vb#1)]  
  
## <a name="see-also"></a>请参阅

- <xref:System.IO.IsolatedStorage.IsolatedStorageFile>
- <xref:System.IO.IsolatedStorage.IsolatedStorageFileStream>
- [独立存储](isolated-storage.md)
