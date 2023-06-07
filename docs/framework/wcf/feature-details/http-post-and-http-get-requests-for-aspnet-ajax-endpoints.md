---
title: 如何：在 ASP.NET AJAX 终结点的 HTTP POST 和 HTTP GET 请求之间进行选择
ms.date: 03/30/2017
ms.assetid: b47de82a-4c92-4af6-bceb-a5cb8bb8ede9
ms.openlocfilehash: c74b1acdf3802ab680123cd9d676919fe47236e8
ms.sourcegitcommit: 0edbeb66d71b8df10fcb374cfca4d731b58ccdb2
ms.contentlocale: zh-CN
ms.lasthandoff: 07/07/2020
ms.locfileid: "86051579"
---
# <a name="how-to-choose-between-http-post-and-http-get-requests-for-aspnet-ajax-endpoints"></a>如何：在 ASP.NET AJAX 终结点的 HTTP POST 和 HTTP GET 请求之间进行选择

Windows Communication Foundation （WCF）允许您创建一个服务，用于公开可从客户端网站上的 JavaScript 中调用的 ASP.NET 支持 AJAX 的终结点。 有关生成此类服务的基本过程，[请参见如何：使用配置添加 ASP.NET Ajax 终结点](how-to-use-configuration-to-add-an-aspnet-ajax-endpoint.md)和[如何：在不使用配置的情况下添加 ASP.NET ajax 终结点](how-to-add-an-aspnet-ajax-endpoint-without-using-configuration.md)。  
  
 ASP.NET AJAX 支持使用 HTTP POST 和 HTTP GET 谓词（HTTP POST 为默认谓词）的操作。 创建没有任何副作用并返回很少或从不进行更改的数据的操作时，应改用 HTTP GET。 GET 操作的结果可以缓存，这意味着对相同操作的多次调用可能导致只请求一次服务。 缓存不是由 WCF 完成，但可以在任何级别（在用户的浏览器中、代理服务器上以及其他级别）进行。如果你想要提高服务性能，但如果经常更改数据或操作执行某些操作，则缓存将非常有利。  
  
 例如，如果设计服务来管理用户的音乐库，则基于唱片集的标题查找艺术家的操作可以从使用 GET 中获益，但是将唱片集添加到用户的个人收藏集的操作必须使用 POST。  
  
 若要控制缓存的生存期，请使用 <xref:System.ServiceModel.Web.OutgoingWebResponseContext> 类型。 例如，在设计返回每小时更新的天气预报时会使用 GET，但应将缓存持续时间限制在一小时以内，以防止服务的用户访问过时的数据。  
  
 在使用来自使用脚本管理器控件的 ASP.NET AJAX 页上的服务时，操作是使用 GET 还是 POST 并没有区别，脚本管理器机制会确保发出正确的请求类型。  
  
 HTTP GET 操作使用由 POST 操作支持的任何输入参数，其中包括复杂的数据协定类型。 但是，大多数情况下建议避免在 GET 操作中使用太多的参数或太复杂的参数，原因是这样会降低缓存的效率。  
  
 本主题演示了如何通过在服务协定中将 <xref:System.ServiceModel.Web.WebGetAttribute> 或 <xref:System.ServiceModel.Web.WebInvokeAttribute> 属性添加到相关的操作以在 GET 和 POST 之间选择。 运行服务所需的其他步骤（若要实现、配置和托管服务）与 WCF 中任何 ASP.NET AJAX 服务使用的步骤类似。  
  
 以 <xref:System.ServiceModel.Web.WebGetAttribute> 标记的操作始终使用 GET 请求。 以 <xref:System.ServiceModel.Web.WebInvokeAttribute> 标记或未以这两个属性中的任何一个进行标记的操作将使用 POST 请求。 通过 <xref:System.ServiceModel.Web.WebInvokeAttribute> 属性，<xref:System.ServiceModel.Web.WebInvokeAttribute.Method%2A> 允许使用 GET 和 POST 之外的其他 HTTP 谓词（如 PUT 和 DELETE 等）。 但是，ASP.NET AJAX 不支持这些谓词。 如果打算使用脚本管理器控件来使用来自 ASP.NET 页上的服务，请不要使用 <xref:System.ServiceModel.Web.WebInvokeAttribute.Method%2A> 属性。  
  
 有关切换到 GET 的工作示例，请参阅[基本 AJAX 服务](../samples/basic-ajax-service.md)示例。  
  
 有关使用 POST 的示例，请参阅[使用 HTTP post 的 AJAX 服务](../samples/ajax-service-using-http-post.md)示例。  
  
