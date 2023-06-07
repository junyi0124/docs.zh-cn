---
title: 独立诊断源示例
ms.date: 03/30/2017
ms.assetid: d31c6c1f-292c-4d95-8e23-ed8565970ea5
ms.openlocfilehash: b4c3613656e0aec42c0d3f5cd7cde0af6540a69a
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96268243"
---
# <a name="stand-alone-diagnostics-feed-sample"></a>独立诊断源示例

此示例演示如何使用 Windows Communication Foundation (WCF) 为联合创建 RSS/Atom 源。 它是一个基本的 "Hello World" 程序，它显示对象模型的基础知识，以及如何在 Windows Communication Foundation (WCF) 服务上对其进行设置。  
  
 WCF 将联合源建模为返回特殊数据类型的服务操作 <xref:System.ServiceModel.Syndication.SyndicationFeedFormatter> 。 <xref:System.ServiceModel.Syndication.SyndicationFeedFormatter> 的实例可以将源序列化为 RSS 2.0 和 Atom 1.0 格式。 下面的示例代码演示所使用的协定。  
  
```csharp  
[ServiceContract(Namespace = "")]  
    interface IDiagnosticsService  
    {  
        [OperationContract]  
        //The [WebGet] attribute controls how WCF dispatches  
        //HTTP requests to service operations based on a URI suffix  
        //(the part of the request URI after the endpoint address)  
        //using the HTTP GET method. The UriTemplate specifies a relative  
        //path of 'feed', and specifies that the format is  
        //supplied using a query string.
        [WebGet(UriTemplate="feed?format={format}")]  
        [ServiceKnownType(typeof(Atom10FeedFormatter))]  
        [ServiceKnownType(typeof(Rss20FeedFormatter))]  
        SyndicationFeedFormatter GetProcesses(string format);  
    }  
```  
  
 此 `GetProcesses` 操作用属性进行批注 <xref:System.ServiceModel.Web.WebGetAttribute> ，该属性使您能够控制 WCF 如何向服务操作调度 HTTP GET 请求并指定所发送消息的格式。  
  
 与任何 WCF 服务一样，联合源可在任何托管的应用程序中自承载。 联合服务需要使用特定绑定 (<xref:System.ServiceModel.WebHttpBinding>) 和特定终结点行为 (<xref:System.ServiceModel.Description.WebHttpBehavior>) 才能正常运行。 新的 <xref:System.ServiceModel.Web.WebServiceHost> 类提供了一个方便的 API，用于在不使用特定配置的情况下创建此类终结点。  
  
```csharp  
WebServiceHost host = new WebServiceHost(typeof(ProcessService), new Uri("http://localhost:8000/diagnostics"));  
  
            //The WebServiceHost will automatically provide a default endpoint at the base address  
            //using the proper binding (the WebHttpBinding) and endpoint behavior (the WebHttpBehavior)  
```  
  
 或者，可在 IIS 承载的 .svc 文件中使用 <xref:System.ServiceModel.Activation.WebServiceHostFactory> 来提供等效的功能（此技术未在此示例代码中演示）。  
  
```xml
<% @ServiceHost Language="C#|VB" Debug="true" Service="ProcessService" %>
```
  
 由于此服务使用标准 HTTP GET 接收请求，因此可以使用任何识别 RSS 或 ATOM 的客户端来访问此服务。 例如，可以通过在 `http://localhost:8000/diagnostics/feed/?format=atom` `http://localhost:8000/diagnostics/feed/?format=rss` 识别 RSS 的浏览器中导航到或来查看此服务的输出。
  
 你还可以使用 [WCF 联合对象模型如何映射到 Atom 和 RSS](../feature-details/how-the-wcf-syndication-object-model-maps-to-atom-and-rss.md) 来读取联合数据并使用命令性代码对其进行处理。  
  
```csharp
XmlReader reader = XmlReader.Create( "http://localhost:8000/diagnostics/feed/?format=rss",
    new XmlReaderSettings()
    {
        //MaxCharactersInDocument can be used to control the maximum amount of data
        //read from the reader and helps prevent OutOfMemoryException
        MaxCharactersInDocument = 1024 * 64
    } );

SyndicationFeed feed = SyndicationFeed.Load(reader);

foreach (SyndicationItem i in feed.Items)
{
    XmlSyndicationContent content = i.Content as XmlSyndicationContent;
    ProcessData pd = content.ReadContent<ProcessData>();

    Console.WriteLine(i.Title.Text);
    Console.WriteLine(pd.ToString());
}
```
  
## <a name="set-up-build-and-run-the-sample"></a>设置、生成和运行示例
  
1. 请确保对计算机上的 HTTP 和 HTTPS 具有正确的地址注册权限，如在 [Windows Communication Foundation 示例的一次性安装过程](one-time-setup-procedure-for-the-wcf-samples.md)中所述。

2. 生成解决方案。

3. 运行控制台应用程序。

4. 当控制台应用程序正在运行时，导航到 `http://localhost:8000/diagnostics/feed/?format=atom` 或 `http://localhost:8000/diagnostics/feed/?format=rss` 使用识别 RSS 的浏览器。

> [!IMPORTANT]
> 您的计算机上可能已安装这些示例。 在继续操作之前，请先检查以下（默认）目录：
>
> `<InstallDrive>:\WF_WCF_Samples`
>
> 如果此目录不存在，请参阅[Windows Communication Foundation (wcf) ，并 Windows Workflow Foundation (的 WF](https://www.microsoft.com/download/details.aspx?id=21459)) .NET Framework Windows Communication Foundation ([!INCLUDE[wf1](../../../../includes/wf1-md.md)] 此示例位于以下目录：
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Syndication\DiagnosticsFeed`

## <a name="see-also"></a>另请参阅

- [WCF Web HTTP 编程模型](../feature-details/wcf-web-http-programming-model.md)
- [WCF 联合](../feature-details/wcf-syndication.md)
