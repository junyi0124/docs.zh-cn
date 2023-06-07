---
title: 审核安全事件
ms.date: 03/30/2017
helpviewer_keywords:
- auditing security events [WCF]
ms.assetid: 5633f61c-a3c9-40dd-8070-1c373b66a716
ms.openlocfilehash: 985004313c7d9843f2e9960805a6c0623d43a41f
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96234812"
---
# <a name="auditing-security-events"></a>审核安全事件

利用 Windows Communication Foundation (WCF) 创建的应用程序可以通过审核功能，记录安全事件 (是成功、失败还是同时) 两者。 这些事件被写入 Windows 系统事件日志，并且可以使用事件查看器进行检查。  
  
 审核为管理员提供了一种检测已经发生或正在发生的攻击的方式。 此外，审核有助于开发人员调试与安全相关的问题。 例如，如果授权或检查策略配置中的错误意外拒绝授权用户进行访问，开发人员可以通过检查事件日志迅速发现并隔离此错误的原因。  
  
 有关 WCF 安全的详细信息，请参阅 [安全性概述](security-overview.md)。 有关编程 WCF 的详细信息，请参阅 [基本 Wcf 编程](../basic-wcf-programming.md)。  
  
## <a name="audit-level-and-behavior"></a>审核级别和行为  

 存在两个安全审核级别：  
  
- 服务授权级别，在该级别对调用方进行授权。  
  
- 消息级别，WCF 检查消息有效性并对调用方进行身份验证。  
  
 您可以检查审核级别的成功或失败情况，这称为 *审核行为*。  
  
## <a name="audit-log-location"></a>审核日志位置  

 一旦确定审核级别和行为，您（或管理员）就可指定审核日志的位置。 有以下三种选择：Default、Application 和 Security。 当指定“默认值”时，实际日志取决于所使用的系统以及该系统是否支持写入安全日志。 有关详细信息，请参阅本主题后面的 "操作系统" 部分。  
  
 写入 Security 日志要求具有 `SeAuditPrivilege`。 默认情况下，只有“本地系统”和“网络服务”帐户具有此特权。 管理 Security 日志功能 `read` 和 `delete` 要求具有 `SeSecurityPrivilege`。 默认情况下，只有管理员具有此特权。  
  
 与此相反，经过身份验证的用户可以读取和写入应用程序日志。 默认情况下，Windows XP 将审核事件写入应用程序日志。 该日志还包含对所有通过身份验证的用户可见的个人信息。  
  
## <a name="suppressing-audit-failures"></a>禁止显示审核失败  

 审核过程中的另一个选择为是否禁止显示任何审核失败。 默认情况下，审核失败不会影响应用程序。 但是，如若需要，可将此选项设置为 `false`，这将导致引发异常。  
  
## <a name="programming-auditing"></a>审核编程  

 可以通过编程或通过配置来指定审核行为。  
  
### <a name="auditing-classes"></a>审核类  

 下表描述了用于对审核行为进行编程的类和属性。  
  
|实例|描述|  
|-----------|-----------------|  
|<xref:System.ServiceModel.Description.ServiceSecurityAuditBehavior>|将设置审核选项作为服务行为启用。|  
|<xref:System.ServiceModel.AuditLogLocation>|枚举值，用于指定要写入的日志。 可能的值为 Default、Application 和 Security。 选择 Default 时，操作系统将确定实际日志位置。 请参见本主题后面的“Application 或 Security 事件日志选择”部分。|  
|<xref:System.ServiceModel.Description.ServiceSecurityAuditBehavior.MessageAuthenticationAuditLevel%2A>|指定在消息级别审核哪些类型的消息身份验证事件。 选择包括 `None`、`Failure`、`Success` 和 `SuccessOrFailure`。|  
|<xref:System.ServiceModel.Description.ServiceSecurityAuditBehavior.ServiceAuthorizationAuditLevel%2A>|指定在服务级别审核哪些类型的服务授权事件。 选择包括 `None`、`Failure`、`Success` 和 `SuccessOrFailure`。|  
|<xref:System.ServiceModel.Description.ServiceSecurityAuditBehavior.SuppressAuditFailure%2A>|指定在审核失败时如何处理客户端请求。 例如，当服务尝试写入 Security 日志但不具有 `SeAuditPrivilege` 时。 默认值 `true` 指示忽略失败，因此将正常处理客户端请求。|  
  
 有关设置应用程序以记录审核事件的示例，请参阅 [如何：审核安全事件](how-to-audit-wcf-security-events.md)。  
  