## <a name="to-create-a-wcf-service-that-responds-to-http-get-or-http-post-requests"></a>创建响应 HTTP GET 或 HTTP POST 请求的 WCF 服务
  
1. 使用以特性标记的接口定义基本 WCF 服务协定 <xref:System.ServiceModel.ServiceContractAttribute> 。 用 <xref:System.ServiceModel.OperationContractAttribute> 标记每个操作。 添加 <xref:System.ServiceModel.Web.WebGetAttribute> 属性以规定操作应响应 HTTP GET 请求。 也可以添加 <xref:System.ServiceModel.Web.WebInvokeAttribute> 属性来显式指定 HTTP POST，或不指定属性（默认设置为 HTTP POST）。
  
    ```csharp
    [ServiceContract]  
    public interface IMusicService  
    {  
        //This operation uses a GET method.  
        [OperationContract]  
        [WebGet]  
        string LookUpArtist(string album);  
  
        //This operation will use a POST method.  
        [OperationContract]  
        [WebInvoke]  
        void AddAlbum(string user, string album);  
  
        //This operation will use POST method by default  
        //since nothing else is explicitly specified.  
        [OperationContract]  
        string[] GetAlbums(string user);  
  
        //Other operations omitted…  
  
    }  
    ```  
  
2. 使用 `IMusicService` 实现 `MusicService` 服务协定。
  
    ```csharp
    public class MusicService : IMusicService  
    {  
        public void AddAlbum(string user, string album)  
        {  
            //Add implementation here.  
        }  
  
         //Other operations omitted…  
    }  
    ```  
  
3. 在应用程序中创建一个名为 service 的新文件（扩展名为 .svc）。 通过为服务添加适当的[ \@ ServiceHost](../../configure-apps/file-schema/wcf-directive/servicehost.md)指令信息来编辑此文件。 指定 <xref:System.ServiceModel.Activation.WebScriptServiceHostFactory> 要在[ \@ ServiceHost](../../configure-apps/file-schema/wcf-directive/servicehost.md)指令中使用以便自动配置 ASP.NET AJAX 终结点。  
  
    ```aspx-csharp
    <%@ServiceHost
        language=c#
        Debug="true"
        Service="Microsoft.Ajax.Samples.MusicService"  
        Factory=System.ServiceModel.Activation.WebScriptServiceHostFactory  
    %>  
    ```  
  
## <a name="to-call-the-service"></a>调用服务  
  
1. 可以通过使用浏览器来测试服务的 GET 操作，而无需使用任何客户端代码。 例如，如果你的服务是在地址配置的 `http://example.com/service.svc` ，则在 `http://example.com/service.svc/LookUpArtist?album=SomeAlbum` 浏览器地址栏中键入会调用服务，并使响应被下载或显示。
  
2. 可以像使用任何其他 ASP.NET AJAX 服务一样将服务与 GET 操作一起使用，即，在 ASP.NET AJAX 脚本管理器控件的“脚本”集合中输入相应的服务 URL。 有关示例，请参阅[基本 AJAX 服务](../samples/basic-ajax-service.md)。
  
## <a name="see-also"></a>请参阅

- [为 ASP.NET AJAX 创建 WCF 服务](creating-wcf-services-for-aspnet-ajax.md)
- [如何：将支持 AJAX 的 ASP.NET Web 服务迁移到 WCF](how-to-migrate-ajax-enabled-aspnet-web-services-to-wcf.md)
