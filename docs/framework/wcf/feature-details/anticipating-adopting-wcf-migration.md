---
title: Windows Communication Foundation 使用展望：轻松实现未来的迁移
ms.date: 03/30/2017
ms.assetid: f49664d9-e9e0-425c-a259-93f0a569d01b
ms.openlocfilehash: bf3155f19e42787746d59ce7b593273522e2840a
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96245050"
---
# <a name="anticipating-adopting-the-windows-communication-foundation-easing-future-migration"></a>Windows Communication Foundation 使用展望：轻松实现未来的迁移

若要确保更方便地将新的 ASP.NET 应用程序迁移到 WCF，请遵循上述建议以及下面的建议。  
  
## <a name="protocols"></a>协议  

 禁用 ASP.NET 2.0 对 SOAP 1.2 的支持：  
  
```xml  
<configuration>  
     <system.web>  
      <webServices >  
          <protocols>  
           <remove name="HttpSoap12"/>  
          </protocols>
      </webServices>  
     </system.web>
</configuration>  
```  
  
 这样做是可行的，因为 WCF 需要与不同的协议（如 SOAP 1.1 和 SOAP 1.2）一致的消息才能使用不同的终结点。 如果将 ASP.NET 2.0 Web 服务配置为支持 SOAP 1.1 和 SOAP 1.2 （这是默认配置），则不能将它转发到原始地址处的单个 WCF 终结点，该终结点肯定与所有 ASP.NET Web 服务的现有客户端兼容。 另外，选择 SOAP 1.2 而非 SOAP 1.1 将更加严格地限制服务的客户。  
  
## <a name="service-development"></a>服务开发  

 WCF 允许您通过将应用 <xref:System.ServiceModel.ServiceContractAttribute> 到接口或类来定义服务协定。 建议将此属性应用于接口而不是类，因为这样可以为可由任意数量的类以不同方式实现的协定创建一个定义。 ASP.NET 2.0 支持将 <xref:System.Web.Services.WebService> 属性应用到接口和类的选项。 但是，如前所述，ASP.NET 2.0 中存在缺陷，如果将 <xref:System.Web.Services.WebService> 属性应用到接口而不是类，则此属性的 Namespace 参数将不起任何作用。 由于通常建议使用特性的 Namespace 参数从默认值修改服务的命名空间，因此，可以 `http://tempuri.org` <xref:System.Web.Services.WebService> 通过 <xref:System.ServiceModel.ServiceContractAttribute> 将特性应用到接口或类来继续定义 ASP.NET Web 服务。  
  
- 在定义那些接口的方法中尽量少使用代码。 允许它们将其工作委托给其他类。 新的 WCF 服务类型还可将其实体工作委托给这些类。  
  
- 使用 `MessageName` 的 <xref:System.Web.Services.WebMethodAttribute> 参数为服务的操作提供显式名称。  
  
    ```csharp  
    [WebMethod(MessageName="ExplicitName")]  
    string Echo(string input);  
    ```  
  
     这样做很重要，因为 ASP.NET 中的操作的默认名称不同于 WCF 提供的默认名称。 通过提供显式名称，可以避免依赖默认名称。  
  
- 不要通过多态方法实现 ASP.NET Web 服务操作，因为 WCF 不支持通过多态方法实现操作。  
  
- 使用 <xref:System.Web.Services.Protocols.SoapDocumentMethodAttribute> 为 SOAPAction HTTP 标头提供显式值，HTTP 请求将通过此标头路由到方法。  
  
    ```csharp  
    [WebMethod]  
    [SoapDocumentMethod(RequestElementName="ExplicitAction")]  
    string Echo(string input);  
    ```  
  
     采用此方法将绕过 ASP.NET 和 WCF 使用的默认 SOAPAction 值。  
  
- 避免使用 SOAP 扩展。 如果 SOAP 扩展是必需的，则确定是否要将其视为已由 WCF 提供的功能。 如果确实如此，请重新考虑选择不立即采用 WCF。  
  
## <a name="state-management"></a>状态管理  

 避免必须在服务中保持状态。 维护状态不仅会损害应用程序的可伸缩性，而且 ASP.NET 和 WCF 的状态管理机制也有很大不同，尽管 WCF 在 ASP.NET 兼容模式下支持 ASP.NET 机制。  
  
## <a name="exception-handling"></a>异常处理  

 在设计服务发送和接收的数据类型的结构时，请同时设计表示异常的不同类型的结构，这些异常可能发生在人们希望传送到客户端的服务中。  
  
```csharp  
[Serializable]  
[XmlRoot(Namespace="ExplicitNamespace", IsNullable=true)]  
public partial class AnticipatedException
{
    private string anticipatedExceptionInformationField;  

    public string AnticipatedExceptionInformation
    {  
        get {
            return this.anticipatedExceptionInformationField;  
        }  
        set {  
            this.anticipatedExceptionInformationField = value;  
        }  
    }  
}  
```  
  
 使这些类能够将其自身序列化为 XML：  
  
```csharp  
public XmlNode ToXML()  
{  
     XmlSerializer serializer = new XmlSerializer(  
      typeof(AnticipatedException));  
     MemoryStream memoryStream = new MemoryStream();  
     XmlTextWriter writer = new XmlTextWriter(  
     memoryStream, UnicodeEncoding.UTF8);  
     serializer.Serialize(writer, this);  
     XmlDocument document = new XmlDocument();  
     document.LoadXml(new string(  
     UnicodeEncoding.UTF8.GetChars(  
     memoryStream.GetBuffer())).Trim());  
    return document.DocumentElement;  
}  
```  
  
 然后，可使用这些类为显式引发的 <xref:System.Web.Services.Protocols.SoapException> 实例提供详细信息：  
  
```csharp  
AnticipatedException exception = new AnticipatedException();  
exception.AnticipatedExceptionInformation = "…";  
throw new SoapException(  
     "Fault occurred",  
     SoapException.ClientFaultCode,  
     Context.Request.Url.AbsoluteUri,  
     exception.ToXML());  
```  
  
 这些异常类将随时与 WCF 类结合使用 <xref:System.ServiceModel.FaultException%601> ，以引发新的 `FaultException<AnticipatedException>(anticipatedException);`  
  
## <a name="security"></a>安全性  

 下面是一些安全建议。  
  
- 避免使用 ASP.NET 2.0 配置文件，因为如果将服务迁移到 WCF，则使用这些配置文件会限制 ASP.NET 集成模式的使用。  
  
- 避免使用 Acl 来控制对服务的访问，因为 ASP.NET Web 服务使用 Internet Information Services (IIS) 支持 Acl，因此 WCF 不支持，因为 ASP.NET Web 服务依赖于 IIS 进行承载，而 WCF 不一定必须承载在 IIS 中。  
  
- 请务必考虑使用 ASP.NET 2.0 角色提供程序来授权对服务资源的访问。  
  
## <a name="see-also"></a>另请参阅

- [Windows Communication Foundation 使用展望：轻松实现未来的集成](anticipating-adopting-the-wcf-easing-future-integration.md)
