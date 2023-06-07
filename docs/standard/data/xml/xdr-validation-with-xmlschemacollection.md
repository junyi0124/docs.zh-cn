---
title: 使用 XmlSchemaCollection 进行 XDR 验证
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 00833027-1428-4586-83c1-42f5de3323d1
ms.openlocfilehash: 9edde2fc0da97b570162775a33c95d472cda974b
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94819274"
---
# <a name="xdr-validation-with-xmlschemacollection"></a>使用 XmlSchemaCollection 进行 XDR 验证

如果验证所依据的 XML 数据缩减 (XDR) 架构存储在 XmlSchemaCollection  中，此架构与添加到集合时指定的命名空间 URI 相关联。 XmlValidatingReader  将 XML 文档中的命名空间 URI 映射到集合中与此 URI 对应的架构。

> [!IMPORTANT]
> 现在，<xref:System.Xml.Schema.XmlSchemaCollection> 类已过时，已由 <xref:System.Xml.Schema.XmlSchemaSet> 类所取代。 若要详细了解 <xref:System.Xml.Schema.XmlSchemaSet> 类，请参阅[用于编译架构的 XmlSchemaSet](xmlschemaset-for-schema-compilation.md)。

例如，如果 XML 文档的根元素是 `<bookstore xmlns="urn:newbooks-schema">`，且架构被添加到 XmlSchemaCollection  ，那么将引用相同的命名空间，如下所示：

```vb
xsc.Add("urn:newbooks-schema", "newbooks.xdr")
```

```csharp
xsc.Add("urn:newbooks-schema", "newbooks.xdr");
```

下面的代码示例创建 XmlValidatingReader  ，它需要使用 XmlTextReader  ，并将 XDR 架构 HeadCount.xdr 添加到 XmlSchemaCollection  中：

```vb
Imports System.IO
Imports System.Xml
Imports System.Xml.Schema

Namespace ValidationSample

    Class Sample

        Public Shared Sub Main()
            Dim tr As New XmlTextReader("HeadCount.xml")
            Dim vr As New XmlValidatingReader(tr)

            vr.Schemas.Add("xdrHeadCount", "HeadCount.xdr")
            vr.ValidationType = ValidationType.XDR
            AddHandler vr.ValidationEventHandler, AddressOf ValidationHandler

            While vr.Read()
                PrintTypeInfo(vr)
                If vr.NodeType = XmlNodeType.Element Then
                    While vr.MoveToNextAttribute()
                        PrintTypeInfo(vr)
                    End While
                End If
            End While
            Console.WriteLine("Validation finished")
        End Sub

        Public Shared Sub PrintTypeInfo(vr As XmlValidatingReader)
            If vr.SchemaType IsNot Nothing Then
                If TypeOf vr.SchemaType Is XmlSchemaDatatype Or TypeOf vr.SchemaType Is XmlSchemaSimpleType Then
                    Dim value As Object = vr.ReadTypedValue()
                    Console.WriteLine($"{vr.NodeType}({vr.Name},{value.GetType().Name}):{value}")
                End If
            End If
        End Sub

        Public Shared Sub ValidationHandler(sender As Object, args As ValidationEventArgs)
            Console.WriteLine("**_Validation error")
            Console.WriteLine($"Severity:{args.Severity}")
            Console.WriteLine($"Message:{args.Message}")
        End Sub
    End Class
End Namespace
```

```csharp
using System;
using System.IO;
using System.Xml;
using System.Xml.Schema;

namespace ValidationSample
{
    class Sample
    {
        public static void Main()
        {
            var tr = new XmlTextReader("HeadCount.xml");
            var vr = new XmlValidatingReader(tr);

            vr.Schemas.Add("xdrHeadCount", "HeadCount.xdr");
            vr.ValidationType = ValidationType.XDR;
            vr.ValidationEventHandler += new ValidationEventHandler (ValidationHandler);

            while(vr.Read())
            {
                PrintTypeInfo(vr);
                if (vr.NodeType == XmlNodeType.Element)
                {
                   while(vr.MoveToNextAttribute())
                       PrintTypeInfo(vr);
                }
            }
            Console.WriteLine("Validation finished");
        }

        public static void PrintTypeInfo(XmlValidatingReader vr)
        {
            if (vr.SchemaType != null)
            {
                if(vr.SchemaType is XmlSchemaDatatype || vr.SchemaType is XmlSchemaSimpleType)
                {
                    object value = vr.ReadTypedValue();
                    Console.WriteLine($"{vr.NodeType}({vr.Name},{value.GetType().Name}):{value}");
                }
            }
        }

        public static void ValidationHandler(object sender, ValidationEventArgs args)
        {
            Console.WriteLine("_*_Validation error");
            Console.WriteLine($"\tSeverity:{args.Severity}");
            Console.WriteLine($"\tMessage:{args.Message}");
        }
    }
}
```

下面概括了要验证的输入文件 HeadCount.xml 的内容：

```xml
<!--Load HeadCount.xdr in SchemaCollection for Validation-->
<HeadCount xmlns='xdrHeadCount'>
   <Name>Waldo Pepper</Name>
   <Name>Red Pepper</Name>
</HeadCount>
```

以下概述了要根据其进行验证的 XDR 架构文件 HeadCount.xdr 的内容  ：

```xml
<Schema xmlns="urn:schemas-microsoft-com:xml-data" xmlns:dt="urn:schemas-microsoft-com:datatypes">
   <ElementType name="Name" content="textOnly"/>
   <AttributeType name="Bldg" default="2"/>
   <ElementType name="HeadCount" content="eltOnly">
      <element type="Name"/>
      <attribute type="Bldg"/>
   </ElementType>
</Schema>
```

## <a name="see-also"></a>请参阅

- <xref:System.Xml.XmlValidatingReader.ValidationType%2A>
- [XmlSchemaCollection 架构编译](xmlschemacollection-schema-compilation.md)
