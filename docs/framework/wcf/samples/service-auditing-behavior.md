---
title: 服务审核行为
ms.date: 03/30/2017
ms.assetid: 59bf0cda-e496-4418-a3a1-2f0f6e85f8ce
ms.openlocfilehash: ae7ed2059b491a71de9c806e78f1fb784da197fa
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96262562"
---
# <a name="service-auditing-behavior"></a>服务审核行为

此示例演示如何在服务操作过程中使用 <xref:System.ServiceModel.Description.ServiceSecurityAuditBehavior> 来启用对安全事件的审核。 此示例基于 [入门](getting-started-sample.md)。 已使用配置服务和客户端 [\<wsHttpBinding>](../../configure-apps/file-schema/wcf/wshttpbinding.md) 。 `mode`的特性已 [\<security>](../../configure-apps/file-schema/wcf/security-of-custombinding.md) 设置为 `Message` ，并且已 `clientCredentialType` 设置为 `Windows` 。 在此示例中，客户端是一个控制台应用程序 (.exe)，服务是由 Internet 信息服务 (IIS) 承载的。  
  
> [!NOTE]
> 本主题的最后介绍了此示例的设置过程和生成说明。  
  
 服务配置文件使用 `serviceSecurityAudit` 元素来配置审核。  
  
```xml  
<behaviors>  
  <serviceBehaviors>  
    <behavior name="CalculatorServiceBehavior">  
      ...  
<!-- serviceSecurityAudit allows specification of audit location   
     and whether to audit success, failure or both. This service   
     logs success and failure of messageAuthentication   
     to the default location -->  
     <serviceSecurityAudit auditLogLocation ="Default" messageAuthenticationAuditLevel = "SuccessOrFailure" />  
    </behavior>  
  </serviceBehaviors>  
</behaviors>  
```  
  
 运行示例时，操作请求和响应将显示在客户端控制台窗口中。 在控制台窗口中按 Enter 可以关闭客户端。  
  
 通过运行事件查看器可以查看所生成的审核日志。 默认情况下，可在 Windows XP 上的应用程序日志中查看审核事件；而在 Windows Server 2003 和 Windows Vista 上，可在安全日志中查看审核事件。 可在 Windows Server 2008 和 Windows 7 上的应用程序和服务日志中查看审核事件。 通过将 `auditLogLocation` 属性设置为 "Application" 或 "Security"，可以指定审核事件的位置。 有关详细信息，请参阅 [如何：审核安全事件](../feature-details/how-to-audit-wcf-security-events.md)。 如果在安全日志中写入事件，则应将 "> LocalSecurityPolicy" 启用对象访问权限设置为 "成功" 和 "失败"。  
  
 在查看事件日志时，审核事件的来源为“ServiceModel Audit 3.0.0.0”。 消息身份验证审核记录的类别为 "MessageAuthentication"，而服务授权审核记录的类别为 "ServiceAuthorization"。  
  
 消息身份验证审核事件包含消息是否被篡改、消息是否已过期以及客户端是否可以向服务进行身份验证等信息。 这些事件还提供了有关身份验证是成功还是失败（以及客户端标识）的信息和有关消息所发送到的终结点（以及与消息关联的操作）的信息。  
  
 服务授权审核事件包含服务授权管理器所做出的授权决定。 这些事件提供了有关以下各项的信息：授权是成功还是失败以及客户端标识、消息所发送到的终结点、与消息关联的操作、从传入消息中生成的授权上下文的标识符和做出访问决定的授权管理器的类型。  
  
### <a name="to-set-up-build-and-run-the-sample"></a>设置、生成和运行示例  
  
1. 确保已对 [Windows Communication Foundation 示例执行了一次性安装过程](one-time-setup-procedure-for-the-wcf-samples.md)。  
  
2. 若要生成 C# 或 Visual Basic .NET 版本的解决方案，请按照 [Building the Windows Communication Foundation Samples](building-the-samples.md)中的说明进行操作。  
  
3. 若要以单机配置或跨计算机配置来运行示例，请按照 [运行 Windows Communication Foundation 示例](running-the-samples.md)中的说明进行操作。  
  
## <a name="see-also"></a>另请参阅

- [审核](../feature-details/auditing-security-events.md)
- [如何：审核安全事件](../feature-details/how-to-audit-wcf-security-events.md)
