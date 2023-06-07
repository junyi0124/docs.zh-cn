---
title: DataContractResolver
ms.date: 03/30/2017
ms.assetid: 6c200c02-bc14-4b8d-bbab-9da31185b805
ms.openlocfilehash: 6c04a055a5ce87ac285d05b086c22b900bc92145
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96253631"
---
# <a name="datacontractresolver"></a>DataContractResolver

此示例演示如何使用 <xref:System.Runtime.Serialization.DataContractResolver> 类来自定义序列化和反序列化过程。 此示例演示在序列化和反序列化过程中，如何使用 DataContractResolver 在 CLR 类型与 xsi:type 表示形式之间进行映射。

## <a name="sample-details"></a>示例详细信息

 此示例定义以下 CLR 类型。

```csharp
using System;
using System.Runtime.Serialization;

namespace Types
{
    [DataContract]
    public class Customer
    {
        [DataMember]
        public string Name { get; set; }
    }

    [DataContract]
    public class VIPCustomer : Customer
    {
        [DataMember]
        public string VipInfo { get; set; }
    }

    [DataContract]
    public class RegularCustomer : Customer
    {
    }

    [DataContract]
    public class PreferredVIPCustomer : VIPCustomer
    {
    }
}
```

 此示例加载程序集，提取每个类型，然后对这些类型进行序列化和反序列化。 <xref:System.Runtime.Serialization.DataContractResolver> 会通过将 <xref:System.Runtime.Serialization.DataContractResolver> 派生类的实例传递给 <xref:System.Runtime.Serialization.DataContractSerializer> 构造函数，插入到序列化过程中，如下面的示例所示。

```csharp
this.serializer = new DataContractSerializer(typeof(Object), null, int.MaxValue, false, true, null, new MyDataContractResolver(assembly));
```

 此示例随后序列化 CLR 类型，如下面的代码示例所示。

```csharp
Assembly assembly = Assembly.Load(new AssemblyName("Types"));

public void serialize(Type type)
{
    Object instance = Activator.CreateInstance(type);

    Console.WriteLine("----------------------------------------");
    Console.WriteLine();
    Console.WriteLine("Serializing type: {0}", type.Name);
    Console.WriteLine();
    this.buffer = new StringBuilder();
    using (XmlWriter xmlWriter = XmlWriter.Create(this.buffer))
    {
        try
        {
            this.serializer.WriteObject(xmlWriter, instance);
        }
        catch (SerializationException error)
        {
            Console.WriteLine(error.ToString());
        }
    }
    Console.WriteLine(this.buffer.ToString());
}
```

 此示例随后反序列化 xsi:type，如下面的代码示例所示。

```csharp
public void deserialize(Type type)
{
    Console.WriteLine();
    Console.WriteLine("Deserializing type: {0}", type.Name);
    Console.WriteLine();
    using (XmlReader xmlReader = XmlReader.Create(new StringReader(this.buffer.ToString())))
    {
        Object obj = this.serializer.ReadObject(xmlReader);
    }
}
```

 由于会将自定义 <xref:System.Runtime.Serialization.DataContractResolver> 传入到 <xref:System.Runtime.Serialization.DataContractSerializer> 构造函数中，因此在序列化过程中会调用 <xref:System.Runtime.Serialization.DataContractResolver.TryResolveType%2A>，以将 CLR 类型映射到等效的 `xsi:type`。 同样，会在反序列化过程中调用 <xref:System.Runtime.Serialization.DataContractResolver.ResolveName%2A>，以将 `xsi:type` 映射到等效的 CLR 类型。 在此示例中定义了 <xref:System.Runtime.Serialization.DataContractResolver>，如下面的示例所示。

 下面的代码示例是一个派生自 <xref:System.Runtime.Serialization.DataContractResolver> 的类。

```csharp
class MyDataContractResolver : DataContractResolver
{
    private Dictionary<string, XmlDictionaryString> dictionary = new Dictionary<string, XmlDictionaryString>();
    Assembly assembly;

    public MyDataContractResolver(Assembly assembly)
    {
        this.assembly = assembly;
    }

    // Used at deserialization
    // Allows users to map xsi:type name to any Type
    public override Type ResolveName(string typeName, string typeNamespace, DataContractResolver knownTypeResolver)
    {
        XmlDictionaryString tName;
        XmlDictionaryString tNamespace;
        if (dictionary.TryGetValue(typeName, out tName) && dictionary.TryGetValue(typeNamespace, out tNamespace))
        {
            return this.assembly.GetType(tNamespace.Value + "." + tName.Value);
        }
        else
        {
            return null;
        }
    }

    // Used at serialization
    // Maps any Type to a new xsi:type representation
    public override void ResolveType(Type dataContractType, DataContractResolver knownTypeResolver, out XmlDictionaryString typeName, out XmlDictionaryString typeNamespace)
    {
        string name = dataContractType.Name;
        string namesp = dataContractType.Namespace;
        typeName = new XmlDictionaryString(XmlDictionary.Empty, name, 0);
        typeNamespace = new XmlDictionaryString(XmlDictionary.Empty, namesp, 0);
        if (!dictionary.ContainsKey(dataContractType.Name))
        {
            dictionary.Add(name, typeName);
        }
        if (!dictionary.ContainsKey(dataContractType.Namespace))
        {
            dictionary.Add(namesp, typeNamespace);
        }
    }
}
```

 作为此示例的一部分，Types 项目将生成具有此示例中所使用的全部类型的程序集。 使用此项目可添加、移除或修改将要序列化的所有类型。

#### <a name="to-use-this-sample"></a>使用此示例

1. 使用 Visual Studio 2012 打开 DCRSample 解决方案文件。

2. 若要运行解决方案，请按 F5。

> [!IMPORTANT]
> 您的计算机上可能已安装这些示例。 在继续操作之前，请先检查以下（默认）目录：  
>
> `<InstallDrive>:\WF_WCF_Samples`  
>
> 如果此目录不存在，请参阅[Windows Communication Foundation (wcf) ，并 Windows Workflow Foundation (的 WF](https://www.microsoft.com/download/details.aspx?id=21459)) .NET Framework Windows Communication Foundation ([!INCLUDE[wf1](../../../../includes/wf1-md.md)] 此示例位于以下目录：  
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Contract\Data\DataContractResolver`  
  
## <a name="see-also"></a>另请参阅

- [使用数据协定解析程序](../feature-details/using-a-data-contract-resolver.md)
