---
title: SystemWebRouting 集成示例
ms.date: 03/30/2017
ms.assetid: f1c94802-95c4-49e4-b1e2-ee9dd126ff93
ms.openlocfilehash: 04c3093097c5bf11e1d4dd5d3124c9fbae4b3665
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96293905"
---
# <a name="systemwebrouting-integration-sample"></a>SystemWebRouting 集成示例

此示例演示 <xref:System.Web.Routing> 命名空间中承载层与类的集成。 通过 <xref:System.Web.Routing> 命名空间中的类，应用程序可以使用不直接与物理资源对应的 URL。 使用 Web 路由，开发人员可以创建 HTTP 的虚拟地址，然后将其映射回实际的 WCF 服务。 如果必须以不使用物理文件或资源的方式承载 WCF 服务，或者必须使用不包含 .html 或 .aspx 这类文件的 URL 访问服务时，这会十分有用。 此示例演示如何利用 <xref:System.Web.Routing.RouteTable> 类创建映射到 global.asax 中定义的运行服务的虚拟 URI。

> [!NOTE]
> <xref:System.Web.Routing> 命名空间中的类仅适用于 HTTP 上承载的服务。  
  
此示例使用 WCF 创建两个 RSS 源：一个 `movies` 源和一个 `channels` 源。 用于激活服务的 Url 不包含扩展，并且是 `Application_Start` 从类派生的类的方法中注册的 `Global` <xref:System.Web.HttpApplication> 。  
  
> [!NOTE]
> 此示例仅适用于 Internet Information Services (IIS) 7.0 及更高版本，因为 IIS 6.0 使用不同方法来支持无扩展的 Url。  

#### <a name="to-download-this-sample"></a>下载此示例
  
此示例可能已安装在您的计算机上。 在继续操作之前，请先检查以下（默认）目录：  

`<InstallDrive>:\WF_WCF_Samples`  

 如果此目录不存在，请参阅[Windows Communication Foundation (wcf) ，并 Windows Workflow Foundation (的 WF](https://www.microsoft.com/download/details.aspx?id=21459)) .NET Framework Windows Communication Foundation ([!INCLUDE[wf1](../../../../includes/wf1-md.md)] 此示例位于以下目录：  

`<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Services\Hosting\WebRoutingIntegration`  
  
#### <a name="to-use-this-sample"></a>使用此示例  
  
1. 使用 Visual Studio 打开 WebRoutingIntegration 文件。  
  
2. 若要运行解决方案并启动 Web 开发服务器，请按 F5。  
  
     出现该示例的目录列表。 请注意，不存在具有 .svc 文件扩展名的文件。  
  
3. 在地址栏中，将添加 `movies` 到 URL，使其读取 `http://localhost:[port]/movies` 并按 enter。  
  
     电影源显示在浏览器中。  
  
4. 在地址栏中，将添加 `channels` 到 URL，以便读取 `http://localhost:[port]/channels` 并按 enter。  
  
     通道源显示在浏览器中。  
  
5. 按 Alt+F4 关闭 Web 浏览器。  
  
     如果尚未退出开发服务器，请右键单击通知区域图标，然后选择 " **停止**"。  
  
#### <a name="to-use-this-sample-when-hosted-in-iis"></a>在 IIS 中承载时使用此示例  
  
1. 使用 Visual Studio 打开 WebRoutingIntegration 文件。  
  
2. 按 Ctrl+Shift+B 生成项目。  
  
3. 在 Internet 信息服务 (IIS) 管理器中创建 Web 应用程序。  
  
    1. 在 IIS 管理器中，右键单击 " **默认** 网站"，然后选择 " **添加应用程序**"。  
  
    2. 对于 " **别名**"，键入 `WebRoutingIntegration` 。  
  
    3. 对于 " **物理路径**"，选择项目中的服务文件夹。  
  
    4. 按“确定”。  
  
4. 启动应用程序，方法是右键单击 Web 应用程序并选择 " **管理应用程序** "，然后单击 " **浏览**"。  
  
5. 在地址栏中，将添加 `movies` 到 URL，以便读取 `http://localhost:[port]/movies` 并按 enter。  
  
     电影源显示在浏览器中。  
  
6. 在地址栏中，将添加 `channels` 到 URL，以便读取 `http://localhost:[port]/channels` 并按 enter。  
  
     通道源显示在浏览器中。  
  
7. 按 Alt+F4 关闭 Web 浏览器。  
  
 此示例演示可以在承载层中使用 <xref:System.Web.Routing> 命名空间中的类进行编写，以便对 HTTP 上承载的服务的请求进行路由。  
  
> [!NOTE]
> 如果默认应用程序池版本设置为版本2，则必须将其更新为 .NET Framework 4。  
  
## <a name="see-also"></a>另请参阅

- [AppFabric 承载和持久性示例](/previous-versions/appfabric/ff383418(v=azure.10))
