---
title: 与 COM+ 应用程序集成的概述
ms.date: 03/30/2017
helpviewer_keywords:
- Windows Communication Foundation, COM+ integration
- WCF, COM+ integration
ms.assetid: e481e48f-7096-40eb-9f20-7f0098412941
ms.openlocfilehash: 1b9b7e57760c2aba0a8e9eadd53ca8e72529b787
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96248963"
---
# <a name="integrating-with-com-applications-overview"></a>与 COM+ 应用程序集成的概述

Windows Communication Foundation (WCF) 提供了一个用于创建分布式应用程序的丰富环境。 如果已在使用 COM + 中承载的基于组件的应用程序逻辑，则可以使用 WCF 来扩展现有的逻辑，而不必重写它。 最常见的情形是通过 Web 服务来公开现有 COM+ 或企业服务业务逻辑。  
  
 在 COM+ 组件上的接口作为 Web 服务公开时，这些服务的规范和协定取决于在应用程序初始化时执行的自动映射。 下面的列表演示此映射的概念模型：  
  
- 为每个公开的 COM 类定义一个服务。  
  
- 服务的协定直接派生自选定组件的接口定义，并可能在配置中定义方法排除。  
  
- 该协定中的操作直接派生自组件接口定义中的方法。  
  
- 这些操作的参数直接派生自与组件的方法参数对应的 COM 互操作性类型。  
  
 服务的默认地址和传输绑定是在服务配置文件中提供的，但可以根据需要重新配置。  
  
> [!NOTE]
> 只要 COM+ 接口和配置保持不变，公开的 Web 服务的协定就会保持不变。 如果修改多个接口，将不会自动更新可用服务，并且需要重新运行 COM+ 服务模块配置工具 (ComSvcConfig.exe)。  
  
 用作 Web 服务时，会继续强制施加 COM+ 应用程序及其组件的身份验证和授权要求。  
  
 如果调用方启动 Web 服务事务，则标记为事务性的组件将在该事务范围内登记。  
  
 在不修改 COM+ 组件的情况下将该组件的接口作为 Web 服务公开时，需要执行下列步骤：  
  
1. 确定是否可将 COM+ 组件的接口作为 Web 服务公开。  
  
2. 选择相应的宿主模式。  
  
3. 使用 COM+ 服务模块配置工具 (ComSvcConfig.exe) 为该接口添加 Web 服务。 有关如何使用 ComSvcConfig.exe 的详细信息，请参阅 [如何：使用 COM + 服务模型配置工具](how-to-use-the-com-service-model-configuration-tool.md)。  
  
4. 在应用程序配置文件中配置任何其他服务设置。 有关如何配置组件的详细信息，请参阅 [如何：配置 COM + 服务设置](how-to-configure-com-service-settings.md)。  
  
## <a name="supported-interfaces"></a>支持的接口  

 对可作为 Web 服务公开的接口的类型有一些限制。 不支持以下类型的接口：  
  
- 将对象引用作为参数传递的接口 –“受限对象引用支持”一节中介绍了以下受限对象引用方法。  
  
- 传递与 .NET Framework COM 互操作性转换不兼容的类型的接口。  
  
- 由 COM+ 承载时启用应用程序池的应用程序的接口。  
  
- 标记为应用程序专用的组件的接口。  
  
- COM+ 基础结构接口。  
  
- 系统应用程序的接口。  
  
- 尚未添加到全局程序集缓存中的企业服务组件的接口。  
  
### <a name="limited-object-reference-support"></a>受限对象引用支持  

 由于许多已部署的 COM+ 组件需要根据引用参数来使用对象（如返回 ADO Recordset 对象），因此 COM+ 集成为对象引用参数提供了有限支持。 该支持仅限于实现 `IPersistStream` COM 接口的对象。 其中包括 ADO Recordset 对象，并且可针对应用程序特定的 COM 对象实现。  
  
 为了实现此支持，ComSvcConfig.exe 工具提供了 **allowreferences** 开关，该开关可禁用常规方法签名参数并检查该工具是否运行以确保不使用对象引用参数。 此外，你作为参数传递的对象类型必须在 <> 配置元素内进行命名和标识， `persistableTypes` 该元素是 <> 元素的子元素 `comContract` 。  
  
 使用此功能时，COM+ 集成服务使用 `IPersistStream` 接口来序列化或反序列化对象实例。 如果对象实例不支持 `IPersistStream`，则会引发异常。  
  
 在客户端应用程序内，可以使用 <xref:System.ServiceModel.ComIntegration.PersistStreamTypeWrapper> 对象上的方法将对象传入服务，同样也可以用于检索对象。  
  
