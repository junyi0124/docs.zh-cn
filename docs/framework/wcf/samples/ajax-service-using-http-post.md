---
title: 使用 HTTP POST 的 AJAX 服务
ms.date: 03/30/2017
ms.assetid: 1ac80f20-ac1c-4ed1-9850-7e49569ff44e
ms.openlocfilehash: 143585b40a493983b7265971a17224165de6f36d
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/09/2020
ms.locfileid: "84575883"
---
# <a name="ajax-service-using-http-post"></a>使用 HTTP POST 的 AJAX 服务

此示例演示如何使用 Windows Communication Foundation （WCF）来创建使用 HTTP POST 的 ASP.NET 异步 JavaScript 和 XML （AJAX）服务。 AJAX 服务是指可以从 Web 浏览器客户端使用基本 JavaScript 代码访问的服务。 此示例以[基本 AJAX 服务](basic-ajax-service.md)示例为基础。这两个示例的唯一区别在于使用 HTTP POST，而不是 HTTP GET。

Windows Communication Foundation （WCF）中的 AJAX 支持经过优化，可通过控件与 ASP.NET AJAX 一起使用 `ScriptManager` 。 有关将 WCF 与 ASP.NET AJAX 一起使用的示例，请参阅[Ajax 示例](ajax-service-using-http-post.md)。

> [!NOTE]
> 本主题的最后介绍了此示例的设置过程和生成说明。

以下示例中的服务是不包含 AJAX 特定代码的 WCF 服务。

如果对 <xref:System.ServiceModel.Web.WebInvokeAttribute> 操作应用了属性，或 <xref:System.ServiceModel.Web.WebGetAttribute> 未应用该属性，则使用默认的 HTTP 谓词（"POST"）。 POST 请求虽然比 GET 请求更难构造，但它们不会被缓存；当不适宜缓存时，应对所有操作使用 POST 请求。

```csharp
[ServiceContract(Namespace = "PostAjaxService")]
public interface ICalculator
{
    [WebInvoke]
    double Add(double n1, double n2);
    //Other operations omitted…
}
```

通过使用 <xref:System.ServiceModel.Activation.WebScriptServiceHostFactory> 在服务上创建 AJAX 终结点，就像在基本 AJAX 服务示例中一样。

与 GET 请求不同的是，不能从浏览器中调用 POST 服务。 例如，导航到会 `http://localhost/ServiceModelSamples/service.svc/Add?n1=100&n2=200` 导致错误，因为 POST 服务要求在 `n1` `n2` 消息正文中以 JSON 格式发送和参数，而不是在 URL 中发送和参数。

客户端网页 PostAjaxClientPage.aspx 包含 ASP.NET 代码以便在用户单击页面上的操作按钮之一时调用服务。 服务的响应方式与在[基本 AJAX 服务](basic-ajax-service.md)示例中一样，通过 GET 请求来实现。

> [!IMPORTANT]
> 您的计算机上可能已安装这些示例。 在继续操作之前，请先检查以下（默认）目录：
>
> `<InstallDrive>:\WF_WCF_Samples`
>
> 如果此目录不存在，请参阅[.NET Framework 4 的 Windows Communication Foundation （wcf）和 Windows Workflow Foundation （WF）示例](https://www.microsoft.com/download/details.aspx?id=21459)以下载所有 WINDOWS COMMUNICATION FOUNDATION （wcf）和 [!INCLUDE[wf1](../../../../includes/wf1-md.md)] 示例。 此示例位于以下目录：
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Ajax\PostAjaxService`

## <a name="to-set-up-build-and-run-the-sample"></a>设置、生成和运行示例

1. 确保为 Windows Communication Foundation 示例执行设置说明[一次性安装过程](one-time-setup-procedure-for-the-wcf-samples.md)。

2. 按照[生成 Windows Communication Foundation 示例](building-the-samples.md)中所述生成解决方案 postajaxservice.sln。

3. 导航到 `http://localhost/ServiceModelSamples/PostAjaxClientPage.aspx` （不要在浏览器中从项目目录打开 postajaxclientpage.aspx）。
