---
title: ASP.NET 缓存集成
ms.date: 03/30/2017
ms.assetid: f581923a-8a72-42fc-bd6a-46de2aaeecc1
ms.openlocfilehash: c541f3caad8a500b9fdb33d00b58706bac876e37
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/09/2020
ms.locfileid: "84594746"
---
# <a name="aspnet-caching-integration"></a>ASP.NET 缓存集成

此示例演示如何通过 WCF WEB HTTP 编程模型使用 ASP.NET 输出缓存。 本主题重点介绍 ASP.NET 输出缓存集成功能。

## <a name="demonstrates"></a>演示

与 ASP.NET 输出缓存的集成

> [!IMPORTANT]
> 您的计算机上可能已安装这些示例。 在继续操作之前，请先检查以下（默认）目录：
>
> `<InstallDrive>:\WF_WCF_Samples`
>
> 如果此目录不存在，请参阅[.NET Framework 4 的 Windows Communication Foundation （wcf）和 Windows Workflow Foundation （WF）示例](https://www.microsoft.com/download/details.aspx?id=21459)以下载所有 WINDOWS COMMUNICATION FOUNDATION （wcf）和 [!INCLUDE[wf1](../../../../includes/wf1-md.md)] 示例。 此示例位于以下目录：
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Web\AspNetCachingIntegration`

## <a name="discussion"></a>讨论区

该示例使用将 <xref:System.ServiceModel.Web.AspNetCacheProfileAttribute> ASP.NET output 缓存与 Windows Communication Foundation （WCF）服务结合使用。 <xref:System.ServiceModel.Web.AspNetCacheProfileAttribute> 应用于服务操作，并在应该应用于给定操作的响应的配置文件中提供缓存配置文件的名称。

在示例服务项目的 Service.cs 文件中， `GetCustomer` 和 `GetCustomers` 操作都用来标记 <xref:System.ServiceModel.Web.AspNetCacheProfileAttribute> ，后者提供缓存配置文件名称 "CacheFor60Seconds"。 在服务项目的 web.config 文件中，缓存配置文件 "CacheFor60Seconds" 在 `caching` < > 的 < > 元素下提供 `system.web` 。 对于此缓存配置文件，属性的值 `duration` 为 "60"，因此与此配置文件关联的响应将在 ASP.NET 输出缓存中缓存60秒。 此外，对于此缓存配置文件，该 `varmByParam` 属性设置为 "format"，因此具有不同 `format` 查询字符串参数值的请求会分别缓存其响应。 最后，缓存配置文件的 `varyByHeader` 属性设置为 "接受"，因此具有不同 Accept 标头值的请求会分别缓存其响应。

客户端项目中的 Program.cs 演示如何使用 <xref:System.Net.HttpWebRequest> 编写此类客户端。 请注意，这只是访问 WCF 服务的一种方式。 还可以使用其他 .NET Framework 类（例如 WCF 通道工厂和）来访问该服务 <xref:System.Net.WebClient> 。 SDK 中的其他示例（如[基本 HTTP 服务](basic-http-service.md)示例）演示如何使用这些类与 WCF 服务进行通信。

## <a name="to-run-the-sample"></a>运行示例

该示例由三个项目组成：

- **服务**：一个 Web 应用程序项目，该项目包含 ASP.NET 中承载的 WCF HTTP 服务。

- **客户端**：使调用服务的控制台应用程序项目。

- **常见**：一个共享库，其中包含客户端和服务使用的客户类型。

在客户端控制台应用程序运行时，客户端会对服务进行请求，并将响应中的相关信息写入控制台窗口。

#### <a name="to-run-the-sample"></a>运行示例

1. 打开 ASP.NET 缓存集成示例的解决方案。

2. 按 CTRL+SHIFT+B 生成解决方案。

3. 如果 "**解决方案资源管理器**" 窗口尚未打开，请按 CTRL + W + S。

4. 从 "**解决方案资源管理器**" 窗口中，右键单击**服务**项目，然后选择 "**启动新实例**"。 这将启动承载服务的 ASP.NET 开发服务器。

5. 从 "**解决方案资源管理器**" 窗口中，右键单击**客户端**项目，然后选择 "**启动新实例**"。

6. 出现客户端控制台窗口，该窗口提供了正在运行的服务的 URI，以及该服务的 HTML 帮助页的 URI。 可随时通过在浏览器中键入 HTML 帮助页的 URI 来查看该帮助页。

7. 在示例运行时，客户端将写入当前活动的状态。

8. 按任意键可终止客户端控制台应用程序。

9. 按 Shift+F5 可停止调试服务。

10. 在 Windows 通知区域中，右键单击 "ASP.NET 开发服务器" 图标，然后选择 "**停止**"。
