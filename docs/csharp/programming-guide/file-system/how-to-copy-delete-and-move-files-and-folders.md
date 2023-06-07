---
title: 如何复制、删除和移动文件和文件夹 - C# 编程指南
description: 了解如何使用 File、Directory、FileInfo 和 DirectoryInfo 类复制、删除和移动文件和文件夹。
ms.date: 07/20/2015
helpviewer_keywords:
- I/O [C#]
ms.assetid: 62e52cd7-9597-4e4a-acf9-1315f5cdbf05
ms.openlocfilehash: 24595c9f5c75b70afb0ff079b6b93e35502f7fb5
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91202508"
---
# <a name="how-to-copy-delete-and-move-files-and-folders-c-programming-guide"></a>如何复制、删除和移动文件和文件夹（C# 编程指南）

以下示例演示如何从 <xref:System.IO?displayProperty=nameWithType> 命名空间使用 <xref:System.IO.File?displayProperty=nameWithType>、<xref:System.IO.Directory?displayProperty=nameWithType>、<xref:System.IO.FileInfo?displayProperty=nameWithType> 和 <xref:System.IO.DirectoryInfo?displayProperty=nameWithType> 类以同步方式复制、移动和删除文件与文件夹。 这些示例未提供进度栏或其他任何用户界面。 如果希望提供一个标准进度对话框，请参阅[如何提供文件操作进度对话框](how-to-provide-a-progress-dialog-box-for-file-operations.md)。  
  
 操作多个文件时，请使用 <xref:System.IO.FileSystemWatcher?displayProperty=nameWithType> 提供事件，以便可以计算进度。 另一种方法是使用平台调用来调用 Windows Shell 中相应的文件相关方法。 有关如何异步执行这些文件操作的信息，请参阅[异步文件 I/O](../../../standard/io/asynchronous-file-i-o.md)。  
  
## <a name="example"></a>示例  

 以下示例演示如何复制文件和目录。  
  
 [!code-csharp[csFilesandFolders#7](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csFilesAndFolders/CS/FileIteration.cs#7)]  
  
## <a name="example"></a>示例  

 以下示例演示如何移动文件和目录。  
  
 [!code-csharp[csFilesandFolders#8](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csFilesAndFolders/CS/FileIteration.cs#8)]  
  
## <a name="example"></a>示例  

 以下示例演示如何删除文件和目录。  
  
 [!code-csharp[csFilesandFolders#9](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csFilesAndFolders/CS/FileIteration.cs#9)]  
  
## <a name="see-also"></a>另请参阅

- <xref:System.IO?displayProperty=nameWithType>
- [C# 编程指南](../index.md)
- [文件系统和注册表（C# 编程指南）](index.md)
- [如何提供文件操作进度对话框](how-to-provide-a-progress-dialog-box-for-file-operations.md)
- [文件和流 I/O](../../../standard/io/index.md)
- [通用 I/O 任务](../../../standard/io/common-i-o-tasks.md)
