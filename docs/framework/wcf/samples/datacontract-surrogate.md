---
title: DataContract 代理项
ms.date: 03/30/2017
ms.assetid: b0188f3c-00a9-4cf0-a887-a2284c8fb014
ms.openlocfilehash: 23a83b0a99a17af011bea1193c2ea013a2c3c5a9
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96253682"
---
# <a name="datacontract-surrogate"></a>DataContract 代理项

本示例演示如何使用数据协定代理类自定义诸如序列化、反序列化、架构导出和架构导入之类的过程。 此示例演示如何在客户端和服务器方案中使用代理项，其中数据在 Windows Communication Foundation (WCF) 客户端和服务之间进行序列化和传输。  
  
> [!NOTE]
> 本主题的最后介绍了此示例的设置过程和生成说明。  
  
 此示例使用下面的服务协定：  
  
```csharp
[ServiceContract(Namespace = "http://Microsoft.ServiceModel.Samples")]  
[AllowNonSerializableTypes]  
public interface IPersonnelDataService  
{  
    [OperationContract]  
    void AddEmployee(Employee employee);  
  
    [OperationContract]  
    Employee GetEmployee(string name);  
}  
```  
  
 `AddEmployee` 操作允许用户添加有关新雇员的数据，`GetEmployee` 操作支持按姓名搜索雇员。  
  
 这些操作使用下面的数据类型：  
  
```csharp
[DataContract(Namespace = "http://Microsoft.ServiceModel.Samples")]  
class Employee  
{  
    [DataMember]  
    public DateTime dateHired;  
  
    [DataMember]  
    public Decimal salary;  
  
    [DataMember]  
    public Person person;  
}  
```  
  
 在 `Employee` 类型中，`Person` 类（显示在下面的示例代码中）不能由 <xref:System.Runtime.Serialization.DataContractSerializer> 序列化，因为它不是有效的数据协定类。  
  
```csharp
public class Person  
{  
    public string firstName;  
  
    public string lastName;  
  
    public int age;  
  
    public Person() { }  
}  
```  
  
 可以将 <xref:System.Runtime.Serialization.DataContractAttribute> 属性应用于 `Person` 类，但不是始终都可以这样做。 例如，`Person` 类可能是在您无法控制的独立程序集中定义的。  
  
 在此限制下，序列化 `Person` 类的一种方式是用标记为 <xref:System.Runtime.Serialization.DataContractAttribute> 的另一个类替换此类并将必要数据复制到新类中。 目标是使 `Person` 类显示为 <xref:System.Runtime.Serialization.DataContractSerializer> 的 DataContract。 请注意，这是序列化非数据协定类的一种方式。  
  
 本示例通过逻辑方式用名为 `Person` 的另一个类替换 `PersonSurrogated` 类。  
  
```csharp
[DataContract(Name="Person", Namespace = "http://Microsoft.ServiceModel.Samples")]  
public class PersonSurrogated  
{  
    [DataMember]  
    public string FirstName;  
  
    [DataMember]  
    public string LastName;  
  
    [DataMember]  
    public int Age;  
}  
```  
  
 数据协定代理项用于实现此替换。 数据协定代理项是实现 <xref:System.Runtime.Serialization.IDataContractSurrogate> 的类。 在本示例中，`AllowNonSerializableTypesSurrogate` 类实现此接口。  
  
 在接口实现中，第一项任务是建立从 `Person` 到 `PersonSurrogated` 的类型映射。 序列化时和架构导出时都使用此映射。 此映射通过实现 <xref:System.Runtime.Serialization.IDataContractSurrogate.GetDataContractType%28System.Type%29> 方法来实现。  
  
```csharp
public Type GetDataContractType(Type type)  
{  
    if (typeof(Person).IsAssignableFrom(type))  
    {  
        return typeof(PersonSurrogated);  
    }  
    return type;  
}  
```  
  
 在序列化过程中，<xref:System.Runtime.Serialization.IDataContractSurrogate.GetObjectToSerialize%28System.Object%2CSystem.Type%29> 方法将 `Person` 实例映射到 `PersonSurrogated` 实例，如下面的示例代码所示。  
  
