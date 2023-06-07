---
title: 弱类型 JSON 序列化示例
ms.date: 03/30/2017
ms.assetid: 0b30e501-4ef5-474d-9fad-a9d559cf9c52
ms.openlocfilehash: 65330e77622920f02b12bd69348aa635529e030e
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96244426"
---
# <a name="weakly-typed-json-serialization-sample"></a>弱类型 JSON 序列化示例

将用户定义的类型序列化为给定的连网格式，或者将连网格式反序列为原来的用户定义的类型时，给定的用户定义的类型必须在服务和客户端上可用。 通常，为实现此目的，系统将 <xref:System.Runtime.Serialization.DataContractAttribute> 属性应用于这些用户定义的类型，并将 <xref:System.Runtime.Serialization.DataMemberAttribute> 属性应用于这些类型的成员。 处理 JavaScript 对象符号 (JSON) 对象时，该机制同样适用，如主题 [How to: Serialize and Deserialize JSON Data](../feature-details/how-to-serialize-and-deserialize-json-data.md)中所述。  
  
 在某些情况下，Windows Communication Foundation (WCF) 服务或客户端必须访问由不受开发人员控制的服务或客户端生成的 JSON 对象。 随着越来越多的 Web 服务公开 JSON Api，WCF 开发人员可能不需要构造任意 JSON 对象要反序列化的本地用户定义类型。 本示例提供了一种机制，使 WCF 开发人员无需创建用户定义的类型即可使用反序列化的任意 JSON 对象。 这称为 JSON 对象的“弱类型序列化”  ，因为 JSON 对象反序列化为的类型在编译时是未知的。  
  
> [!NOTE]
> 本主题的最后介绍了此示例的设置过程和生成说明。  
  
 例如，一个公共 Web 服务 API 返回以下 JSON 对象，该对象描述有关该服务的用户的一些信息。  
  
```json  
{"personal": {"name": "Paul", "age": 23, "height": 1.7, "isSingle": true, "luckyNumbers": [5,17,21]}, "favoriteBands": ["Band ABC", "Band XYZ"]}  
```  
  
 若要反序列化此对象，WCF 客户端必须实现以下用户定义的类型。  
  
```csharp  
[DataContract]  
 public class MemberProfile  
 {  
     [DataMember]  
     public PersonalInfo personal;  
  
     [DataMember]  
     public string[] favoriteBands;  
 }  
  
 [DataContract]  
 public class PersonalInfo  
 {  
     [DataMember]  
     public string name;  
  
     [DataMember]  
     public int age;  
  
     [DataMember]  
     public double height;  
  
     [DataMember]  
     public bool isSingle;  
  
     [DataMember]  
     public int[] luckyNumbers;  
 }  
```  
  
 这可能会很麻烦，尤其是客户端必须处理多种 JSON 对象时。  
  
 此示例提供的 `JsonObject` 类型引入了以弱类型表示的反序列化的 JSON 对象。 `JsonObject` 依赖于 JSON 对象与 .NET Framework 字典之间的自然映射和 JSON 数组与 .NET Framework 数组之间的映射。 下面的代码演示 `JsonObject` 类型。  
  
```csharp  
// Instantiation of JsonObject json omitted  
  
string name = json["root"]["personal"]["name"];  
int age = json["root"]["personal"]["age"];  
double height = json["root"]["personal"]["height"];  
bool isSingle = json["root"]["personal"]["isSingle"];  
int[] luckyNumbers = {  
                                     json["root"]["personal"]["luckyNumbers"][0],  
                                     json["root"]["personal"]["luckyNumbers"][1],  
                                     json["root"]["personal"]["luckyNumbers"][2]
                                 };  
string[] favoriteBands = {  
                                        json["root"]["favoriteBands"][0],  
                                        json["root"]["favoriteBands"][1]  
                                    };  
```  
  
 请注意，您无需在编译时声明 JSON 对象和数组的类型便可以“浏览”它们。 有关顶级 `["root"]` 对象的要求的说明，请参阅主题 [Mapping Between JSON and XML](../feature-details/mapping-between-json-and-xml.md)中所述。  
  
> [!NOTE]
> `JsonObject` 类仅作为示例提供， 它未经全面测试，不应在生产环境中使用。 弱类型 JSON 序列化的一个明显含义是在处理 `JsonObject`时缺乏类型安全。  
  
 若要使用 `JsonObject` 类型，客户端操作协定必须使用 <xref:System.ServiceModel.Channels.Message> 作为其返回类型。  
  
```csharp  
[ServiceContract]  
    interface IClientSideProfileService  
    {  
        // There is no need to write a DataContract for the complex type returned by the service.  
        // The client will use a JsonObject to browse the JSON in the received message.  
  
        [OperationContract]  
        [WebGet(ResponseFormat = WebMessageFormat.Json)]  
        Message GetMemberProfile();  
    }  
```  
  
 随后实例化 `JsonObject` ，如下面的代码所示。  
  
```csharp  
// Code to instantiate IClientSideProfileService channel omitted…  
  
// Make a request to the service and obtain the Json response  
XmlDictionaryReader reader = channel.GetMemberProfile().GetReaderAtBodyContents();  
  
// Go through the Json as though it is a dictionary. There is no need to map it to a .NET CLR type.  
JsonObject json = new JsonObject(reader);  
```  
  
 `JsonObject` 构造函数采用通过 <xref:System.Xml.XmlDictionaryReader>方法获取的 <xref:System.ServiceModel.Channels.Message.GetReaderAtBodyContents%2A> 。 该读取器包含客户端接收的 JSON 消息的 XML 表示形式。 有关详细信息，请参阅 [JSON 和 XML 之间的映射](../feature-details/mapping-between-json-and-xml.md)主题。  
  
 该程序生成以下输出：  
  
```console  
Service listening at http://localhost:8000/.  
To view the JSON output from the sample, navigate to http://localhost:8000/GetMemberProfile  
This is Paul's page. I am 23 years old and I am 1.7 meters tall.  
I am single.  
My lucky numbers are 5, 17, and 21.  
My favorite bands are Band ABC and Band XYZ.  
```  
  
### <a name="to-set-up-build-and-run-the-sample"></a>设置、生成和运行示例  
  
1. 确保已对 [Windows Communication Foundation 示例执行了一次性安装过程](one-time-setup-procedure-for-the-wcf-samples.md)。  
  
2. 按照在 [Building the Windows Communication Foundation Samples](building-the-samples.md)中所述的方法生成解决方案 WeaklyTypedJson.sln。  
  
3. 运行解决方案。  
  
> [!IMPORTANT]
> 您的计算机上可能已安装这些示例。 在继续操作之前，请先检查以下（默认）目录：  
>
> `<InstallDrive>:\WF_WCF_Samples`  
>
> 如果此目录不存在，请参阅[Windows Communication Foundation (wcf) ，并 Windows Workflow Foundation (的 WF](https://www.microsoft.com/download/details.aspx?id=21459)) .NET Framework Windows Communication Foundation ([!INCLUDE[wf1](../../../../includes/wf1-md.md)] 此示例位于以下目录：  
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Scenario\Ajax\WeaklyTypedJson`  
