---
title: Internet 信息服务承载最佳实践
ms.date: 03/30/2017
ms.assetid: 0834768e-9665-46bf-86eb-d4b09ab91af5
ms.openlocfilehash: 419b272ea3926d215ae2eed0add699af4101e648
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96258759"
---
# <a name="internet-information-services-hosting-best-practices"></a>Internet 信息服务承载最佳实践

本主题概述了 Windows Communication Foundation (WCF) 服务中承载的最佳实践。  
  
## <a name="implementing-wcf-services-as-dlls"></a>将 WCF 服务实现为 DLL  

 如果将 WCF 服务实现为部署到 Web 应用程序的 \bin 目录的 DLL，则可以在 Web 应用程序模型之外重用该服务，例如，在可能未部署 (IIS) Internet Information Services 测试环境中。  
  
## <a name="service-hosts-in-iis-hosted-applications"></a>承载于 IIS 中的应用程序中的服务主机  

 不要使用命令式自宿主 Api 来创建新的服务主机，这些主机在 iis 宿主环境中不受本机支持的网络传输上进行侦听 (例如，IIS 6.0 来托管 TCP 服务，因为 IIS 6.0) 在本机上不支持 TCP 通信。 不建议使用此方法。 强制创建的服务主机在 IIS 承载环境内是未知的。 很重要的一点是，在 IIS 确定承载应用程序池是否空闲时，它未说明强制创建的服务所进行的处理。 结果是具有这样强制创建的服务主机的应用程序具有主动处置 IIS 托管进程的 IIS 承载环境。  
  
## <a name="uris-and-iis-hosted-endpoints"></a>URI 和承载于 IIS 中的终结点  

 承载于 IIS 中的服务的终结点应该使用相对统一资源标识符 (URI) 而不是绝对地址进行配置。 这保证终结点地址在属于承载应用程序的 URI 地址集范围内，并确保像预期的那样发生基于消息的激活。  
  
## <a name="state-management-and-process-recycling"></a>状态管理和进程回收  

 为不在内存中维护本地状态的服务优化了 IIS 承载环境。 IIS 回收托管进程以响应各种外部和内部事件，导致仅存储在内存中的任何可变状态丢失。 寄宿在 IIS 中的服务应该在进程外存储其状态（例如，在数据库中），或者在出现应用程序回收事件时可以轻松重新创建的内存中缓存中存储其状态。  
  
> [!NOTE]
> WCF 用于消息层可靠性和安全性的协议利用可变的内存中状态。 WCF 可靠会话和安全会话可能由于应用程序回收而意外终止。 使用这些协议的 IIS 托管的应用程序应该依赖于 WCF 提供的会话密钥来关联应用层状态 (例如，应用层构造或自定义相关标头) 或为托管应用程序禁用 IIS 进程回收。  
  
