---
title: 如何使用 XmlSerializer 反序列化对象
description: 了解如何反序列化对象。 传输格式决定着是创建流还是文件对象。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- deserializing objects
- objects, deserializing steps
ms.assetid: 287129c8-035a-4fea-b7b3-4790057ca076
ms.openlocfilehash: e08ae0d77539219223650fd3bcbd1bcee4df2739
ms.sourcegitcommit: d6bd7903d7d46698e9d89d3725f3bb4876891aa3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/13/2020
ms.locfileid: "83379114"
---
# <a name="how-to-deserialize-an-object-using-xmlserializer"></a>如何使用 XmlSerializer 反序列化对象

当您反序列化对象时，传输格式确定您将创建流还是文件对象。 确定了传输格式之后，就可以根据需要调用 <xref:System.Xml.Serialization.XmlSerializer.Serialize%2A> 或 <xref:System.Xml.Serialization.XmlSerializer.Deserialize%2A> 方法。

## <a name="to-deserialize-an-object"></a>反序列化对象

1. 使用要反序列化的对象的类型构造 <xref:System.Xml.Serialization.XmlSerializer>。

1. 调用 <xref:System.Xml.Serialization.XmlSerializer.Deserialize%2A> 方法以生成该对象的副本。 在反序列化时，必须将返回的对象强制转换为原始对象的类型，如以下示例所示，该示例从文件反序列化该对象（尽管也可以从流反序列化该对象）。

    ```vb
    ' Construct an instance of the XmlSerializer with the type
    ' of object that is being deserialized.
    Dim mySerializer As New XmlSerializer(GetType(MySerializableClass))
    ' To read the file, create a FileStream.
    Dim myFileStream As New FileStream("myFileName.xml", FileMode.Open)
    ' Call the Deserialize method and cast to the object type.
    Dim myObject = CType( _
    mySerializer.Deserialize(myFileStream), MySerializableClass)
    ```

    ```csharp
    // Construct an instance of the XmlSerializer with the type
    // of object that is being deserialized.
    var mySerializer = new XmlSerializer(typeof(MySerializableClass));
    // To read the file, create a FileStream.
    var myFileStream = new FileStream("myFileName.xml", FileMode.Open);
    // Call the Deserialize method and cast to the object type.
    var myObject = (MySerializableClass) mySerializer.Deserialize(myFileStream)
    ```

## <a name="see-also"></a>请参阅

- [XML 序列化简介](introducing-xml-serialization.md)
- [如何：序列化对象](how-to-serialize-an-object.md)