```csharp
public object GetObjectToSerialize(object obj, Type targetType)  
{  
    if (obj is Person)  
    {  
        Person person = (Person)obj;  
        PersonSurrogated personSurrogated = new PersonSurrogated();  
        personSurrogated.FirstName = person.firstName;  
        personSurrogated.LastName = person.lastName;  
        personSurrogated.Age = person.age;  
        return personSurrogated;  
    }  
    return obj;  
}  
```  
  
 <xref:System.Runtime.Serialization.IDataContractSurrogate.GetDeserializedObject%28System.Object%2CSystem.Type%29> 方法为反序列化提供反向映射，如下面的示例代码所示。  
  
```csharp
public object GetDeserializedObject(object obj,
Type targetType)  
{  
    if (obj is PersonSurrogated)  
    {  
        PersonSurrogated personSurrogated = (PersonSurrogated)obj;  
        Person person = new Person();  
        person.firstName = personSurrogated.FirstName;  
        person.lastName = personSurrogated.LastName;  
        person.age = personSurrogated.Age;  
        return person;  
    }  
    return obj;  
}  
```  
  
 为了在架构导如过程中将 `PersonSurrogated` 数据协定映射到现有 `Person` 类，本示例实现 <xref:System.Runtime.Serialization.IDataContractSurrogate.GetReferencedTypeOnImport%28System.String%2CSystem.String%2CSystem.Object%29> 方法，如下面的示例代码所示。  
  
```csharp
public Type GetReferencedTypeOnImport(string typeName,
               string typeNamespace, object customData)  
{  
if (  
typeNamespace.Equals("http://schemas.datacontract.org/2004/07/DCSurrogateSample")  
)  
    {  
         if (typeName.Equals("PersonSurrogated"))  
        {  
             return typeof(Person);  
        }  
     }  
     return null;  
}  
```  
  
 下面的示例代码完成 <xref:System.Runtime.Serialization.IDataContractSurrogate> 接口的实现。  
  
```csharp
public System.CodeDom.CodeTypeDeclaration ProcessImportedType(  
          System.CodeDom.CodeTypeDeclaration typeDeclaration,
          System.CodeDom.CodeCompileUnit compileUnit)  
{  
    return typeDeclaration;  
}  
public object GetCustomDataToExport(Type clrType,
                               Type dataContractType)  
{  
    return null;  
}  
  
public object GetCustomDataToExport(  
System.Reflection.MemberInfo memberInfo, Type dataContractType)  
{  
    return null;  
}  
public void GetKnownCustomDataTypes(  
        KnownTypeCollection customDataTypes)  
{  
    // It does not matter what we do here.  
    throw new NotImplementedException();  
}  
```  
  
 在本示例中，由一个名为 `AllowNonSerializableTypesAttribute` 的属性在 ServiceModel 中启用该代理项。 开发人员需要在他们的服务协定上应用此属性，如上面的 `IPersonnelDataService` 服务协定所示。 此属性实现 `IContractBehavior` 并在其 `ApplyClientBehavior` 和 `ApplyDispatchBehavior` 方法中对操作设置该代理项。  
  
 本例中该属性并不是必要的，它在此示例中仅用于演示目的。 用户也可以使用代码或使用配置，手动添加类似的 `IContractBehavior`、`IEndpointBehavior` 或 `IOperationBehavior` 来启用代理项。  
  
 `IContractBehavior` 实现通过检查操作是否已注册 `DataContractSerializerOperationBehavior` 来查找使用 DataContract 的操作。 如果已注册，则对该行为设置 `DataContractSurrogate` 属性。 下面的示例代码演示如何完成以上过程。 在此操作行为上设置代理项可以为序列化和反序列化启用该代理项。  
  
