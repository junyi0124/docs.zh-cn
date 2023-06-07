---
title: 如何：从 XML 文件读取对象数据
ms.date: 07/20/2015
ms.assetid: 1e1423bf-74a4-4dde-a3bb-ae1bfc0a68ed
ms.openlocfilehash: 7677b32f76bee3fe579f96715b6c748c08c83a82
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91077236"
---
# <a name="how-to-read-object-data-from-an-xml-file-visual-basic"></a>如何：读取 XML 文件中的对象数据 (Visual Basic)

本示例使用 <xref:System.Xml.Serialization.XmlSerializer> 类读取之前写入 XML 文件的对象数据。  
  
## <a name="example"></a>示例  
  
```vb  
Public Class Book  
    Public Title As String  
End Class  
  
Public Sub ReadXML()  
    Dim reader As New System.Xml.Serialization.XmlSerializer(GetType(Book))  
    Dim file As New System.IO.StreamReader(  
        "c:\temp\SerializationOverview.xml")  
    Dim overview As Book  
    overview = CType(reader.Deserialize(file), Book)  
    Console.WriteLine(overview.Title)  
End Sub  
```  
  
## <a name="compile-the-code"></a>编译代码  

 将文件名称“c:\temp\SerializationOverview.xml”替换为包含序列化数据的文件的名称。 有关序列化数据的详细信息，请参阅 [如何：将对象数据写入 XML 文件 (Visual Basic) ](how-to-write-object-data-to-an-xml-file.md)。  
  
 类必须有一个公共的无参数构造函数。  
  
 只有公共属性和字段才会进行反序列化。  
  
## <a name="robust-programming"></a>可靠编程  

 以下情况可能会导致异常：  
  
- 进行序列化的类没有公共的无参数构造函数。  
  
- 文件中的数据不表示要进行反序列化的类中的数据。  
  
- 该文件不存在 (<xref:System.IO.IOException>)。  
  
## <a name="net-framework-security"></a>.NET Framework 安全性  

 始终验证输入，并且绝不会反序列化来自不受信任源的数据。 重新创建的对象会在具有对它进行反序列化的代码的权限的本地计算机上运行。 在应用程序中使用输入的数据之前，需验证所有的输入内容。  
  
## <a name="see-also"></a>请参阅

- <xref:System.IO.StreamWriter>
- [如何：将对象数据写入 XML 文件 (Visual Basic)](how-to-write-object-data-to-an-xml-file.md)
- 序列化 (Visual Basic)[](index.md)
- [Visual Basic 编程指南](../../index.md)
