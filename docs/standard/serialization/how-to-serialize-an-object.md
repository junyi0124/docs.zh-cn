---
title: 如何：序列化对象
description: 本文演示如何序列化对象。 选择 XML 流的传输格式，即它是作为流还是作为文件进行存储。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- serializing objects
- objects, serializing steps
ms.assetid: a1207d05-32b2-4953-8582-959607991227
ms.openlocfilehash: ce8510a987f75ed4110a44ffa9f2ac813d36a5be
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95678891"
---
# <a name="how-to-serialize-an-object"></a>如何：序列化对象

要序列化对象，首先应创建要序列化的对象，然后设置其公共属性和字段。 为此，必须确定 XML 流的传输格式，即它是作为流还是作为文件进行存储。 例如，如果 XML 流必须以永久形式保存，则应创建 <xref:System.IO.FileStream> 对象。  
  
> [!NOTE]
> 有关 XML 序列化的更多示例，请参见 [XML 序列化示例](examples-of-xml-serialization.md)。  
  
### <a name="to-serialize-an-object"></a>序列化对象  
  
1. 创建对象并设置其公共字段和属性。  
  
2. 使用对象的类型构造 <xref:System.Xml.Serialization.XmlSerializer>。 有关更多信息，请参见 <xref:System.Xml.Serialization.XmlSerializer> 类构造函数。  
  
3. 调用 <xref:System.Xml.Serialization.XmlSerializer.Serialize%2A> 方法生成对象的公共属性和字段的 XML 流或文件表示形式。 下面的示例将创建一个文件。  
  
    ```vb  
    Dim myObject As MySerializableClass = New MySerializableClass()  
    ' Insert code to set properties and fields of the object.  
    Dim mySerializer As XmlSerializer = New XmlSerializer(GetType(MySerializableClass))  
    ' To write to a file, create a StreamWriter object.  
    Dim myWriter As StreamWriter = New StreamWriter("myFileName.xml")  
    mySerializer.Serialize(myWriter, myObject)  
    myWriter.Close()  
    ```  
  
    ```csharp  
    MySerializableClass myObject = new MySerializableClass();  
    // Insert code to set properties and fields of the object.  
    XmlSerializer mySerializer = new
    XmlSerializer(typeof(MySerializableClass));  
    // To write to a file, create a StreamWriter object.  
    StreamWriter myWriter = new StreamWriter("myFileName.xml");  
    mySerializer.Serialize(myWriter, myObject);  
    myWriter.Close();  
    ```  
  
## <a name="see-also"></a>请参阅

- [XML 序列化简介](introducing-xml-serialization.md)
- [如何：反序列化对象](how-to-deserialize-an-object.md)
