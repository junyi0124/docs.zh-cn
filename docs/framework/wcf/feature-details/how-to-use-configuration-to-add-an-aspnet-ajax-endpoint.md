---
title: 如何：使用配置来添加 ASP.NET AJAX 终结点
ms.date: 03/30/2017
ms.assetid: 7cd0099e-dc3a-47e4-a38c-6e10f997f6ea
ms.openlocfilehash: b229173381eed3e821a9ad9e1a6639912521731c
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96268425"
---
# <a name="how-to-use-configuration-to-add-an-aspnet-ajax-endpoint"></a>如何：使用配置来添加 ASP.NET AJAX 终结点

Windows Communication Foundation (WCF) 允许您创建一个服务，该服务使 ASP.NET 的启用 AJAX 的终结点可以从客户端网站上的 JavaScript 中调用。 若要创建这样的终结点，可以使用配置文件，就像所有其他 Windows Communication Foundation (WCF) 终结点一样，或使用不需要任何配置元素的方法。 本主题演示配置方法。  
  
 允许服务终结点成为 ASP.NET 启用 AJAX 的过程部分包括将终结点配置为使用 <xref:System.ServiceModel.WebHttpBinding> 和来添加 [\<enableWebScript>](../../configure-apps/file-schema/wcf/enablewebscript.md) 终结点行为。 配置终结点后，实现和托管服务的步骤与任何 WCF 服务使用的步骤类似。 有关工作示例，请参阅 [使用 HTTP POST 的 AJAX 服务](../samples/ajax-service-using-http-post.md)。  
  
 有关如何在不使用配置的情况下配置 ASP.NET AJAX 终结点的详细信息，请参阅 [如何：在不使用配置的情况下添加 ASP.NET AJAX 终结点](how-to-add-an-aspnet-ajax-endpoint-without-using-configuration.md)。  
  
## <a name="to-create-a-basic-wcf-service"></a>创建基本 WCF 服务  
  
1. 使用以特性标记的接口定义基本 WCF 服务协定 <xref:System.ServiceModel.ServiceContractAttribute> 。 用 <xref:System.ServiceModel.OperationContractAttribute> 标记每个操作。 确保设置 <xref:System.ServiceModel.ServiceContractAttribute.Namespace%2A> 属性。  
  
    ```csharp
    [ServiceContract(Namespace = "MyService")]  
    public interface ICalculator  
    {  
        [OperationContract]  
         // This operation returns the sum of d1 and d2.  
        double Add(double n1, double n2);  
  
        //Other operations omitted…  
  
    }  
    ```  
  
2. 使用 `ICalculator` 实现 `CalculatorService` 服务协定。  
  
    ```csharp
    public class CalculatorService : ICalculator  
    {  
        public double Add(double n1, double n2)  
        {  
            return n1 + n2;  
        }
        // Other operations omitted…
    }
    ```  
  
3. 定义 `ICalculator` 和 `CalculatorService` 实现的命名空间，方法是将它们放置在一个命名空间块中。  
  
    ```csharp
    namespace Microsoft.Ajax.Samples
    {  
        //Include the code for ICalculator and Caculator here.  
    }  
    ```  
  
## <a name="to-create-an-aspnet-ajax-endpoint-for-the-service"></a>创建服务的 ASP.NET AJAX 终结点  
  
1. 创建一个行为配置，并 [\<enableWebScript>](../../configure-apps/file-schema/wcf/enablewebscript.md) 为该服务的 ASP.NET 启用 AJAX 的终结点指定行为。  
  
    ```xml  
    <system.serviceModel>  
        <behaviors>  
            <endpointBehaviors>  
                <behavior name="AspNetAjaxBehavior">  
                    <enableWebScript />  
                </behavior>  
            </endpointBehaviors>  
        </behaviors>  
    </system.serviceModel>  
    ```  
  
2. 为使用 <xref:System.ServiceModel.WebHttpBinding> 和前面的步骤中定义的 ASP.NET AJAX 行为的服务创建终结点。  
  
    ```xml  
    <system.serviceModel>  
        <services>  
            <service name="Microsoft.Ajax.Samples.CalculatorService">  
                <endpoint address=""  
                    behaviorConfiguration="AspNetAjaxBehavior"
                    binding="webHttpBinding"  
                    contract="Microsoft.Ajax.Samples.ICalculator" />  
            </service>  
        </services>  
    </system.serviceModel>
    ```  
  
## <a name="to-host-the-service-in-iis"></a>在 IIS 中承载服务  
  
1. 若要在 IIS 中承载服务，请使用应用程序中的 .svc 扩展创建名为 service 的新文件。 通过为服务添加适当的[ \@ ServiceHost](../../configure-apps/file-schema/wcf-directive/servicehost.md)指令信息来编辑此文件。 例如，`CalculatorService` 示例的服务文件中的内容包含以下信息。  
  
    ```aspx-csharp
    <%@ServiceHost
    language=c#
    Debug="true"
    Service="Microsoft.Ajax.Samples.CalculatorService"  
    %>  
    ```  
  
2. 有关在 IIS 中承载的详细信息，请参阅 [如何：在 iis 中承载 WCF 服务](how-to-host-a-wcf-service-in-iis.md)。  
  
## <a name="to-call-the-service"></a>调用服务  
  
1. 终结点是在相对于 .svc 文件的空地址处配置的，因此该服务现在可用，并可通过将请求发送到服务 .svc/ \<operation> -例如，为该操作发送服务。 `Add` 可以通过在 ASP.NET AJAX 脚本管理器控件的脚本集合中输入终结点 URL 来使用它。 有关示例，请参阅 [使用 HTTP POST 的 AJAX 服务](../samples/ajax-service-using-http-post.md)。  
  
## <a name="see-also"></a>另请参阅

- [为 ASP.NET AJAX 创建 WCF 服务](creating-wcf-services-for-aspnet-ajax.md)
- [如何：将支持 AJAX 的 ASP.NET Web 服务迁移到 WCF](how-to-migrate-ajax-enabled-aspnet-web-services-to-wcf.md)
