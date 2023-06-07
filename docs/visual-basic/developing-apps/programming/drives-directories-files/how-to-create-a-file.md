---
title: 如何：创建文件
ms.date: 07/20/2015
helpviewer_keywords:
- text files [Visual Basic], creating
- files [Visual Basic], creating
ms.assetid: 0253bb6d-5519-4a50-b882-b93ef5cca0d9
ms.openlocfilehash: 20533ec01d3198d499312ed0c15ec8cca2ff70bd
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2020
ms.locfileid: "74348800"
---
# <a name="how-to-create-a-file-in-visual-basic"></a>如何：在 Visual Basic 中创建文件

此示例使用 <xref:System.IO.File> 类中的 <xref:System.IO.File.Create%2A> 方法在指定的路径中创建一个空文本文件。  
  
## <a name="example"></a>示例  

 [!code-vb[VbFileIOMisc#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbFileIOMisc/VB/class2.vb#1)]  
  
## <a name="compiling-the-code"></a>编译代码  

 使用 `file` 变量写入文件。  
  
## <a name="robust-programming"></a>可靠编程  

 如果该文件已存在，则替换它。  
  
 以下情况可能会导致异常：  
  
- 路径名称格式不正确。 例如，它包含非法字符或仅为空白 (<xref:System.ArgumentException>)。  
  
- 路径是只读的 (<xref:System.IO.IOException>)。  
  
- 路径名称为 `Nothing` (<xref:System.ArgumentNullException>)。  
  
- 路径名称过长 (<xref:System.IO.PathTooLongException>)。  
  
- 路径无效 (<xref:System.IO.DirectoryNotFoundException>)。  
  
- 路径仅为冒号“:”(<xref:System.NotSupportedException>)。  
  
## <a name="net-framework-security"></a>.NET Framework 安全性  

 在部分信任的环境中可能引发 <xref:System.Security.SecurityException>。  
  
 调用 <xref:System.IO.File.Create%2A> 方法需要 <xref:System.Security.Permissions.FileIOPermission>。  
  
 如果用户无权创建文件，将引发 <xref:System.UnauthorizedAccessException>。  
  
## <a name="see-also"></a>另请参阅

- <xref:System.IO>
- <xref:System.IO.File.Create%2A>
- [通过部分受信任的代码使用库](../../../../framework/misc/using-libraries-from-partially-trusted-code.md)
- [代码访问安全性基础知识](../../../../framework/misc/code-access-security-basics.md)
