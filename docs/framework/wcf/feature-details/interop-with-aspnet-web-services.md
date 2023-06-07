---
title: 与 ASP.NET Web 服务的互操作性
ms.date: 03/30/2017
ms.assetid: 622422f8-6651-442f-b8be-e654a4aabcac
ms.openlocfilehash: cca4e1b3da26d8026d41d0a7107432cdd2960545
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96276628"
---
# <a name="interoperability-with-aspnet-web-services"></a>与 ASP.NET Web 服务的互操作性

通过确保使用这两种技术实现的服务符合 WS-I Basic Profile 1.1 规范，可以实现 ASP.NET Web 服务与 Windows Communication Foundation (WCF) Web 服务之间的互操作性。 符合 WS-I Basic Profile 1.1 的 ASP.NET Web 服务可通过 WCF 系统提供的绑定与 WCF 客户端进行互操作 <xref:System.ServiceModel.BasicHttpBinding> 。  
  
 使用 ASP.NET 2.0 选项将 <xref:System.Web.Services.WebService> 和属性添加 <xref:System.Web.Services.WebMethodAttribute> 到接口而不是类，并编写类来实现接口，如下面的示例代码所示。  
  
```csharp
[WebService]  
public interface IEcho  
{  
    [WebMethod]  
    string Echo(string input);  
}  
  
public class Service : IEcho  
{  
  
   public string Echo(string input)  
   {  
        return input;  
    }  
}  
```  
  
 使用此选项是首选方法，因为具有 <xref:System.Web.Services.WebService> 属性的接口为服务所执行的操作构成了一个协定，在以不同方式实现该协定的各个类中，都可重用该协定。  
  
 避免使用 <xref:System.Web.Services.Protocols.SoapDocumentServiceAttribute> 特性依据 SOAP 消息正文元素的完全限定名称将消息路由到方法，而应使用 `SOAPAction` 标头。 WCF 使用 `SOAPAction` HTTP 标头来路由消息。  
  
 只要为 XML 显式定义了命名空间，则在默认情况下，<xref:System.Xml.Serialization.XmlSerializer> 将类型序列化为的 XML 与 <xref:System.Runtime.Serialization.DataContractSerializer> 将类型序列化为的 XML 在语义上完全相同。 定义与 Web 服务一起使用的数据类型时，如果采用 WCF，则请执行以下操作：  
  
- 使用 .NET Framework 类而不是 XML 架构来定义该类型。  
  
- 仅将 <xref:System.SerializableAttribute> 和 <xref:System.Xml.Serialization.XmlRootAttribute> 添加到该类，使用后者显式定义类型的命名空间。 不要添加 <xref:System.Xml.Serialization> 命名空间中的其他属性来控制如何将 .NET Framework 类转换为 XML。  
  
- 通过采用此方法，以后可将 .NET 类转换为附加了 <xref:System.Runtime.Serialization.DataContractAttribute> 和 <xref:System.Runtime.Serialization.DataMemberAttribute> 的数据协定，而无需为了传输而对类序列化成的 XML 进行重大更改。 ASP.NET Web 服务在消息中使用的类型可以作为数据协定由 WCF 应用程序进行处理，从而实现其他优势，并提高 WCF 应用程序的性能。  
  
 避免使用 Internet Information Services (IIS) 提供的身份验证选项。 WCF 客户端不支持它们。 如果服务必须是安全的，请使用 WCF 提供的选项，因为这些选项非常可靠，并且基于标准协议。  
  
## <a name="performance-impact-caused-by-loading-the-servicemodel-httpmodule"></a>由加载 ServiceModel HttpModule 而导致的性能影响  

 在 .NET Framework 3.0 中，WCF `HttpModule` 安装在根 Web.config 文件中，因此，每个 ASP.NET 应用程序都启用了 WCF。 这可能会影响性能，因此，可以移除 Web.config 文件的 `ServiceModel`，如下面的示例所示。  
  
```xml  
<httpModules>  
    <remove name="ServiceModel" />  
</httpModules>
```  
  
## <a name="see-also"></a>另请参阅

- [如何：配置 WCF 服务以与 ASP.NET Web 服务客户端进行互操作](config-wcf-service-with-aspnet-web-service.md)
