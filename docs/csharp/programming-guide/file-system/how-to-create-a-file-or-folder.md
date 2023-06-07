---
title: 如何创建文件或文件夹 - C# 编程指南
description: 了解如何通过编程方式创建文件或文件夹。 可以创建文件夹、子文件夹和子文件夹中的文件，并将数据写入该文件。
ms.date: 07/20/2015
helpviewer_keywords:
- folders [C#]
- creating files [C#]
- files [C#]
- creating folders [C#]
ms.assetid: 4582ee2d-d72d-4687-bcb9-08d336c62c25
ms.openlocfilehash: 4d60b8c6c0f9d4ea66125374327f5e1ad2098694
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91167439"
---
# <a name="how-to-create-a-file-or-folder-c-programming-guide"></a>如何创建文件或文件夹（C# 编程指南）

可通过编程方式在计算机上创建文件夹、子文件夹和子文件夹中的文件，并将数据写入文件。  
  
## <a name="example"></a>示例  

 [!code-csharp[csFilesandFolders#10](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csFilesAndFolders/CS/FileIteration.cs#10)]  
  
 如果文件夹已存在，<xref:System.IO.Directory.CreateDirectory%2A> 不执行任何操作，未引发任何异常。 但 <xref:System.IO.File.Create%2A?displayProperty=nameWithType> 用新文件替换现有文件。 本示例使用 `if`-`else` 语句阻止替换现有文件。  
  
 通过在示例中作出以下更改，可根据具有特定名称的文件是否存在来指定不同的结果。 如果该文件不存在，代码就会创建一个文件。 如果该文件存在，代码就会将数据追加到该文件中。  
  
- 指定一个非随机文件名。  
  
    ```csharp  
    // Comment out the following line.  
    //string fileName = System.IO.Path.GetRandomFileName();  
  
    // Replace that line with the following assignment.  
    string fileName = "MyNewFile.txt";  
    ```  
  
- 用以下代码中的 `using` 语句替换 `if`-`else` 语句。  
  
    ```csharp  
    using (System.IO.FileStream fs = new System.IO.FileStream(pathString, FileMode.Append))
    {  
        for (byte i = 0; i < 100; i++)  
        {  
            fs.WriteByte(i);  
        }  
    }  
    ```  
  
 多次运行此示例，验证数据是否每次都添加到了文件中。  
  
 有关可尝试的 `FileMode` 值，请参阅 <xref:System.IO.FileMode>。  
  
 以下情况可能会导致异常：  
  
- 文件夹名称格式不正确。 例如，它包含非法字符或它仅为空格（<xref:System.ArgumentException> 类）。 使用 <xref:System.IO.Path> 类创建有效的路径名。  
  
- 要创建的文件夹的父文件夹为只读（<xref:System.IO.IOException> 类）。  
  
- 文件夹名为 `null`<xref:System.ArgumentNullException> 类）。  
  
- 文件夹名过长（<xref:System.IO.PathTooLongException> 类）。  
  
- 文件夹仅为冒号“:”（<xref:System.IO.PathTooLongException> 类）。  
  
## <a name="net-security"></a>.NET 安全性  

 可能在部分信任场景中引发 <xref:System.Security.SecurityException> 类的实例。  
  
 如果没有创建文件夹的权限，则本示例引发 <xref:System.UnauthorizedAccessException> 类的实例。  
  
## <a name="see-also"></a>请参阅

- <xref:System.IO?displayProperty=nameWithType>
- [C# 编程指南](../index.md)
- [文件系统和注册表（C# 编程指南）](./index.md)