> [!NOTE]
> 由于序列化方法的自定义和平台特定性质，这最适用于 WCF 客户端和 WCF 服务之间的使用。  
  
## <a name="selecting-the-hosting-mode"></a>选择宿主模式  

 COM+ 可以通过以下宿主模式之一来公开 Web 服务：  
  
- COM+ 承载  
  
     Web 服务承载于应用程序的专用 COM+ 服务器进程 (Dllhost.exe) 中。 此模式要求显式启动应用程序才能接收 Web 服务请求。 可以使用 COM+ 选项“Run as an NT Service”[作为 NT 服务运行]或“Leave running when idle”[空闲时保持运行]来防止在空闲时关闭应用程序及其服务。 此模式对服务器应用程序同时提供 Web 服务和 DCOM 访问。  
  
- Web 承载  
  
     Web 服务承载于 Web 服务器工作进程中。 此模式不要求 COM+ 在接收初始请求时处于活动状态。 如果在接收该请求时应用程序处于非活动状态，则在处理该请求之前会自动激活它。 此模式也对服务器应用程序同时提供 Web 服务和 DCOM 访问，但会导致 Web 服务请求产生进程跃点。 这通常要求客户端启用模拟。 在 WCF 中，可以使用类的属性来完成此操作 <xref:System.ServiceModel.Security.WindowsClientCredential.AllowedImpersonationLevel%2A> <xref:System.ServiceModel.Security.WindowsClientCredential> ，该类可作为泛型类的属性进行访问 <xref:System.ServiceModel.ChannelFactory%601> ，也可用于 <xref:System.Security.Principal.TokenImpersonationLevel.Impersonation> 枚举值。  
  
- 进程内 Web 承载  
  
     Web 服务和 COM+ 应用程序逻辑承载于 Web 服务器工作进程中。 这样可以自动激活 Web 承载模式，而不会导致 Web 服务请求产生进程跃点。 其缺点在于无法通过 DCOM 访问服务器应用程序。  
  
### <a name="security-considerations"></a>安全注意事项  

 与其他 WCF 服务类似，公开服务的安全设置通过 WCF 通道的配置设置进行管理。 不会强制执行传统的 DCOM 安全设置，如 DCOM 计算机范围内的权限设置。 若要强制执行 COM+ 应用程序角色，必须为组件启用“组件级别访问检查”授权。  
  
 使用非安全绑定可能会致使通信被篡改或信息泄漏。 为防止发生此类情况，建议您使用安全绑定。  
  
 对于 COM+ 承载模式和 Web 承载模式，客户端应用程序必须允许服务器进程模拟客户端用户。 可以通过将模拟级别设置为来在 WCF 客户端中完成此操作 <xref:System.Security.Principal.TokenImpersonationLevel.Impersonation> 。  
  
 通过 Internet 信息服务 (IIS) 或使用 HTTP 传输的 Windows 进程激活服务 (WAS)，Httpcfg.exe 工具可用于保留传输终结点地址。 在其他配置中，务必防止非法服务充当既定的服务，这一点相当重要。 若要防止非法服务在所需终结点启动，可以将合法服务配置为作为 NT 服务来运行。 这样，合法服务就可以先于任何非法服务来声明终结点地址。  
  
 当使用配置的 COM + 角色作为 Web 承载的服务公开 COM + 应用程序时，必须将 "启动 IIS 进程帐户" 添加到该应用程序的其中一个角色。 必须先添加此帐户（名称通常为 IWAM_machinename）才能在使用之后完全关闭对象。 不应授予此帐户任何附加权限。  
  
 COM+ 进程回收功能不能在集成应用程序上使用。 如果该应用程序配置为使用进程回收功能，并且组件正在 COM+ 承载的进程中运行，则服务将无法启动。 此需求不包括使用进程内 Web 承载模式的服务，这是因为没有应用进程回收设置。  
  
## <a name="see-also"></a>另请参阅

- [COM 应用程序集成概述](integrating-with-com-applications-overview.md)
