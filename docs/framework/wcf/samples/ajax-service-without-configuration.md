---
title: 无配置的 AJAX 服务
ms.date: 03/30/2017
ms.assetid: e6db7acd-5679-45d4-b98a-8449c6873838
ms.openlocfilehash: ab3731ab6aeb80e0e46228b8bf702b0fe5c6e6e9
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/09/2020
ms.locfileid: "84575896"
---
# <a name="ajax-service-without-configuration"></a>无配置的 AJAX 服务

此示例演示如何使用 Windows Communication Foundation （WCF）来创建基本 ASP.NET 异步 JavaScript 和 XML （AJAX）服务（可通过从 Web 浏览器客户端使用 JavaScript 代码访问的服务）而无需使用任何配置设置。 该服务在 .svc 文件中使用特殊语法来自动启用 AJAX 终结点。

WCF 中的 AJAX 支持经过优化，可在控件中与 ASP.NET AJAX 一起使用 `ScriptManager` 。 有关将 WCF 与 ASP.NET AJAX 一起使用的示例，请参阅[Ajax 示例](ajax.md)。

> [!NOTE]
> 本主题的最后介绍了此示例的设置过程和生成说明。

 此示例是基于使用 HTTP POST 的 AJAX 服务生成的。 如[基本 AJAX 服务](basic-ajax-service.md)示例中所述， <xref:System.ServiceModel.Activation.WebScriptServiceHostFactory> 用于宿主服务。

```text
<%ServiceHost
    language=c#
    Debug="true"
    Service="Microsoft.Ajax.Samples.CalculatorService
    Factory="System.ServiceModel.Activation.WebScriptServiceHostFactory"
%>
```

<xref:System.ServiceModel.Activation.WebScriptServiceHostFactory> 自动将 <xref:System.ServiceModel.Description.WebScriptEndpoint> 添加到服务。 如果不需要对终结点进行任何配置更改，则可从服务的 Web.config 文件中完全移除 `<system.ServiceModel>` 部分。 Web.config 文件包含一些由 ConfigFreeClientPage.aspx 使用的 ASP.NET 设置。 如果不是这样，则可以移除整个 Web.config 文件。

> [!IMPORTANT]
> 您的计算机上可能已安装这些示例。 在继续操作之前，请先检查以下（默认）目录：
>
> `<InstallDrive>:\WF_WCF_Samples`
>
> 如果此目录不存在，请参阅[.NET Framework 4 的 Windows Communication Foundation （wcf）和 Windows Workflow Foundation （WF）示例](https://www.microsoft.com/download/details.aspx?id=21459)以下载所有 WINDOWS COMMUNICATION FOUNDATION （wcf）和 [!INCLUDE[wf1](../../../../includes/wf1-md.md)] 示例。 此示例位于以下目录：
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Ajax\ConfigFreeAjaxService`

#### <a name="to-set-up-build-and-run-the-sample"></a>设置、生成和运行示例

1. 确保在[Windows Communication Foundation 示例的一次性安装过程](one-time-setup-procedure-for-the-wcf-samples.md)中执行设置说明。

2. 按照[生成 Windows Communication Foundation 示例](building-the-samples.md)中所述生成解决方案 ConfigFreeAjaxService。

3. 导航到 `http://localhost/ServiceModelSamples/ConfigFreeClientPage.aspx` （不要在浏览器中从项目目录中打开 configfreeclientpage.aspx）。

> [!NOTE]
> 运行此示例时，请确保不要对 IIS 中的 ServiceModelSamples 文件夹同时启用匿名身份验证和 Windows 身份验证。 如果同时启用了这两种身份验证，请禁用 Windows 身份验证。 运行了该示例后，请启用 Windows 身份验证并运行“iisreset”。

## <a name="see-also"></a>另请参阅

- [基本 AJAX 服务](basic-ajax-service.md)
