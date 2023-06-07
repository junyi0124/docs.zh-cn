---
title: 基本 AJAX 服务
ms.date: 03/30/2017
ms.assetid: d66d0c91-0109-45a0-a901-f3e4667c2465
ms.openlocfilehash: 334cc9e53d7d9746c204abe37e7c30d00baa824b
ms.sourcegitcommit: 5fb5b6520b06d7f5e6131ec2ad854da302a28f2e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/03/2019
ms.locfileid: "74716127"
---
# <a name="basic-ajax-service"></a>基本 AJAX 服务

此示例演示如何使用 Windows Communication Foundation （WCF）来创建基本 ASP.NET 异步 JavaScript 和 XML （AJAX）服务（可使用 Web 浏览器客户端中的 JavaScript 代码访问的服务）。 该服务使用 <xref:System.ServiceModel.Web.WebGetAttribute> 属性以确保服务响应 HTTP GET 请求并被配置为对响应使用 JavaScript 对象表示法 (JSON) 数据格式。

WCF 中的 AJAX 支持经过优化，可通过 `ScriptManager` 控件与 ASP.NET AJAX 一起使用。 有关将 WCF 与 ASP.NET AJAX 一起使用的示例，请参阅[Ajax 示例](ajax.md)。

> [!NOTE]
> 本主题的最后介绍了此示例的设置过程和生成说明。

在下面的代码中，将 <xref:System.ServiceModel.Web.WebGetAttribute> 属性应用于 `Add` 操作以确保服务响应 HTTP GET 请求。 为了简单起见，该代码使用 GET（你可以从任何 Web 浏览器构造 HTTP GET 请求）。 也可以使用 GET 来启用缓存。 在缺少 `WebGetAttribute` 属性时，HTTP POST 是默认属性。

```csharp
[ServiceContract(Namespace = "SimpleAjaxService")]
public interface ICalculator
{
    [WebGet]
    double Add(double n1, double n2);
    //Other operations omitted…
}
```

示例 .svc 文件使用的是 <xref:System.ServiceModel.Activation.WebScriptServiceHostFactory>，后者会将 <xref:System.ServiceModel.Description.WebScriptEndpoint> 标准终结点添加到服务。 可在相对于 .svc 文件的空地址处配置该终结点。 这意味着服务的地址是 `http://localhost/ServiceModelSamples/service.svc`的，除了操作名称外没有其他后缀。

`<%@ServiceHost language="C#" Debug="true" Service="Microsoft.Samples.SimpleAjaxService.CalculatorService" Factory="System.ServiceModel.Activation.WebScriptServiceHostFactory" %>`

将对 <xref:System.ServiceModel.Description.WebScriptEndpoint> 进行预配置，以便能从 ASP.NET AJAX 客户端页访问服务。 可以使用 Web.config 中的以下节对终结点进行其他配置更改。 如果不需要额外更改，则可以移除该节。

```xml
<system.serviceModel>
  <standardEndpoints>
    <webScriptEndpoint>
      <!-- Use this element to configure the endpoint -->
      <standardEndpoint name=""  />
    </webScriptEndpoint>
  </standardEndpoints>
</system.serviceModel>
```

<xref:System.ServiceModel.Description.WebScriptEndpoint> 将服务的默认数据格式设置为 JSON 而不是 XML。 若要调用服务，请在完成本主题后面的设置和生成步骤后导航到 `http://localhost/ServiceModelSamples/service.svc/Add?n1=100&n2=200`。 这个测试功能是通过使用 HTTP GET 请求实现的。

客户端 Web 页 SimpleAjaxClientPage.aspx 包含 ASP.NET 代码，无论何时用户单击该页面上的操作按钮之一，就能调用服务。 `ScriptManager` 控件用于使服务的代理可以通过 JavaScript 访问。

```aspx-csharp
<asp:ScriptManager ID="ScriptManager" runat="server">
    <Services>
        <asp:ServiceReference Path="service.svc" />
    </Services>
</asp:ScriptManager>
```

使用以下 JavaScript 代码实例化本地代理和调用操作。

```javascript
// Code for extracting arguments n1 and n2 omitted…
// Instantiate a service proxy
var proxy = new SimpleAjaxService.ICalculator();
// Code for selecting operation omitted…
proxy.Add(parseFloat(n1), parseFloat(n2), onSuccess, onFail, null);
```

如果服务调用成功，则代码调用 `onSuccess` 处理程序并在文本框中显示操作结果。

```javascript
function onSuccess(mathResult){
     document.getElementById("result").value = mathResult;
}
```

> [!IMPORTANT]
> 您的计算机上可能已安装这些示例。 在继续操作之前，请先检查以下（默认）目录：
>
> `<InstallDrive>:\WF_WCF_Samples`
>
> 如果此目录不存在，请参阅[.NET Framework 4 的 Windows Communication Foundation （wcf）和 Windows Workflow Foundation （WF）示例](https://www.microsoft.com/download/details.aspx?id=21459)以下载所有 WINDOWS COMMUNICATION FOUNDATION （wcf）和 [!INCLUDE[wf1](../../../../includes/wf1-md.md)] 示例。 此示例位于以下目录：
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Ajax\SimpleAjaxService`