```csharp
public void ApplyClientBehavior(ContractDescription description, ServiceEndpoint endpoint, System.ServiceModel.Dispatcher.ClientRuntime proxy)  
{  
    foreach (OperationDescription opDesc in description.Operations)  
    {  
        ApplyDataContractSurrogate(opDesc);  
    }  
}  
  
public void ApplyDispatchBehavior(ContractDescription description, ServiceEndpoint endpoint, System.ServiceModel.Dispatcher.DispatchRuntime dispatch)  
{  
    foreach (OperationDescription opDesc in description.Operations)  
    {  
        ApplyDataContractSurrogate(opDesc);  
    }  
}  
  
private static void ApplyDataContractSurrogate(OperationDescription description)  
{  
    DataContractSerializerOperationBehavior dcsOperationBehavior = description.Behaviors.Find<DataContractSerializerOperationBehavior>();  
    if (dcsOperationBehavior != null)  
    {  
        if (dcsOperationBehavior.DataContractSurrogate == null)  
            dcsOperationBehavior.DataContractSurrogate = new AllowNonSerializableTypesSurrogate();  
    }  
}  
```  
  
 需要采取附加步骤才能插入元数据生成期间所要使用的代理项。 完成此过程的一种机制是提供本示例所演示的 `IWsdlExportExtension`。 另一种方式是直接修改 `WsdlExporter`。  
  
 `AllowNonSerializableTypesAttribute`特性实现 `IWsdlExportExtension` 和 `IContractBehavior` 。 `IContractBehavior` `IEndpointBehavior` 在这种情况下，扩展可以是或。 其 `IWsdlExportExtension.ExportContract` 方法实现通过将代理项添加到为 DataContract 生成架构的过程中使用的`XsdDataContractExporter` 来启用该代理项。 下面的代码段演示如何执行此操作。  
  
```csharp
public void ExportContract(WsdlExporter exporter, WsdlContractConversionContext context)  
{  
    if (exporter == null)  
        throw new ArgumentNullException("exporter");  
  
    object dataContractExporter;  
    XsdDataContractExporter xsdDCExporter;  
    if (!exporter.State.TryGetValue(typeof(XsdDataContractExporter), out dataContractExporter))  
    {  
        xsdDCExporter = new XsdDataContractExporter(exporter.GeneratedXmlSchemas);  
        exporter.State.Add(typeof(XsdDataContractExporter), xsdDCExporter);  
    }  
    else  
    {  
        xsdDCExporter = (XsdDataContractExporter)dataContractExporter;  
    }  
    if (xsdDCExporter.Options == null)  
        xsdDCExporter.Options = new ExportOptions();  
  
    if (xsdDCExporter.Options.DataContractSurrogate == null)  
        xsdDCExporter.Options.DataContractSurrogate = new AllowNonSerializableTypesSurrogate();  
}  
```  
  
 运行示例时，客户端将调用 AddEmployee，然后调用 GetEmployee 以检查第一个调用是否成功。 GetEmployee 操作请求的结果显示在客户端控制台窗口中。 若要查找员工并打印 "找到"，GetEmployee 操作必须成功。  
  
> [!NOTE]
> 本示例演示如何插入用于序列化、反序列化和元数据生成的代理项。 示例不演示如何插入用于从元数据中生成代码的代理项。 若要查看如何使用代理项插入客户端代码生成的示例，请参阅 [自定义 WSDL 发布](custom-wsdl-publication.md) 示例。  
  
### <a name="to-set-up-build-and-run-the-sample"></a>设置、生成和运行示例  
  
1. 确保已对 [Windows Communication Foundation 示例执行了一次性安装过程](one-time-setup-procedure-for-the-wcf-samples.md)。  
  
2. 若要生成 c # 版本的解决方案，请按照 [生成 Windows Communication Foundation 示例](building-the-samples.md)中的说明进行操作。  
  
3. 若要以单机配置或跨计算机配置来运行示例，请按照 [运行 Windows Communication Foundation 示例](running-the-samples.md)中的说明进行操作。  
  
> [!IMPORTANT]
> 您的计算机上可能已安装这些示例。 在继续操作之前，请先检查以下（默认）目录：  
>
> `<InstallDrive>:\WF_WCF_Samples`  
>
> 如果此目录不存在，请参阅[Windows Communication Foundation (wcf) ，并 Windows Workflow Foundation (的 WF](https://www.microsoft.com/download/details.aspx?id=21459)) .NET Framework Windows Communication Foundation ([!INCLUDE[wf1](../../../../includes/wf1-md.md)] 此示例位于以下目录：  
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Extensibility\DataContract`  
