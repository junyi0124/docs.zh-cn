---
title: 使用 DataContractSerializer 和 DataContractResolver 实现 NetDataContractSerializer 的功能
ms.date: 03/30/2017
ms.assetid: 1376658f-f695-45f7-a7e0-94664e9619ff
ms.openlocfilehash: 002cd101ddf760e28a71ded0d6a7f4ee29a56c3b
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96253604"
---
# <a name="using-datacontractserializer-and-datacontractresolver-to-provide-the-functionality-of-netdatacontractserializer"></a>使用 DataContractSerializer 和 DataContractResolver 实现 NetDataContractSerializer 的功能

此示例演示如何将 <xref:System.Runtime.Serialization.DataContractSerializer> 与相应的 <xref:System.Runtime.Serialization.DataContractResolver> 结合使用来提供与 <xref:System.Runtime.Serialization.NetDataContractSerializer> 相同的功能。 此示例演示如何创建相应的 <xref:System.Runtime.Serialization.DataContractResolver> 以及如何将其添加到 <xref:System.Runtime.Serialization.DataContractSerializer>。

## <a name="sample-details"></a>示例详细信息

 <xref:System.Runtime.Serialization.NetDataContractSerializer> 与 <xref:System.Runtime.Serialization.DataContractSerializer> 之间存在一个重要区别：<xref:System.Runtime.Serialization.NetDataContractSerializer> 在序列化的 XML 中包含 CLR 类型信息，而 <xref:System.Runtime.Serialization.DataContractSerializer> 不包含这种信息。 因此，只有在序列化和反序列化端共享相同的 CLR 类型时，才能使用 <xref:System.Runtime.Serialization.NetDataContractSerializer>。 但是，建议使用 <xref:System.Runtime.Serialization.DataContractSerializer>，因为其性能比 <xref:System.Runtime.Serialization.NetDataContractSerializer> 更佳。 通过将 <xref:System.Runtime.Serialization.DataContractSerializer> 添加到 <xref:System.Runtime.Serialization.DataContractResolver>，可以更改在其中序列化的信息。

 此示例由两个项目组成。 第一个项目使用 <xref:System.Runtime.Serialization.NetDataContractSerializer> 来序列化对象。 第二个项目将 <xref:System.Runtime.Serialization.DataContractSerializer> 与 <xref:System.Runtime.Serialization.DataContractResolver> 结合使用，以提供与第一个项目相同的功能。

 以下代码示例演示如何实现名为 <xref:System.Runtime.Serialization.DataContractResolver> 的自定义 `MyDataContractResolver`，后者将添加到 DCSwithDCR 项目中的  <xref:System.Runtime.Serialization.DataContractSerializer>。

```csharp
class MyDataContractResolver : DataContractResolver
{
    private XmlDictionary dictionary = new XmlDictionary();

    public MyDataContractResolver()
    {
    }

    // Used at deserialization
    // Allows users to map xsi:type name to any Type
    public override Type ResolveName(string typeName, string typeNamespace, DataContractResolver knownTypeResolver)
    {
        Type type = knownTypeResolver.ResolveName(typeName, typeNamespace, null);
        type ??= Type.GetType(typeName + ", " + typeNamespace);
        return type;
    }

    // Used at serialization
    // Maps any Type to a new xsi:type representation
    public override void ResolveType(Type dataContractType, DataContractResolver knownTypeResolver, out XmlDictionaryString typeName, out XmlDictionaryString typeNamespace)
    {
        knownTypeResolver.ResolveType(dataContractType, null, out typeName, out typeNamespace);
        if (typeName == null || typeNamespace == null)
        {
            XmlDictionary dictionary = new XmlDictionary();
            typeName = dictionary.Add(dataContractType.FullName);
            typeNamespace = dictionary.Add(dataContractType.Assembly.FullName);
        }
    }
}
```

#### <a name="to-use-this-sample"></a>使用此示例

1. 使用 Visual Studio 2012 打开 DCRSample 解决方案文件。

2. 右键单击该解决方案文件，然后选择 " **属性**"。

3. 在 " **解决方案属性页** " 对话框中，在 " **通用属性**、 **启动项目**" 下，选择 " **多个启动项目：**"。

4. 在 " **DCSwithDCR** " 项目旁边，从 "**操作**" 下拉列表中选择 "**启动**"。

5. 在 " **NetDCS** " 项目旁边，从 "**操作**" 下拉列表中选择 "**启动**"。

6. 单击 **“确定”**，关闭对话框。

7. 要生成解决方案，按 Ctrl+Shift+B。

8. 若要运行解决方案，请按 Ctrl+F5。

> [!IMPORTANT]
> 您的计算机上可能已安装这些示例。 在继续操作之前，请先检查以下（默认）目录：  
>
> `<InstallDrive>:\WF_WCF_Samples`  
>
> 如果此目录不存在，请参阅[Windows Communication Foundation (wcf) ，并 Windows Workflow Foundation (的 WF](https://www.microsoft.com/download/details.aspx?id=21459)) .NET Framework Windows Communication Foundation ([!INCLUDE[wf1](../../../../includes/wf1-md.md)] 此示例位于以下目录：  
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Contract\Data\NetDcSasDcSwithDCR`  
