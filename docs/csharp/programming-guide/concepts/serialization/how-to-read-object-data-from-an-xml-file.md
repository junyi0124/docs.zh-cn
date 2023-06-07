---
title: 如何从 XML 文件读取对象数据 (C#)
description: 本 C# 示例使用 XmlSerializer 类读取之前写入 XML 文件的对象数据。
ms.date: 07/20/2015
ms.assetid: 6ad60d96-a4d9-48e6-a8b0-d7f6f803cafa
ms.openlocfilehash: 8d607424201cfad08df1c5ffbfb66a114b31886d
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91178757"
---
# <a name="how-to-read-object-data-from-an-xml-file-c"></a>如何从 XML 文件读取对象数据 (C#)

本示例使用 <xref:System.Xml.Serialization.XmlSerializer> 类读取之前写入 XML 文件的对象数据。  
  
## <a name="example"></a>示例  
  
```csharp  
public class Book  
{  
    public String title;  
}
  
public void ReadXML()  
{  
    // First write something so that there is something to read ...  
    var b = new Book { title = "Serialization Overview" };  
    var writer = new System.Xml.Serialization.XmlSerializer(typeof(Book));  
    var wfile = new System.IO.StreamWriter(@"c:\temp\SerializationOverview.xml");  
    writer.Serialize(wfile, b);  
    wfile.Close();  
  
    // Now we can read the serialized book ...  
    System.Xml.Serialization.XmlSerializer reader =
        new System.Xml.Serialization.XmlSerializer(typeof(Book));  
    System.IO.StreamReader file = new System.IO.StreamReader(  
        @"c:\temp\SerializationOverview.xml");  
    Book overview =  (Book)reader.Deserialize(file);  
    file.Close();  
  
    Console.WriteLine(overview.title);  
  
}  
```  
  
## <a name="compiling-the-code"></a>编译代码  

将文件名称“c:\temp\SerializationOverview.xml”替换为包含序列化数据的文件的名称。 有关序列化数据的详细信息，请参阅[如何将对象数据写入 XML 文件 (C#)](./how-to-write-object-data-to-an-xml-file.md)。
  
 类必须有一个公共的无参数构造函数。  
  
 只有公共属性和字段才会进行反序列化。  
  
## <a name="robust-programming"></a>可靠编程  

 以下情况可能会导致异常：  
  
- 进行序列化的类没有公共的无参数构造函数。  
  
- 文件中的数据不表示要进行反序列化的类中的数据。  
  
- 该文件不存在 (<xref:System.IO.IOException>)。  
  
## <a name="net-security"></a>.NET 安全性  

 始终验证输入，并且绝不会反序列化来自不受信任源的数据。 重新创建的对象会在具有对它进行反序列化的代码的权限的本地计算机上运行。 在应用程序中使用输入的数据之前，需验证所有的输入内容。  
  
## <a name="see-also"></a>请参阅

- <xref:System.IO.StreamWriter>
- [如何将对象数据写入 XML 文件 (C#)](./how-to-write-object-data-to-an-xml-file.md)
- [序列化 (C#)](./index.md)
- [C# 编程指南](../../index.md)
