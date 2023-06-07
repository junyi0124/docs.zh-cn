---
title: 配置 Windows 进程激活服务以用于 Windows Communication Foundation
ms.date: 03/30/2017
ms.assetid: 1d50712e-53cd-4773-b8bc-a1e1aad66b78
ms.openlocfilehash: 2f84afba72e5260a44726dcc812401da5475679f
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96284090"
---
# <a name="configuring-the-windows-process-activation-service-for-use-with-windows-communication-foundation"></a>配置 Windows 进程激活服务以用于 Windows Communication Foundation

本主题介绍设置 Windows 进程激活服务所需的步骤， (在 Windows Vista 中也称为) ，以承载 Windows Communication Foundation (不通过 HTTP 网络协议进行通信的 WCF) 服务。 下面的部分略述此配置的步骤：  
  
- 安装 (或确认 WCF 激活组件) 的安装。  
  
- 创建一个具有要使用的网络协议绑定的 WAS 站点，或者向现有站点添加新协议绑定。  
  
- 创建一个应用程序以承载服务，并使该应用程序可以使用所需的网络协议。  
  
- 生成公开非 HTTP 终结点的 WCF 服务。  
  
## <a name="configuring-a-site-with-non-http-bindings"></a>使用非 HTTP 绑定配置站点  

 若要将非 HTTP 绑定与 WAS 一起使用，必须将站点绑定添加到 WAS 配置。 WAS 的配置存储是 applicationHost.config 文件，该文件位于 %windir%\system32\inetsrv\config 目录中。 此配置存储由 WAS 和 IIS 7.0 共享。  
  
 applicationHost.config 是一个 XML 文本文件，可以使用任何标准文本编辑器（如记事本）打开。 但是，IIS 7.0 命令行配置工具 ( # A0) 是添加非 HTTP 网站绑定的首选方法。  
  
 下面的命令使用 appcmd.exe 将 net.tcp 站点绑定添加到默认网站（将此命令作为单独的一行输入）。  
  
```console  
appcmd.exe set site "Default Web Site" -+bindings.[protocol='net.tcp',bindingInformation='808:*']  
```  
  
 通过将下面指示的行添加到 applicationHost.config 文件，此命令将新 net.tcp 绑定添加到默认网站。  
  
```xml  
<sites>  
    <site name="Default Web Site" id="1">  
        <bindings>  
            <binding protocol="HTTP" bindingInformation="*:80:" />  
            //The following line is added by the command.  
            <binding protocol="net.tcp" bindingInformation="808:*" />  
        </bindings>  
    </site>  
</sites>  
```  
  
## <a name="enabling-an-application-to-use-non-http-protocols"></a>使应用程序可以使用非 HTTP 协议  

 可以启用或禁用单个网络 protocolsat 应用程序级别。 下面的命令说明如何为在 `Default Web Site` 中运行的应用程序同时启用 HTTP 和 net.tcp 协议。  
  
```console  
appcmd.exe set app "Default Web Site/appOne" /enabledProtocols:net.tcp  
```  
  
 还可以在 \<applicationDefaults> 存储在 ApplicationHost.config 中的站点 XML 配置的元素中设置启用的协议的列表。  
  
 摘自 applicationHost.config 的以下 XML 代码说明一个已同时绑定到 HTTP 协议和非 HTTP 协议的站点。 支持非 HTTP 协议所需的其他配置通过注释进行了突出。  
  
```xml  
<sites>  
    <site name="Default Web Site" id="1">  
      <application path="/">  
        <virtualDirectory path="/" physicalPath="D:\inetpub\wwwroot" />  
      </application>  
      <bindings>  
            <!-- The following two lines are added by the command. -->
            <binding protocol="HTTP" bindingInformation="*:80:" />  
            <binding protocol="net.tcp" bindingInformation="808:*" />  
       </bindings>  
    </site>  
    <siteDefaults>  
        <logFile
        customLogPluginClsid="{FF160663-DE82-11CF-BC0A-00AA006111E0}"  
          directory="D:\inetpub\logs\LogFiles" />  
        <traceFailedRequestsLogging
          directory="D:\inetpub\logs\FailedReqLogFiles" />  
    </siteDefaults>  
    <applicationDefaults
      applicationPool="DefaultAppPool"
      <!-- The following line is inserted by the command. -->
      enabledProtocols="http, net.tcp" />  
    <virtualDirectoryDefaults allowSubDirConfig="true" />  
</sites>  
```  
  
 如果您尝试通过用于非 HTTP 激活的 WAS 来激活服务，并且您未安装和配置 WAS，您可能会看到以下错误：  
  
```output  
[InvalidOperationException: The protocol 'net.tcp' does not have an implementation of HostedTransportConfiguration type registered.]   System.ServiceModel.AsyncResult.End(IAsyncResult result) +15778592   System.ServiceModel.Activation.HostedHttpRequestAsyncResult.End(IAsyncResult result) +15698937   System.ServiceModel.Activation.HostedHttpRequestAsyncResult.ExecuteSynchronous(HttpApplication context, Boolean flowContext) +265   System.ServiceModel.Activation.HttpModule.ProcessRequest(Object sender, EventArgs e) +227   System.Web.SyncEventExecutionStep.System.Web.HttpApplication.IExecutionStep.Execute() +80   System.Web.HttpApplication.ExecuteStep(IExecutionStep step, Boolean& completedSynchronously) +171  
```  
  
 如果您看到此错误，确保已安装并正确配置了用于非 HTTP 激活的 WAS。 有关详细信息，请参阅 [如何：安装和配置 WCF 激活组件](how-to-install-and-configure-wcf-activation-components.md)。  
  
## <a name="building-a-wcf-service-that-uses-was-for-non-http-activation"></a>生成一个将 WAS 用于非 HTTP 激活的 WCF 服务  

 执行安装和配置 WAS 的步骤后 (参阅 [如何：安装和配置 WCF 激活组件](how-to-install-and-configure-wcf-activation-components.md)) ，将服务配置为使用 WAS 用于激活类似于配置在 IIS 中托管的服务。  
  
 有关生成已激活的 WCF 服务的详细说明，请参阅 [如何：在 was 中承载 WCF 服务](how-to-host-a-wcf-service-in-was.md)。  
  
## <a name="see-also"></a>另请参阅

- [在 Windows 进程激活服务中承载](hosting-in-windows-process-activation-service.md)
- [Windows Server App Fabric 承载功能](/previous-versions/appfabric/ee677189(v=azure.10))
