---
title: 如何将对象数据写入 XML 文件 (C#)
description: 此 C# 示例使用 XmlSerializer 类从某个类将对象写入 XML 文件。 了解如何编译代码。
ms.date: 07/20/2015
ms.assetid: 7681eb98-703d-4005-a369-26a7bca0f894
ms.openlocfilehash: 776ade1752adf15d6acce07d38120de8481a233d
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91178705"
---
# <a name="how-to-write-object-data-to-an-xml-file-c"></a>如何将对象数据写入 XML 文件 (C#)

本示例使用 <xref:System.Xml.Serialization.XmlSerializer> 类从某个类将对象写入 XML 文件。  
  
## <a name="example"></a>示例  
  
```csharp  
public class XMLWrite  
{  
  
   static void Main(string[] args)  
    {  
        WriteXML();  
    }  
  
    public class Book  
    {  
        public String title;
    }  
  
    public static void WriteXML()  
    {  
        Book overview = new Book();  
        overview.title = "Serialization Overview";  
        System.Xml.Serialization.XmlSerializer writer =
            new System.Xml.Serialization.XmlSerializer(typeof(Book));  
  
        var path = Environment.GetFolderPath(Environment.SpecialFolder.MyDocuments) + "//SerializationOverview.xml";  
        System.IO.FileStream file = System.IO.File.Create(path);  
  
        writer.Serialize(file, overview);  
        file.Close();  
    }  
}  
```  
  
## <a name="compiling-the-code"></a>编译代码  

 所序列化的类必须有一个公共的无参数构造函数。  
  
## <a name="robust-programming"></a>可靠编程  

 以下情况可能会导致异常：  
  
- 进行序列化的类没有公共的无参数构造函数。  
  
- 文件存在且为只读 (<xref:System.IO.IOException>)。  
  
- 路径过长 (<xref:System.IO.PathTooLongException>)。  
  
- 磁盘已满 (<xref:System.IO.IOException>)。  
  
## <a name="net-security"></a>.NET 安全性  

 此示例在文件尚未存在时创建新文件。 如果某个应用程序需要创建文件，则该应用程序需要针对文件夹的 `Create` 访问权限。 如果文件已存在，则该应用程序只需要 `Write` 访问权限（这是较弱的特权）。 如有可能，在部署过程中创建文件，并且仅授予针对单个文件的 `Read` 访问权限（而不是针对 `Create` 文件夹的访问权限）会更加安全。  
  
## <a name="see-also"></a>请参阅

- <xref:System.IO.StreamWriter>
- [如何从 XML 文件读取对象数据 (C#)](./how-to-read-object-data-from-an-xml-file.md)
- [序列化 (C#)](./index.md)
