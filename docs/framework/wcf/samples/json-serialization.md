---
title: DataContractJsonSerializer 示例
ms.date: 03/30/2017
ms.assetid: 3c2c4747-7510-4bdf-b4fe-64f98428ef4a
ms.openlocfilehash: 4aa0ee679ae424251000b14abfbacf0590a6ccd3
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/09/2020
ms.locfileid: "84592016"
---
# <a name="datacontractjsonserializer-sample"></a>DataContractJsonSerializer 示例

> [!NOTE]
> 此示例适用于 <xref:System.Runtime.Serialization.Json.DataContractJsonSerializer> 。 对于涉及序列化和反序列化 JSON 的大多数方案，我们建议在[system.web 命名空间](../../../standard/serialization/system-text-json-overview.md)中提供 api。

<xref:System.Runtime.Serialization.Json.DataContractJsonSerializer> 支持同一类型，如 <xref:System.Runtime.Serialization.DataContractSerializer>。 JSON 数据格式在编写异步 JavaScript 和 XML (AJAX) 样式的 Web 应用程序时特别有用。 Windows Communication Foundation （WCF）中的 AJAX 支持经过优化，可通过 ScriptManager 控件与 ASP.NET AJAX 一起使用。 有关如何将 Windows Communication Foundation （WCF）与 ASP.NET AJAX 一起使用的示例，请参阅[AJAX 示例](ajax.md)。  
  
本主题的最后介绍了此示例的设置过程和生成说明。  
  
此示例使用 `Person` 数据协定演示序列化和反序列化。  

```csharp
[DataContract]
class Person
{
    [DataMember]
    internal string name;

    [DataMember]
    internal int age;
}
```

 若要将 `Person` 类型的实例序列化为 JSON，首先创建 <xref:System.Runtime.Serialization.Json.DataContractJsonSerializer> 并使用 `WriteObject` 方法将 JSON 数据编写成流。  

```csharp
Person p = new Person();
//Set up Person object...
MemoryStream stream1 = new MemoryStream();
DataContractJsonSerializer ser = new DataContractJsonSerializer(typeof(Person));
ser.WriteObject(stream1, p);
```

 内存流包含有效的 JSON 数据。
  
```json  
{"age":42,"name":"John"}  
```  
  
 此示例演示从 JSON 数据反序列化为对象。 然后重绕流并调用 `ReadObject`。  

```csharp
Person p2 = (Person)ser.ReadObject(stream1);
```

 检查 `p2` 对象显示 JSON 数据已正确反序列化。  
  
> [!IMPORTANT]
> 您的计算机上可能已安装这些示例。 在继续操作之前，请先检查以下（默认）目录：  
>
> `<InstallDrive>:\WF_WCF_Samples`  
>
> 如果此目录不存在，请参阅[.NET Framework 4 的 Windows Communication Foundation （wcf）和 Windows Workflow Foundation （WF）示例](https://www.microsoft.com/download/details.aspx?id=21459)以下载所有 WINDOWS COMMUNICATION FOUNDATION （wcf）和 [!INCLUDE[wf1](../../../../includes/wf1-md.md)] 示例。 此示例位于以下目录：  
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Ajax\JsonSerialization`  
  
#### <a name="to-set-up-build-and-run-the-sample"></a>设置、生成和运行示例  
  
1. 按照[生成 Windows Communication Foundation 示例](building-the-samples.md)中所述生成解决方案 JsonSerialization。  
  
2. 运行生成的控制台应用程序。  
