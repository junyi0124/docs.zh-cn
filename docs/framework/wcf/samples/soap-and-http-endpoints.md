---
title: SOAP 和 HTTP 终结点
ms.date: 03/30/2017
ms.assetid: e3c8be75-9dda-4afa-89b6-a82cb3b73cf8
ms.openlocfilehash: 9e7ce32a0f5a2f37294db57659e2b30b364bef24
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96268256"
---
# <a name="soap-and-http-endpoints"></a>SOAP 和 HTTP 终结点

此示例演示如何使用 WCF Web 编程模型实现基于 RPC 的服务并以 SOAP 格式和 "纯旧 XML" (POX) 格式公开此服务。 有关服务的 HTTP 绑定的更多详细信息，请参阅 [基本 HTTP 服务](basic-http-service.md) 示例。 本示例重点介绍有关使用不同绑定通过 SOAP 和 HTTP 公开相同服务的详细信息。  
  
## <a name="demonstrates"></a>演示  

 使用 WCF 通过 SOAP 和 HTTP 公开 RPC 服务。  
  
## <a name="discussion"></a>讨论 (Discussion)  

 此示例由两个组件组成：一个 Web 应用程序项目 (服务) ，其中包含一个 (客户端) ，使用 SOAP 和 HTTP 绑定调用服务操作。  
  
 WCF 服务公开2个操作– `GetData` 并 `PutData` 回显作为输入传递的字符串。 这些服务操作使用 <xref:System.ServiceModel.Web.WebGetAttribute> 和 <xref:System.ServiceModel.Web.WebInvokeAttribute> 进行批注。 这些特性控制这些操作的 HTTP 投影。 此外，这些操作使用 <xref:System.ServiceModel.OperationContractAttribute> 进行批注，从而使其可以通过 SOAP 绑定进行公开。 服务的 `PutData` 方法引发 <xref:System.ServiceModel.Web.WebFaultException>，该异常使用 HTTP 状态代码通过 HTTP 发回，通过 SOAP 作为 SOAP 错误发回。  
  
 Web.config 文件将 WCF 服务配置为3个终结点：  
  
- ~/service.svc/mex 终结点，公开服务元数据供基于 SOAP 的客户端访问。  
  
- ~/service.svc/http 终结点，使客户端可以使用 HTTP 绑定访问服务。  
  
- ~/service.svc/soap 终结点，使客户端可以使用 SOAP over HTTP 绑定访问服务。  
  
 使用 `webHttp` 设置为的 <> 标准终结点配置 HTTP 终结点 `helpEnabled` `true` 。 因此，服务在 ~/service.svc/http/help 上公开基于 XHTML 的页面，基于 HTTP 的客户端可以使用该页面访问服务。  
  
 客户端项目演示如何使用通过 **添加服务引用**) 生成的 SOAP 代理 (来访问服务，并使用访问服务 <xref:System.Net.WebClient> 。  
  
 该示例包含一个 Web 承载的服务和一个控制台应用程序。 在控制台应用程序运行时，客户端会对服务进行请求，并将响应中的相关信息写入控制台窗口。  
  
#### <a name="to-run-the-sample"></a>运行示例  
  
1. 打开 SOAP 和 HTTP 终结点示例的解决方案。  
  
2. 按 CTRL+SHIFT+B 生成解决方案。  
  
3. 如果该窗口尚未打开，请按 CTRL + W，S 打开 **解决方案资源管理器** 窗口。  
  
4. 在 " **解决方案资源管理器** " 窗口中，右键单击 **服务** 项目，将光标放在 " **调试** " 上下文菜单选项上，以显示 " **启动新实例** " 上下文菜单。 单击 " **启动新实例**"。 这将启动承载服务的 ASP.NET 开发服务器。  
  
5. 在解决方案资源管理器窗口中，右键单击客户端项目，然后将光标放在 " **调试** " 上下文菜单选项上以显示 " **启动新实例** " 上下文菜单。 单击 " **启动新实例**"。  
  
6. 出现客户端控制台窗口，该窗口提供了正在运行的服务的 URI，以及该服务的 HTML 帮助页的 URI。 可随时通过在浏览器中键入 HTML 帮助页的 URI 来查看该帮助页。  
  
7. 在示例运行时，客户端将写入当前活动的状态。  
  
8. 按任意键可终止客户端控制台应用程序。  
  
9. 按 Shift+F5 可停止调试服务。  
  
10. 在 Windows 通知区域中，右键单击 ASP.NET 开发服务器图标，然后从上下文菜单中选择 " **停止** "。  
  
> [!IMPORTANT]
> 您的计算机上可能已安装这些示例。 在继续操作之前，请先检查以下（默认）目录：  
>
> `<InstallDrive>:\WF_WCF_Samples`  
>
> 如果此目录不存在，请参阅[Windows Communication Foundation (wcf) ，并 Windows Workflow Foundation (的 WF](https://www.microsoft.com/download/details.aspx?id=21459)) .NET Framework Windows Communication Foundation ([!INCLUDE[wf1](../../../../includes/wf1-md.md)] 此示例位于以下目录：  
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Web\SoapAndHttpEndpoints`
