---
title: 如何写入文本文件 - C# 编程指南
description: 了解如何使用 C# 编写文本文件。 查看一些代码示例和其他可用资源。
ms.date: 07/20/2015
f1_keywords:
- TextWriter.WriteLine
- StreamWriter.Close
helpviewer_keywords:
- files [C#], text files
- text, writing to files [C#]
ms.assetid: 2e99f184-d88b-4719-a7f1-d9ec482aa809
ms.openlocfilehash: 48739852cd1730d71c3b20ae6a9394b57785faa6
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91170397"
---
# <a name="how-to-write-to-a-text-file-c-programming-guide"></a>如何写入文本文件（C# 编程指南）

以下示例给出了将文本写入文件的各种方法。 前两个示例对 <xref:System.IO.File?displayProperty=nameWithType> 类使用静态便捷方法以将任何 `IEnumerable<string>` 的每个元素和字符串写入文本文件。 示例 3 展示了在写入文件时必须分别处理文本的每一行时，如何将文本添加到文件。 示例 1-3 覆盖文件中的所有现有内容，但示例 4 展示如何将文本追加到现有文件。  
  
 这些示例都将字符串文本写入了文件。 如果想设置写入文件的文本的格式，请使用 <xref:System.String.Format%2A> 方法或 C# [字符串内插](../../language-reference/tokens/interpolated.md)功能。  
  
## <a name="example"></a>示例  

 [!code-csharp[csFilesandFolders#3](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csFilesAndFolders/CS/FileIteration.cs#3)]  
  
## <a name="robust-programming"></a>可靠编程  

 以下情况可能会导致异常：  
  
- 文件已存在并且为只读。  
  
- 路径名可能太长。  
  
- 磁盘可能已满。  
  
## <a name="see-also"></a>另请参阅

- [C# 编程指南](../index.md)
- [文件系统和注册表（C# 编程指南）](./index.md)
- [示例：将集合保存到应用程序存储](https://code.msdn.microsoft.com/CSWinStoreAppSaveCollection-bed5d6e6)
