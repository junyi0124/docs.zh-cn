---
title: WCF 服务和 ASP.NET
description: 了解如何并行承载 WCF 服务和 ASP.NET，并在 ASP.NET 兼容模式下承载 WCF 服务。
ms.date: 03/30/2017
ms.assetid: b980496a-f0b0-4319-8e55-a0f0fa32da70
ms.openlocfilehash: 765a509f94a0a934cdbbf0212cfc1d4053d29f9c
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90553312"
---
# <a name="wcf-services-and-aspnet"></a>WCF 服务和 ASP.NET

本主题讨论如何将 WCF) 服务与 ASP.NET 并行宿主 (Windows Communication Foundation 并在 ASP.NET 兼容模式下承载它们。

## <a name="hosting-wcf-side-by-side-with-aspnet"></a>并行承载 WCF 与 ASP.NET

 (IIS) 中承载的 WCF 服务可以通过查找 Internet Information Services。单个公共应用程序域内的 ASPX 页和 .ASMX Web 服务。 ASP.NET 为 WCF 和 ASP.NET HTTP 运行时提供常见基础结构服务，如 AppDomain 管理和动态编译。 WCF 的默认配置与 ASP.NET 并行。

![显示 WCF 服务和 ASP .NET：共享状态的屏幕截图。](./media/wcf-services-and-aspnet/windows-communication-foundation-services-asp-dotnet-configuration.gif)

ASP.NET HTTP 运行时处理 ASP.NET 请求，但不参与处理发送到 WCF 服务的请求，即使这些服务托管在与 ASP.NET 内容相同的 AppDomain 中。 相反，WCF 服务模型会截获寻址到 WCF 服务的消息，并通过 WCF 传输/通道堆栈路由它们。

并行模型的结果如下：

- ASP.NET 和 WCF 服务可以共享 AppDomain 状态。 由于这两个框架可以共存于同一 AppDomain 中，因此 WCF 还可以与 ASP.NET 共享 AppDomain 状态， (包括静态变量、事件等) 。

- WCF 服务的行为一致，与宿主环境和传输无关。 ASP.NET HTTP 运行库与 IIS/ASP.NET 宿主环境和 HTTP 通信是有意耦合在一起的。 相反，WCF 旨在跨宿主环境保持一致 (WCF 在) IIS 内部和外部的行为一致， (在 IIS 7.0 和更高版本中承载的服务在其公开的所有终结点上具有一致的行为，即使某些终结点使用 HTTP) 以外的协议也是如此。

- 在 AppDomain 中，由 HTTP 运行时实现的功能适用于 ASP.NET 内容而不是 WCF。 ASP.NET 应用程序平台的许多 HTTP 特定功能不适用于在包含 ASP.NET 内容的 AppDomain 中承载的 WCF 服务。 这些功能的示例包括：

  - HttpContext： <xref:System.Web.HttpContext.Current%2A> 始终是 `null` 从 WCF 服务中访问时。 请改用 <xref:System.ServiceModel.Channels.RequestContext>。

  - 基于文件的授权：在决定是否授权服务请求时，WCF 安全模型不允许访问控制列表 (ACL) 应用于服务的 .svc 文件。

  - 基于配置的 URL 授权：同样，WCF 安全模型不遵循 System.web 的配置元素中指定的任何基于 URL 的授权规则 \<authorization> 。 如果服务驻留在受 ASP 保护的 URL 空间中，则 WCF 请求将忽略这些设置。NET 的 URL 授权规则。

  - HttpModule 扩展性：在引发事件时，WCF 宿主基础结构会截获 WCF 请求 <xref:System.Web.HttpApplication.PostAuthenticateRequest> ，且不会将处理返回给 ASP.NET HTTP 管道。 编码为在管道的更高阶段截获请求的模块不会截获 WCF 请求。

  - ASP.NET 模拟：默认情况下，WCF 请求始终作为 IIS 进程标识运行，即使将 ASP.NET 设置为使用 System.web 的配置选项启用模拟也是如此 \<identity impersonate="true" /> 。

这些限制仅适用于在 IIS 应用程序中承载的 WCF 服务。 ASP.NET 内容的行为不受 WCF 的存在的影响。

需要传统地由 HTTP 管道提供的功能的 WCF 应用程序应考虑使用 WCF 等效项，这是独立于主机和传输的：

- <xref:System.ServiceModel.OperationContext>，而不是 <xref:System.Web.HttpContext>。

- <xref:System.ServiceModel.Description.ServiceAuthorizationBehavior>，而不是 ASP.NET 的文件/URL 授权。

- <xref:System.ServiceModel.Dispatcher.IDispatchMessageInspector> 或自定义分层通道，而不是 HTTP 模块。

- 使用 WCF 而不是 System.web 模拟来模拟每个操作。

或者，你可以考虑在 WCF 的 ASP.NET 兼容模式下运行你的服务。

## <a name="hosting-wcf-services-in-aspnet-compatibility-mode"></a>在 ASP.NET 兼容模式下承载 WCF 服务

尽管 WCF 模型设计为跨宿主环境和传输实现一致的行为，但在某些情况下，应用程序不需要这种灵活性。 WCF 的 ASP.NET 兼容模式适用于不需要在 IIS 外部承载或通过 HTTP 以外的协议进行通信但使用 ASP.NET Web 应用程序平台的所有功能的方案。

不同于默认的并行配置，其中，WCF 宿主基础结构会截获 WCF 消息并将其路由到 HTTP 管道，在 ASP.NET 兼容模式下运行的 WCF 服务完全参与 ASP.NET HTTP 请求生命周期。 在兼容模式下，WCF 服务通过实现使用 HTTP 管道 <xref:System.Web.IHttpHandler> ，这与处理 ASPX 页和 .Asmx Web 服务的请求的方式类似。 因此，WCF 的行为与对以下 ASP.NET 功能的行为完全相同：

- <xref:System.Web.HttpContext>：在 ASP.NET 兼容模式下运行的 WCF 服务可以访问 <xref:System.Web.HttpContext.Current%2A> 及其关联状态。

- 基于文件的授权：在 ASP.NET 兼容模式下运行的 WCF 服务可以通过附加文件系统访问控制列表 (Acl) 到服务的 .svc 文件来保证安全。

- 可配置的 URL 授权： ASP。当 WCF 服务在 ASP.NET 兼容模式下运行时，将对 WCF 请求强制执行 NET URL 授权规则。

- <xref:System.Web.HttpModuleCollection> 扩展性：由于在 ASP.NET 兼容模式下运行的 WCF 服务完全参与 ASP.NET HTTP 请求生命周期，因此 HTTP 管道中配置的任何 HTTP 模块都可以在服务调用前后对 WCF 请求进行操作。

- ASP.NET 模拟：如果已为应用程序启用 ASP.NET 模拟，WCF 服务将使用 ASP.NET 模拟线程的当前标识运行，这可能不同于 IIS 进程标识。 如果为特定服务操作启用了 ASP.NET 模拟和 WCF 模拟，则服务实现最终使用从 WCF 获取的标识运行。

WCF 的 ASP.NET 兼容模式在应用程序级别通过以下配置 (在应用程序级别上启用：) 位于应用程序的 Web.config 文件中：

```xml
<system.serviceModel>
    <serviceHostingEnvironment aspNetCompatibilityEnabled="true" />
</system.serviceModel>
```

如果未指定，则此值默认为 `false` 。 的值 `false` 指示在应用程序中运行的所有 WCF 服务都不会在 ASP.NET 兼容模式下运行。

由于 ASP.NET 兼容模式意味着请求处理语义本质上不同于 WCF 默认值，因此单个服务实现可以控制它们是否在已启用 ASP.NET 兼容模式的应用程序中运行。 服务可以使用 <xref:System.ServiceModel.Activation.AspNetCompatibilityRequirementsAttribute> 来指示其是否支持 ASP.NET 兼容模式。 此特性的默认值为 <xref:System.ServiceModel.Activation.AspNetCompatibilityRequirementsMode.Allowed>。

```csharp
[AspNetCompatibilityRequirements(RequirementsMode = AspNetCompatibilityRequirementsMode.Allowed)]
public class CalculatorService : ICalculatorSession
{//Implement calculator service methods.}
```

下表演示应用程序范围的兼容模式设置如何与单独服务指定的支持级别交互：

|应用程序范围的兼容模式设置|[AspNetCompatibilityRequirementsMode]<br /><br /> 设置|观察到的结果|
|--------------------------------------------------|---------------------------------------------------------|---------------------|
|aspNetCompatibilityEnabled = " `true` "|<xref:System.ServiceModel.Activation.AspNetCompatibilityRequirementsMode.Required>|服务成功激活。|
|aspNetCompatibilityEnabled = " `true` "|<xref:System.ServiceModel.Activation.AspNetCompatibilityRequirementsMode.Allowed>|服务成功激活。|
|aspNetCompatibilityEnabled = " `true` "|<xref:System.ServiceModel.Activation.AspNetCompatibilityRequirementsMode.NotAllowed>|服务接收消息时发生激活错误。|
|aspNetCompatibilityEnabled = " `false` "|<xref:System.ServiceModel.Activation.AspNetCompatibilityRequirementsMode.Required>|服务接收消息时发生激活错误。|
|aspNetCompatibilityEnabled = " `false` "|<xref:System.ServiceModel.Activation.AspNetCompatibilityRequirementsMode.Allowed>|服务成功激活。|
|aspNetCompatibilityEnabled = " `false` "|<xref:System.ServiceModel.Activation.AspNetCompatibilityRequirementsMode.NotAllowed>|服务成功激活。|

> [!NOTE]
> IIS 7.0 和 WAS 允许 WCF 服务通过 HTTP 以外的协议进行通信。 但是，不允许在启用了 ASP.NET 兼容模式的应用程序中运行的 WCF 服务公开非 HTTP 终结点。 在服务接收其第一条消息时，这种配置会生成激活异常。

有关为 WCF 服务启用 ASP.NET 兼容模式的详细信息，请参阅 <xref:System.ServiceModel.Activation.AspNetCompatibilityRequirementsMode> 和 [ASP.NET 兼容性](../samples/aspnet-compatibility.md) 示例。

## <a name="see-also"></a>请参阅

- <xref:System.ServiceModel.Activation.AspNetCompatibilityRequirementsAttribute>
- [Windows Server App Fabric 承载功能](/previous-versions/appfabric/ee677189(v=azure.10))
