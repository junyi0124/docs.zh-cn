---
title: 如何获取有关文件、文件夹和驱动器的信息 - C# 编程指南
description: 了解如何获取有关文件、文件夹和驱动器的信息。 查看代码示例和其他可用资源。
ms.date: 07/20/2015
helpviewer_keywords:
- files [C#], getting information about
ms.assetid: 22fc2da6-5494-405b-995e-c0b99142a93e
ms.openlocfilehash: 7cbaea4dc5381a2ebeb97ce2797ffe850488e126
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91170449"
---
# <a name="how-to-get-information-about-files-folders-and-drives--c-programming-guide"></a>如何获取有关文件、文件夹和驱动器的信息（C# 编程指南）

在 .NET 中，可以使用以下类访问文件系统信息：  
  
- <xref:System.IO.FileInfo?displayProperty=nameWithType>  
  
- <xref:System.IO.DirectoryInfo?displayProperty=nameWithType>  
  
- <xref:System.IO.DriveInfo?displayProperty=nameWithType>  
  
- <xref:System.IO.Directory?displayProperty=nameWithType>  
  
- <xref:System.IO.File?displayProperty=nameWithType>  
  
 <xref:System.IO.FileInfo> 和 <xref:System.IO.DirectoryInfo> 类表示文件或目录，并包含用于公开 NTFS 文件系统所支持的许多文件特性的属性。 它们还包含用于打开、关闭、移动和删除文件和文件夹的方法。 可以通过将表示文件、文件夹或驱动器名称的字符串传入构造函数来创建这些类的实例：  
  
```csharp  
System.IO.DriveInfo di = new System.IO.DriveInfo(@"C:\");  
```  
  
 还可以使用对 <xref:System.IO.DirectoryInfo.GetDirectories%2A?displayProperty=nameWithType><xref:System.IO.DirectoryInfo.GetFiles%2A?displayProperty=nameWithType> 和 <xref:System.IO.DriveInfo.RootDirectory%2A?displayProperty=nameWithType> 的调用来获取文件、文件夹或驱动器的名称。  
  
 <xref:System.IO.Directory?displayProperty=nameWithType> 和 <xref:System.IO.File?displayProperty=nameWithType> 类提供相关静态方法，用于检索有关目录和文件的信息。  
  
## <a name="example"></a>示例  

 下面的示例演示用于访问有关文件和文件夹的信息的各种方法。  
  
 [!code-csharp[csFilesandFolders#6](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csFilesAndFolders/CS/FileIteration.cs#6)]  
  
## <a name="robust-programming"></a>可靠编程  

 处理用户指定的路径字符串时，还应针对以下情况处理异常：  
  
- 文件名格式不正确。 例如，它包含无效字符或只包含空格。  
  
- 文件名为 null。  
  
- 文件名超过系统定义的最大长度。  
  
- 文件名包含冒号 (:)。  
  
 如果应用程序没有足够权限来读取指定文件，`Exists` 方法会返回 `false`（无论路径是否存在）；该方法不会引发异常。  
  
## <a name="see-also"></a>请参阅

- <xref:System.IO?displayProperty=nameWithType>
- [C# 编程指南](../index.md)
- [文件系统和注册表（C# 编程指南）](./index.md)
