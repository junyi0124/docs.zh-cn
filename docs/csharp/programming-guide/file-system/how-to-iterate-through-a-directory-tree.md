---
title: 如何循环访问目录树 - C# 编程指南
description: 了解如何循环访问目录树。 访问指定根文件夹下每个嵌套子目录中的每个文件。
ms.date: 07/20/2015
helpviewer_keywords:
- iterating through folders [C#]
- file iteration [C#]
ms.assetid: c4be4a75-6b1b-46a7-9d38-bab353091ed7
ms.openlocfilehash: 9d927e8517ddbdb1c5a9a8aa8ca3c321bf7e8d9c
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91178536"
---
# <a name="how-to-iterate-through-a-directory-tree-c-programming-guide"></a>如何循环访问目录树（C# 编程指南）

短语“循环访问目录树”的意思是访问特定根文件夹下的每个嵌套子目录中的每个文件，可以是任意深度。 不需要打开每个文件。 可以以 `string` 的形式只检索文件或子目录的名称，也可以以 <xref:System.IO.FileInfo?displayProperty=nameWithType> 或 <xref:System.IO.DirectoryInfo?displayProperty=nameWithType> 对象的形式检索其他信息。  
  
> [!NOTE]
> 在 Windows 中，术语“目录”和“文件夹”可以互换使用。 大多数文档和用户界面文本使用术语“文件夹”，但 .NET 类库使用术语“目录”。  
  
 在最简单的情况下，如果你确信拥有指定根目录下的所有目录的访问权限，则可以使用 `System.IO.SearchOption.AllDirectories` 标志。 此标志返回与指定的模式匹配的所有嵌套的子目录。 下面的示例演示如何使用此标志。  
  
```csharp  
root.GetDirectories("*.*", System.IO.SearchOption.AllDirectories);  
```  
  
 此方法的缺点是，如果指定根目录下的任何子目录引发 <xref:System.IO.DirectoryNotFoundException> 或 <xref:System.UnauthorizedAccessException> 异常，则整个方法失败且不返回任何目录。 使用 <xref:System.IO.DirectoryInfo.GetFiles%2A> 方法时也是如此。 如果需要处理特定子文件夹中的异常，则必须手动遍历目录树，如以下示例所示。  
  
 手动遍历目录树时，可以先处理子目录（前序遍历），或者先处理文件（后序遍历）。 如果执行前序遍历，那么在遍历直接位于当前文件夹中的文件之前，先遍历此文件夹下的整棵树。 本文档后面的示例执行的是后序遍历，但你可以轻松地修改它们以执行前序遍历。  
  
 另一种选择是，是使用递归遍历还是基于堆栈的遍历。 本文档后面的示例演示了这两种方法。  
  
 如果需要对文件和文件夹执行各种操作，则可以模块化这些示例，方法是将操作重构为可使用单个委托进行调用的单独的函数。  
  
> [!NOTE]
> NTFS 文件系统可以包含交接点、符号链接和硬链接等形式的重解析点。 诸如 <xref:System.IO.DirectoryInfo.GetFiles%2A> 和 <xref:System.IO.DirectoryInfo.GetDirectories%2A> 等 .NET 方法不会返回重分析点下的任何子目录。 当两个重解析点相互引用时，此行为可防止进入无限循环。 通常，处理重解析点时应格外小心，以确保不会无意中修改或删除文件。 如果需要精确控制重解析点，请使用平台调用或本机代码直接调用相应的 Win32 文件系统方法。  
  
## <a name="example"></a>示例  

 下面的示例演示如何以递归方式遍历目录树。 递归方法是一种很好的方法，但是如果目录树较大且嵌套深度较深，则可能引起堆栈溢出异常。  
  
 在每个文件或文件夹上处理的特定异常和执行的特定操作仅作为示例提供。 你可以修改此代码来满足你的特定要求。 有关详细信息，请参阅代码中的注释。  
  
 [!code-csharp[csFilesandFolders#1](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csFilesAndFolders/CS/FileIteration.cs#1)]  
  
## <a name="example"></a>示例  

 下面的示例演示如何不使用递归方式遍历目录树中的文件和文件夹。 此方法使用泛型 <xref:System.Collections.Generic.Stack%601> 集合类型，此集合类型是一个后进先出 (LIFO) 堆栈。  
  
 在每个文件或文件夹上处理的特定异常和执行的特定操作仅作为示例提供。 你可以修改此代码来满足你的特定要求。 有关详细信息，请参阅代码中的注释。  
  
 [!code-csharp[csFilesandFolders#2](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csFilesAndFolders/CS/FileIteration.cs#2)]  
  
 通常，检测每个文件夹以确定应用程序是否有权限打开它是一个很费时的过程。 因此，此代码示例只将此部分操作封装在 `try/catch` 块中。 你可以修改 `catch` 块，以便在拒绝访问某个文件夹时，可以尝试提升权限，然后再次访问此文件夹。 一般来说，仅捕获可以处理的、不会将应用程序置于未知状态的异常。  
  
 如果必须在内存或磁盘上存储目录树的内容，那么最佳选择是仅存储每个文件的 <xref:System.IO.FileSystemInfo.FullName%2A> 属性（类型为 `string`）。 然后可以根据需要使用此字符串创建新的 <xref:System.IO.FileInfo> 或 <xref:System.IO.DirectoryInfo> 对象，或打开需要进行其他处理的任何文件。  
  
## <a name="robust-programming"></a>可靠编程  

 可靠的文件迭代代码必须考虑文件系统的诸多复杂性。 有关 Windows 文件系统的详细信息，请参阅 [NTFS 概述](/windows-server/storage/file-server/ntfs-overview)。  
  
## <a name="see-also"></a>请参阅

- <xref:System.IO>
- [LINQ 和文件目录](../concepts/linq/linq-and-file-directories.md)
- [文件系统和注册表（C# 编程指南）](./index.md)