## <a name="optimizing-performance-in-middle-tier-scenarios"></a>在中间层方案中优化性能  

 若要在 *中间层方案* 中实现最佳性能（一项服务，该服务为响应传入消息而调用其他服务），请一次将 WCF 服务客户端实例化到远程服务，并跨多个传入请求重复使用它。 实例化 WCF 服务客户端是一项成本高昂的操作，相对于对预先存在的客户端实例进行服务调用，中间层方案通过跨请求缓存远程客户端来产生明显的性能提升。 WCF 服务客户端是线程安全的，因此不必跨多个线程同步对客户端的访问。  
  
 中间层方案还通过使用由 `svcutil /a` 选项生成的异步 API 来产生性能提升。 `/a`选项会使 "工作项[元数据实用工具" 工具 ( # A0) ](../servicemodel-metadata-utility-tool-svcutil-exe.md)为 `BeginXXX/EndXXX` 每个服务操作生成方法，这允许在后台线程上进行远程服务的可能长时间运行的调用。  
  
## <a name="wcf-in-multi-homed-or-multi-named-scenarios"></a>多宿主或多名称方案中的 WCF  

 你可以在 IIS Web 场中部署 WCF 服务，其中一组计算机共享公用外部名称 (例如 `http://www.contoso.com`) ，但由不同的主机名单独寻址 (例如， `http://www.contoso.com` 可能会将流量定向到名为和) 两个不同的计算机 `http://machine1.internal.contoso.com` `http://machine2.internal.contoso.com` 。 WCF 完全支持此部署方案，但需要对承载 WCF 服务的 IIS 网站进行特殊配置，才能在服务的元数据 (Web Services 描述语言) 中显示正确的 (外部) 主机名。  
  
 若要确保服务元数据中出现正确的主机名，WCF 将生成，请将承载 WCF 服务的 IIS 网站的默认标识配置为使用显式主机名。 例如，驻留在场中的计算机 `www.contoso.com` 应使用 *：80： www .com FOR HTTP 的 IIS 网站绑定，并使用 \* ：443： www .COM for HTTPS。  
  
 可以使用 IIS Microsoft 管理控制台 (MMC) 管理单元配置 IIS 网站绑定。  
  
## <a name="application-pools-running-in-different-user-contexts-overwrite-assemblies-from-other-accounts-in-the-temporary-folder"></a>在不同用户上下文中运行的应用程序池覆盖来自临时文件夹中其他帐户的程序集  

 若要确保在不同用户上下文中运行的应用程序池无法覆盖临时 ASP.NET files 文件夹中其他帐户的程序集，请对不同的应用程序使用不同的标识和临时文件夹。 例如，如果具有两个虚拟应用程序 /Application1 和 / Application2，则可以创建两个应用程序池 A 和 B，它们具有不同的标识。 应用程序池 A 可以使用一个用户标识 (user1) 运行，而应用程序池 B 可以使用其他用户标识 (user2) 运行，并将 /Application1 配置为使用 A，将 /Application2 配置为使用 B。  
  
 在 Web.config 中，你可以使用配置临时文件夹 \<system.web/compilation/@tempFolder> 。 对于/Application1，可以是 "c:\tempForUser1"，对于 application2，它可以是 "c:\tempForUser2"。 将相应的写入权限授予两个标识的这些文件夹。  
  
 之后，user2 无法更改 /application2 的代码生成文件夹（在 c:\tempForUser1 下）。  
  
## <a name="enabling-asynchronous-processing"></a>启用异步处理  

 默认情况下，会以同步方式处理发送到在 IIS 6.0 和更早版本下承载的 WCF 服务的消息。 ASP.NET 在 ASP.NET 工作) 线程 (的线程上调用 WCF，并使用其他线程处理请求。 WCF 在完成其处理之前会保持 ASP.NET 工作线程。 这将导致同步处理请求。 异步处理请求可实现更高的可伸缩性，因为它可减少处理请求所需的线程数–在处理请求时，WCF 不会保存到 ASP.NET 线程。 对于运行 IIS 6.0 的计算机，不建议使用异步行为，因为无法限制打开 *服务器 (DOS) 攻击的传入* 请求。 从 IIS 7.0 开始，引入了并发请求限制：`[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\ASP.NET\2.0.50727.0]"MaxConcurrentRequestsPerCpu`。 借助这一新的限制，可安全地使用异步处理。  默认情况下，在 IIS 7.0 中会注册异步处理程序和模块。 如果关闭了此功能，可在应用程序的 Web.config 文件中手动启用对请求的异步处理。 所使用的设置取决于 `aspNetCompatibilityEnabled` 设置。 如果已将 `aspNetCompatibilityEnabled` 设置为 `false`，请按照下面的配置代码段所示配置 `System.ServiceModel.Activation.ServiceHttpModule`。  
  
```xml  
<system.serviceModel>  
    <serviceHostingEnvironment aspNetCompatibilityEnabled="false" />
  </system.serviceModel>  
  <system.webServer>  
    <modules>  
      <remove name="ServiceModel"/>  
      <add name="ServiceModel"
           preCondition="integratedMode,runtimeVersionv2.0"
           type="System.ServiceModel.Activation.ServiceHttpModule, System.ServiceModel,Version=3.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089"/>  
    </modules>  
    </system.webServer>  
```  
  
 如果已将 `aspNetCompatibilityEnabled` 设置为 `true`，请按照下面的配置代码段所示配置 `System.ServiceModel.Activation.ServiceHttpHandlerFactory`。  
  
```xml  
<system.serviceModel>  
    <serviceHostingEnvironment aspNetCompatibilityEnabled="true" />
  </system.serviceModel>  
  <system.webServer>  
    <handlers>  
          <clear/>  
          <add name="TestAsyncHttpHandler"
               path="*.svc"
               verb="*"
               type="System.ServiceModel.Activation.ServiceHttpHandlerFactory, System.ServiceModel, Version=3.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089"
               />  
    </handlers>
  </system.webServer>  
```  
  
## <a name="see-also"></a>另请参阅

- [服务承载示例](../samples/hosting.md)
- [Windows Server App Fabric 承载功能](/previous-versions/appfabric/ee677189(v=azure.10))
