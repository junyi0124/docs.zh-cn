---
title: 使用复杂类型的 AJAX 服务示例
ms.date: 03/30/2017
ms.assetid: 88242b99-4811-4cbe-8201-52ddf48fb174
ms.openlocfilehash: 4227446e8844accd06490d8e7cf933da43d875a6
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/09/2020
ms.locfileid: "84575909"
---
# <a name="ajax-service-using-complex-types-sample"></a>使用复杂类型的 AJAX 服务示例

此示例演示如何使用 Windows Communication Foundation （WCF）创建 ASP.NET 的异步 JavaScript 和 XML （AJAX）服务，该服务创建复杂类型的实例，并在服务和客户端之间以 JavaScript 对象表示法（JSON）的形式发送它们。 可以从 Web 浏览器客户端使用 JavaScript 代码来访问 AJAX 服务。 此示例以[基本 AJAX 服务](basic-ajax-service.md)示例为基础。

WCF 中的 AJAX 支持经过优化，可在控件中与 ASP.NET AJAX 一起使用 <xref:System.Web.UI.ScriptManager> 。 有关将 WCF 与 ASP.NET AJAX 一起使用的示例，请参阅[Ajax 示例](ajax.md)。

> [!NOTE]
> 本主题的最后介绍了此示例的设置过程和生成说明。

以下示例中的服务是不包含 AJAX 特定代码的 WCF 服务。 由于未应用 <xref:System.ServiceModel.Web.WebGetAttribute> 属性，因此将使用默认的 HTTP 谓词（“POST”）。 该服务有一个操作 `DoMath`，该操作返回一个名为 `MathResult` 的复杂类型。 该复杂类型是标准的数据协定类型，也不包含特定于 AJAX 的代码。

```csharp
[DataContract]
public class MathResult
{
    [DataMember]
    public double sum;
    [DataMember]
    public double difference;
    [DataMember]
    public double product;
    [DataMember]
    public double quotient;
}
```

通过使用 <xref:System.ServiceModel.Activation.WebScriptServiceHostFactory> 在服务上创建 AJAX 终结点，就像在基本 AJAX 服务示例中一样。

当用户单击页面上的 "**执行计算**" 按钮时，客户端网页 complextypeclientpage.aspx 包含用于调用服务的 ASP.NET 和 JavaScript 代码。 用于调用服务的代码构造 JSON 正文并使用 HTTP POST 发送它，类似于[使用 HTTP post 的 AJAX 服务](ajax-service-using-http-post.md)示例。

在服务调用成功之后，您可以在所得到的 JavaScript 对象上访问各个数据成员（`sum`、`difference`、`product` 和 `quotient`）。

```javascript
function onSuccess(mathResult){
     document.getElementById("sum").value = mathResult.sum;
     document.getElementById("difference").value = mathResult.difference;
     document.getElementById("product").value = mathResult.product;
     document.getElementById("quotient").value = mathResult.quotient;
}
```

### <a name="to-set-up-build-and-run-the-sample"></a>设置、生成和运行示例

1. 确保已对[Windows Communication Foundation 示例执行了一次性安装过程](one-time-setup-procedure-for-the-wcf-samples.md)。

2. 按照[生成 Windows Communication Foundation 示例](building-the-samples.md)中所述生成解决方案 ComplexTypeAjaxService。

3. 导航到 `http://localhost/ServiceModelSamples/ComplexTypeClientPage.aspx` （不要在浏览器中从项目目录打开 complextypeclientpage.aspx）。

> [!IMPORTANT]
> 您的计算机上可能已安装这些示例。 在继续操作之前，请先检查以下（默认）目录：
>
> `<InstallDrive>:\WF_WCF_Samples`
>
> 如果此目录不存在，请参阅[.NET Framework 4 的 Windows Communication Foundation （wcf）和 Windows Workflow Foundation （WF）示例](https://www.microsoft.com/download/details.aspx?id=21459)以下载所有 WINDOWS COMMUNICATION FOUNDATION （wcf）和 [!INCLUDE[wf1](../../../../includes/wf1-md.md)] 示例。 此示例位于以下目录：
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Ajax\ComplexTypeAjaxService`

## <a name="see-also"></a>另请参阅

- [基本 AJAX 服务](basic-ajax-service.md)