### <a name="configuration"></a>Configuration  

 通过在下添加，还可以使用配置来指定审核行为 [\<serviceSecurityAudit>](../../configure-apps/file-schema/wcf/servicesecurityaudit.md) [\<behaviors>](../../configure-apps/file-schema/wcf/behaviors.md) 。 必须在下添加元素 [\<behavior>](../../configure-apps/file-schema/wcf/behavior-of-endpointbehaviors.md) ，如下面的代码所示。  
  
```xml  
<configuration>  
  <system.serviceModel>  
    <behaviors>  
      <behavior>  
        <!-- auditLogLocation="Application" or "Security" -->  
        <serviceSecurityAudit  
                  auditLogLocation="Application"  
                  suppressAuditFailure="true"  
                  serviceAuthorizationAuditLevel="Failure"  
                  messageAuthenticationAuditLevel="SuccessOrFailure" />
      </behavior>  
    </behaviors>  
  </system.serviceModel>  
</configuration>  
```  
  
 如果启用了审核但未指定 `auditLogLocation`，则对于支持写入 Security 日志的平台来说，默认日志名称为“Security”日志；否则为“Application”日志。 只有 Windows Server 2003 和 Windows Vista 操作系统才支持写入安全日志。 有关详细信息，请参阅本主题后面的 "操作系统" 部分。  
  
## <a name="security-considerations"></a>安全注意事项  

 如果恶意用户了解到审核功能处于启用状态，攻击者可能会发送将导致写入审核项的无效消息。 如果以这种方式填充审核日志，则审核系统会出现故障。 为了缓解此问题，请将 <xref:System.ServiceModel.Description.ServiceSecurityAuditBehavior.SuppressAuditFailure%2A> 属性设置为 `true`，然后使用事件查看器的属性来控制审核行为。  
  
 写入到 Windows XP 上的应用程序日志的审核事件对任何经过身份验证的用户可见。  
  
## <a name="choosing-between-application-and-security-event-logs"></a>选择 Application 或 Security 事件日志  

 下表提供的信息有助于您选择是记录到 Application 事件日志中还是记录到 Security 事件日志中。  
  
#### <a name="operating-system"></a>操作系统  
  
|系统|应用程序日志|安全日志|  
|------------|---------------------|------------------|  
|Windows XP SP2 或更高版本|支持|不支持|  
|Windows Server 2003 SP1 和 Windows Vista|支持|线程上下文必须具有 `SeAuditPrivilege`|  
  
#### <a name="other-factors"></a>其他因素  

 除操作系统以外，下表描述了其他用于控制是否启用日志记录的设置。  
  
|因子|应用程序日志|安全日志|  
|------------|---------------------|------------------|  
|审核策略管理|不适用。|除配置以外，Security 日志还受到本地安全机构 (LSA) 策略的控制。 还必须启用“审核对象访问”类别。|  
|默认用户体验|所有通过身份验证的用户都可以写入 Application 日志，因此对于应用程序进程，不需要执行其他权限步骤。|应用程序进程（上下文）必须具有 `SeAuditPrivilege`。|  
  
## <a name="see-also"></a>另请参阅

- <xref:System.ServiceModel.Description.ServiceSecurityAuditBehavior>
- <xref:System.ServiceModel.AuditLogLocation>
- [安全性概述](security-overview.md)
- [基本 WCF 编程](../basic-wcf-programming.md)
- [如何：审核安全事件](how-to-audit-wcf-security-events.md)
- [\<serviceSecurityAudit>](../../configure-apps/file-schema/wcf/servicesecurityaudit.md)
- [\<behaviors>](../../configure-apps/file-schema/wcf/behaviors.md)
- [Windows Server App Fabric 的安全模型](/previous-versions/appfabric/ee677202(v=azure.10))
