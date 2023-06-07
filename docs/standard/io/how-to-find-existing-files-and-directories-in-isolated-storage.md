---
title: 如何：在独立存储中查找现有文件和目录
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- stores, finding files and directories
- locating files in isolated storage file
- directories [.NET], isolated storage
- isolated storage, finding files and directories
- data storage using isolated storage, finding files and directories
- files [.NET], isolated storage
- data stores, finding files and directories
- locating directories in isolated storage file
- storing data using isolated storage, finding files and directories
ms.assetid: eb28458a-6161-4e7a-9ada-30ef93761b5c
ms.openlocfilehash: 43685a6ecb92510ad8d80c472a1c774d46cbb5f7
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95734629"
---
# <a name="how-to-find-existing-files-and-directories-in-isolated-storage"></a>如何：在独立存储中查找现有文件和目录

为了搜索独立存储中的目录，请使用 <xref:System.IO.IsolatedStorage.IsolatedStorageFile.GetDirectoryNames%2A?displayProperty=nameWithType> 方法。 此方法接受表示搜索模式的字符串。 你可以在搜索模式中使用单字符 (?) 和多字符 (\*) 通配符，但是通配符必须出现在名称的最后一部分。 例如，`directory1/*ect*` 是有效的搜索字符串，但 `*ect*/directory2` 不是。  
  
 若要搜索文件，请使用 <xref:System.IO.IsolatedStorage.IsolatedStorageFile.GetFileNames%2A?displayProperty=nameWithType> 方法。 对适用于 <xref:System.IO.IsolatedStorage.IsolatedStorageFile.GetDirectoryNames%2A> 的搜索字符串中通配符的限制也适用于 <xref:System.IO.IsolatedStorage.IsolatedStorageFile.GetFileNames%2A>。  
  
 这些方法都不是递归的；<xref:System.IO.IsolatedStorage.IsolatedStorageFile> 类不提供用于列出存储区中所有目录或文件的任何方法。 但是，在下面的代码示例中显示有递归方法。  
  
## <a name="example"></a>示例  

 下面的代码示例展示了如何在独立存储中创建文件和目录。 首先，检索一个为用户、域和程序集隔离的存储区，并放入 `isoStore` 变量。 <xref:System.IO.IsolatedStorage.IsolatedStorageFile.CreateDirectory%2A> 方法用于设置几个不同的目录，<xref:System.IO.IsolatedStorage.IsolatedStorageFileStream.%23ctor%28System.String%2CSystem.IO.FileMode%2CSystem.IO.IsolatedStorage.IsolatedStorageFile%29> 构造函数在这些目录中创建了一些文件。 然后，代码循环访问 `GetAllDirectories` 方法的结果。 该方法使用 <xref:System.IO.IsolatedStorage.IsolatedStorageFile.GetDirectoryNames%2A> 来查找当前目录中的所有目录名。 这些名称存储在数组中，然后 `GetAllDirectories` 调用其本身，传入它所找到的每个目录。 结果是所有目录名都返回到数组中。 接下来，代码调用 `GetAllFiles` 方法。 该方法调用 `GetAllDirectories` 来查找所有目录的名称，然后它使用 <xref:System.IO.IsolatedStorage.IsolatedStorageFile.GetFileNames%2A> 方法检查文件的每个目录。 结果是在数组中返回，以供显示。  
  
 [!code-cpp[Conceptual.IsolatedStorage#9](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.isolatedstorage/cpp/source8.cpp#9)]
 [!code-csharp[Conceptual.IsolatedStorage#9](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.isolatedstorage/cs/source8.cs#9)]
 [!code-vb[Conceptual.IsolatedStorage#9](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.isolatedstorage/vb/source8.vb#9)]  
  
## <a name="see-also"></a>另请参阅

- <xref:System.IO.IsolatedStorage.IsolatedStorageFile>
- [独立存储](isolated-storage.md)
