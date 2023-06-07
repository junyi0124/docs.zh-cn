---
title: 具有 JSON 和 XML 的 AJAX 服务示例
ms.date: 03/30/2017
ms.assetid: 8ea5860d-0c42-4ae9-941a-e07efdd8e29c
ms.openlocfilehash: 8f70b6aa2e61d01a075a6edb3fe490ef593e73b0
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/09/2020
ms.locfileid: "84575948"
---
# <a name="ajax-service-with-json-and-xml-sample"></a>具有 JSON 和 XML 的 AJAX 服务示例

此示例演示如何使用 Windows Communication Foundation （WCF）创建返回 JavaScript 对象表示法（JSON）或 XML 数据的异步 JavaScript 和 XML （AJAX）服务。 可以从 Web 浏览器客户端使用 JavaScript 代码来访问 AJAX 服务。 此示例以[基本 AJAX 服务](basic-ajax-service.md)示例为基础。

与其他 AJAX 示例不同的是，此示例不使用 ASP.NET AJAX 和 <xref:System.Web.UI.ScriptManager> 控件。 利用一些附加配置，可以通过 JavaScript 从任何 HTML 页面访问 WCF AJAX 服务，此处显示了这种情况。 有关将 WCF 与 ASP.NET AJAX 一起使用的示例，请参阅[Ajax 示例](ajax.md)。

此示例演示如何在 JSON 和 XML 之间切换操作的响应类型。 无论服务是配置为由 ASP.NET AJAX 访问还是由 HTML/JavaScript 客户端页面访问，此功能均可用。

> [!NOTE]
> 本主题的最后介绍了此示例的设置过程和生成说明。

为了启用非 ASP.NET AJAX 客户端，请在 .svc 文件中使用 <xref:System.ServiceModel.Activation.WebServiceHostFactory>（而不是 <xref:System.ServiceModel.Activation.WebScriptServiceHostFactory>）。 <xref:System.ServiceModel.Activation.WebServiceHostFactory> 将 <xref:System.ServiceModel.Description.WebHttpEndpoint> 标准终结点添加到服务中。 在相对于 .svc 文件的空地址处配置终结点;这意味着服务的地址是 `http://localhost/ServiceModelSamples/service.svc` ，除操作名称外没有其他后缀。

`<%@ServiceHost language="c#" Debug="true" Service="Microsoft.Samples.XmlAjaxService.CalculatorService" Factory="System.ServiceModel.Activation.WebServiceHostFactory" %>`

可以使用 Web.config 中的以下节对终结点进行其他配置更改。 如果不需要额外更改，则可以移除该节。

```xml
<system.serviceModel>
  <standardEndpoints>
    <webHttpEndpoint>
      <!-- Use this element to configure the endpoint -->
      <standardEndpoint name="" />
    </webHttpEndpoint>
  </standardEndpoints>
</system.serviceModel>
```

的默认数据格式 <xref:System.ServiceModel.Description.WebHttpEndpoint> 为 XML，而的默认数据格式 <xref:System.ServiceModel.Description.WebScriptEndpoint> 为 JSON。 有关详细信息，请参阅在[不 ASP.NET 的情况下创建 WCF AJAX 服务](../feature-details/creating-wcf-ajax-services-without-aspnet.md)。

以下示例中的服务是一个包含两个操作的标准 WCF 服务。 这两个操作都需要针对 <xref:System.ServiceModel.Web.WebMessageBodyStyle.Wrapped> 或 <xref:System.ServiceModel.Web.WebGetAttribute> 属性使用 <xref:System.ServiceModel.Web.WebInvokeAttribute> 正文样式，该样式特定于 `webHttp` 行为，对于 JSON/XML 数据格式开关没有任何影响。

```csharp
[OperationContract]
[WebInvoke(ResponseFormat = WebMessageFormat.Xml, BodyStyle = WebMessageBodyStyle.Wrapped)]
MathResult DoMathXml(double n1, double n2);
```

操作的响应格式指定为 XML，这是行为的默认设置 [\<webHttp>](../../configure-apps/file-schema/wcf/webhttp.md) 。 但是，最好显式指定响应格式。

另一个操作使用 `WebInvokeAttribute` 属性并显式指定响应的 JSON（而不是 XML）。

```csharp
[OperationContract]
[WebInvoke(ResponseFormat = WebMessageFormat.Json, BodyStyle = WebMessageBodyStyle.Wrapped)]
MathResult DoMathJson(double n1, double n2);
```

请注意，在这两种情况下，操作都返回复杂类型，即 `MathResult` 标准 WCF 数据协定类型。

客户端网页 Xmlajaxclientpage.htm 包含 JavaScript 代码，该代码在用户单击页面上的 "**执行计算" （返回 JSON）** 或 "**执行计算（返回 XML）** " 按钮时调用上述两个操作之一。 用来调用服务的代码将构造 JSON 正文并使用 HTTP POST 发送它。 该请求是在 JavaScript 中手动创建的，不同于[基本 AJAX 服务](basic-ajax-service.md)示例和其他使用 ASP.NET ajax 的示例。

```csharp
// Create HTTP request
var xmlHttp;
// Request instantiation code omitted…
// Result handler code omitted…

// Build the operation URL
var url = "service.svc/ajaxEndpoint/";
url = url + operation;

// Build the body of the JSON message
var body = '{"n1":';
body = body + document.getElementById("num1").value + ',"n2":';
body = body + document.getElementById("num2").value + '}';

// Send the HTTP request
xmlHttp.open("POST", url, true);
xmlHttp.setRequestHeader("Content-type", "application/json");
xmlHttp.send(body);
```

当服务进行响应时，响应将显示出来，而不会在页面上的文本框中进一步执行任何处理。 这是为了进行演示而实现的，您可以直接遵循所使用的 XML 和 JSON 数据格式。

```javascript
// Create result handler
xmlHttp.onreadystatechange=function(){
     if(xmlHttp.readyState == 4){
          document.getElementById("result").value = xmlHttp.responseText;
     }
}
```

> [!IMPORTANT]
> 您的计算机上可能已安装这些示例。 在继续操作之前，请先检查以下（默认）目录：
>
> `<InstallDrive>:\WF_WCF_Samples`
>
> 如果此目录不存在，请参阅[.NET Framework 4 的 Windows Communication Foundation （wcf）和 Windows Workflow Foundation （WF）示例](https://www.microsoft.com/download/details.aspx?id=21459)以下载所有 WINDOWS COMMUNICATION FOUNDATION （wcf）和 [!INCLUDE[wf1](../../../../includes/wf1-md.md)] 示例。 此示例位于以下目录：
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\AJAX\XmlAjaxService`

#### <a name="to-set-up-build-and-run-the-sample"></a>设置、生成和运行示例

1. 确保已对[Windows Communication Foundation 示例执行了一次性安装过程](one-time-setup-procedure-for-the-wcf-samples.md)。

2. 按照[生成 Windows Communication Foundation 示例](building-the-samples.md)中所述生成解决方案 XmlAjaxService。

3. 导航到 `http://localhost/ServiceModelSamples/XmlAjaxClientPage.htm` （不要在浏览器中从项目目录打开 xmlajaxclientpage.htm）。

## <a name="see-also"></a>另请参阅

- [使用 HTTP POST 的 AJAX 服务](ajax-service-using-http-post.md)
