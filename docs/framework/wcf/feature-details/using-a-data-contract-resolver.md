---
title: 使用数据协定解析程序
ms.date: 03/30/2017
ms.assetid: 2e68a16c-36f0-4df4-b763-32021bff2b89
ms.openlocfilehash: a86ad21a5846feec37f8b4b48843eab2d6c161da
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96289628"
---
# <a name="using-a-data-contract-resolver"></a>使用数据协定解析程序

使用数据协定解析程序可以动态配置已知类型。 序列化或反序列化并非数据协定所需的类型时，要求提供已知类型。 有关已知类型的详细信息，请参阅 [数据协定已知类型](data-contract-known-types.md)。 已知类型通常以静态方式指定。 这意味着您必须了解在实现某个操作期间，该操作可能接收的所有可能类型。 在某些方案中无法做到这一点，因此能够以动态方式指定已知类型十分重要。  
  
## <a name="creating-a-data-contract-resolver"></a>创建数据协定解析程序  

 创建数据协定解析程序涉及到实现两个方法：<xref:System.Runtime.Serialization.DataContractResolver.TryResolveType%2A> 和 <xref:System.Runtime.Serialization.DataContractResolver.ResolveName%2A>。 这两个方法分别实现在序列化和反序列化期间使用的回调。 在序列化期间将调用 <xref:System.Runtime.Serialization.DataContractResolver.TryResolveType%2A> 方法，用于获取数据协定类型并将其映射到 `xsi:type` 名称和命名空间。 在反序列化期间将调用 <xref:System.Runtime.Serialization.DataContractResolver.ResolveName%2A> 方法，用于获取 `xsi:type` 名称和命名空间并将其解析为数据协定类型。 这两个方法均具有 `knownTypeResolver` 参数，该参数可用于在实现中使用默认已知类型解析程序。  
  
 下面的示例演示了如何实现 <xref:System.Runtime.Serialization.DataContractResolver>，以映射到派生自数据协定类型 `Customer` 的数据协定类型 `Person`，或者从后一个数据协定类型进行映射。  
  
```csharp  
public class MyCustomerResolver : DataContractResolver  
{  
    public override bool TryResolveType(Type dataContractType, Type declaredType, DataContractResolver knownTypeResolver, out XmlDictionaryString typeName, out XmlDictionaryString typeNamespace)  
    {  
        if (dataContractType == typeof(Customer))  
        {  
            XmlDictionary dictionary = new XmlDictionary();  
            typeName = dictionary.Add("SomeCustomer");  
            typeNamespace = dictionary.Add("http://tempuri.com");  
            return true;  
        }  
        else  
        {  
            return knownTypeResolver.TryResolveType(dataContractType, declaredType, null, out typeName, out typeNamespace);  
        }  
    }  
  
    public override Type ResolveName(string typeName, string typeNamespace, DataContractResolver knownTypeResolver)  
    {  
        if (typeName == "SomeCustomer" && typeNamespace == "http://tempuri.com")  
        {  
            return typeof(Customer);  
        }  
        else  
        {  
            return knownTypeResolver.ResolveName(typeName, typeNamespace, null);  
        }  
    }  
}  
```  
  
 一旦定义了 <xref:System.Runtime.Serialization.DataContractResolver>，即可将它传递到 <xref:System.Runtime.Serialization.DataContractSerializer> 构造函数加以使用，如下面的示例所示。  
  
```csharp
XmlObjectSerializer serializer = new DataContractSerializer(typeof(Customer), null, Int32.MaxValue, false, false, null, new MyCustomerResolver());  
```  
  
 可以在对 <xref:System.Runtime.Serialization.DataContractResolver> 或 <xref:System.Runtime.Serialization.DataContractSerializer.ReadObject%2A?displayProperty=nameWithType> 方法的调用中指定  <xref:System.Runtime.Serialization.DataContractSerializer.WriteObject%2A?displayProperty=nameWithType>，如下面的示例所示。  
  
```csharp
MemoryStream ms = new MemoryStream();  
DataContractSerializer serializer = new DataContractSerializer(typeof(Customer));  
XmlDictionaryWriter writer = XmlDictionaryWriter.CreateDictionaryWriter(XmlWriter.Create(ms));  
serializer.WriteObject(writer, new Customer(), new MyCustomerResolver());  
writer.Flush();  
ms.Position = 0;  
Console.WriteLine(((Customer)serializer.ReadObject(XmlDictionaryReader.CreateDictionaryReader(XmlReader.Create(ms)), false, new MyCustomerResolver()));  
```  
  
 或者，可以在 <xref:System.ServiceModel.Description.DataContractSerializerOperationBehavior> 上设置该构造函数，如下面的示例所示。  
  
```csharp
ServiceHost host = new ServiceHost(typeof(MyService));  
  
ContractDescription cd = host.Description.Endpoints[0].Contract;  
OperationDescription myOperationDescription = cd.Operations.Find("Echo");  
  
DataContractSerializerOperationBehavior serializerBehavior = myOperationDescription.Behaviors.Find<DataContractSerializerOperationBehavior>();  
if (serializerBehavior == null)  
{  
    serializerBehavior = new DataContractSerializerOperationBehavior(myOperationDescription);  
    myOperationDescription.Behaviors.Add(serializerBehavior);  
}  
  
SerializerBehavior.DataContractResolver = new MyCustomerResolver();  
```  
  
 通过实现可以应用于服务的特性，可以通过声明方式指定数据协定解析程序。  有关详细信息，请参阅 [KnownAssemblyAttribute](../samples/knownassemblyattribute.md) 示例。 此示例实现一个名为 "KnownAssembly" 的属性，该属性将自定义数据协定解析程序添加到服务的行为。  
  
## <a name="see-also"></a>另请参阅

- [数据协定已知类型](data-contract-known-types.md)
- [DataContractSerializer 示例](../samples/datacontractserializer-sample.md)
- [KnownAssemblyAttribute](../samples/knownassemblyattribute.md)
