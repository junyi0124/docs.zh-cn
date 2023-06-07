---
title: 如何：将对象数据写入 XML 文件
ms.date: 07/20/2015
ms.assetid: f7966480-5ed2-43ac-9894-33427436de2a
ms.openlocfilehash: af62d10b29e76701668fd3d595b967bd1754a22c
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91077223"
---
# <a name="how-to-write-object-data-to-an-xml-file-visual-basic"></a>如何：将对象数据写入 XML 文件 (Visual Basic)

本示例使用 <xref:System.Xml.Serialization.XmlSerializer> 类从某个类将对象写入 XML 文件。  
  
## <a name="example"></a>示例  
  
```vb  
Public Module XMLWrite  
  
    Sub Main()  
        WriteXML()  
    End Sub  
  
    Public Class Book  
        Public Title As String  
    End Class  
  
    Public Sub WriteXML()  
        Dim overview As New Book  
        overview.Title = "Serialization Overview"  
        Dim writer As New System.Xml.Serialization.XmlSerializer(GetType(Book))  
        Dim file As New System.IO.StreamWriter(  
            "c:\temp\SerializationOverview.xml")  
        writer.Serialize(file, overview)  
        file.Close()  
    End Sub  
End Module  
```  
  
## <a name="compile-the-code"></a>编译代码  

 类必须有一个公共的无参数构造函数。  
  
## <a name="robust-programming"></a>可靠编程  

 以下情况可能会导致异常：  
  
- 进行序列化的类没有公共的无参数构造函数。  
  
- 文件存在且为只读 (<xref:System.IO.IOException>)。  
  
- 路径过长 (<xref:System.IO.PathTooLongException>)。  
  
- 磁盘已满 (<xref:System.IO.IOException>)。  
  
## <a name="net-framework-security"></a>.NET Framework 安全性  

 此示例在文件尚未存在时创建新文件。 如果某个应用程序需要创建文件，则该应用程序需要针对文件夹的 `Create` 访问权限。 如果文件已存在，则该应用程序只需要 `Write` 访问权限（这是较弱的特权）。 如有可能，在部署过程中创建文件，并且仅授予针对单个文件的 `Read` 访问权限（而不是针对 `Create` 文件夹的访问权限）会更加安全。  
  
## <a name="see-also"></a>请参阅

- <xref:System.IO.StreamWriter>
- [如何：读取 XML 文件中的对象数据 (Visual Basic)](how-to-read-object-data-from-an-xml-file.md)
- 序列化 (Visual Basic)[](index.md)
