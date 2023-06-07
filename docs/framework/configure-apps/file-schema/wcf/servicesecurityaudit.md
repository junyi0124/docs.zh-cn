---
title: <serviceSecurityAudit>
ms.date: 03/30/2017
ms.assetid: ba517369-a034-4f8e-a2c4-66517716062b
ms.openlocfilehash: 6cec3373dae3127f16bb8a418a91a684554f2b0c
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91153659"
---
# \<serviceSecurityAudit>

指定用于在服务操作过程中启用安全事件审核的设置。  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<behaviors>**](behaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<serviceBehaviors>**](servicebehaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<behavior>**](behavior-of-servicebehaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<serviceSecurityAudit>**  
  
## <a name="syntax"></a>语法  
  
```xml  
<serviceSecurityAudit auditLogLocation="Default/Application/Security"
                      messageAuthenticationAuditLevel="None/Success/Failure/SuccessOrFailure"
                      serviceAuthorizationAuditLevel="None/Success/Failure/SuccessOrFailure"
                      suppressAuditFailure="Boolean" />
```  
  
## <a name="attributes-and-elements"></a>特性和元素  

 下列各节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  
  
|属性|描述|  
|---------------|-----------------|  
|auditLogLocation|指定审核日志的位置。 有效值包括以下值：<br /><br /> -默认：将安全事件写入 Windows XP 上的应用程序日志以及 Windows Server 2003 和 Windows Vista 上的事件日志。<br />-应用程序：审核事件将写入到应用程序事件日志中。<br />-Security：审核事件将写入安全事件日志。<br /><br /> 默认值为 Default。 有关详细信息，请参阅 <xref:System.ServiceModel.AuditLogLocation>。|  
|suppressAuditFailure|一个布尔值，指定取消显示审核日志写入失败的行为。<br /><br /> 应将对审核日志的写入错误通知给应用程序。 如果应用程序并不用于处理审核错误，则应使用此属性取消显示审核日志写入失败。<br /><br /> 如果此属性为 `true`，则因尝试写入审核事件而导致的异常（OutOfMemoryException、StackOverFlowException、ThreadAbortException 和 ArgumentException 除外）将由系统进行处理并且不会传播到应用程序。 如果此属性为 `false`，则因尝试写入审核事件而导致的所有异常都将向上传递给应用程序。<br /><br /> 默认为 `true`。|  
|serviceAuthorizationAuditLevel|指定审核日志中记录的授权事件的类型。 有效值包括以下值：<br /><br /> -None：不执行任何服务授权事件的审核。<br />-Success：仅审核成功的服务授权事件。<br />-失败：仅审核失败的服务授权事件。<br />-SuccessOrFailure：审核成功和失败的服务授权事件。<br /><br /> 默认值为 None。 有关详细信息，请参阅 <xref:System.ServiceModel.AuditLevel>。|  
|messageAuthenticationAuditLevel|指定所记录的消息身份验证审核事件的类型。 有效值包括以下值：<br /><br /> -None：不生成审核事件。<br />-Success：仅记录成功安全 (完全验证，包括消息签名验证、密码和令牌验证) 事件。<br />-失败：只记录失败事件。<br />-SuccessOrFailure：成功和失败事件都被记录。<br /><br /> 默认值为 None。 有关详细信息，请参阅 <xref:System.ServiceModel.AuditLevel>。|  
  
### <a name="child-elements"></a>子元素  

 无。  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<behavior>](behavior-of-endpointbehaviors.md)|指定行为元素。|  
  
## <a name="remarks"></a>备注  

 此配置元素用于审核 Windows Communication Foundation (WCF) 身份验证事件。 当启用了审核时，可对成功或失败的身份验证尝试（或这两者）进行审核。 事件会写入到三个事件日志之一：操作系统版本的应用程序日志、安全日志或默认日志。 可以使用 Windows 事件查看器查看所有事件日志。  
  
 有关使用此配置元素的详细示例，请参阅 [服务审核行为](../../../wcf/samples/service-auditing-behavior.md)。  
  
 默认情况下，可在 Windows XP 上的应用程序日志中查看审核事件；而在 Windows Server 2003 和 Windows Vista 上，可在安全日志中查看审核事件。 通过将 `auditLogLocation` 属性设置为“Application”或“Security”，可以指定审核事件的位置。 有关详细信息，请参阅 [如何：审核安全事件](../../../wcf/feature-details/how-to-audit-wcf-security-events.md)。 如果在安全日志中写入事件，则应将 > LocalSecurityPolicy 启用对象访问设置为 "成功" 和 "失败"。  
  
 在查看事件日志时，审核事件的来源为“ServiceModel Audit 3.0.0.0”。 消息身份验证审核记录的类别为“MessageAuthentication”，而服务授权审核记录的类别为“ServiceAuthorization”。  
  
 消息身份验证审核事件包含消息是否被篡改、消息是否已过期以及客户端是否可以向服务进行身份验证等信息。 这些事件还提供了有关身份验证是成功还是失败（以及客户端标识）的信息和有关消息所发送到的终结点（以及与消息关联的操作）的信息。  
  
 服务授权审核事件包含服务授权管理器所做出的授权决定。 它们提供有关授权是成功还是失败以及客户端标识、消息所发送到的终结点、与消息关联的操作、从传入消息中生成的授权上下文的标识符和做出访问决定的授权管理器类型的信息。  
  
## <a name="example"></a>示例  
  
```xml  
<system.serviceModel>
  <behaviors>
    <serviceBehaviors>
      <behavior name="NewBehavior">
        <serviceSecurityAudit auditLogLocation="Application"
                              suppressAuditFailure="true"
                              serviceAuthorizationAuditLevel="Success"
                              messageAuthenticationAuditLevel="Success" />
      </behavior>
    </serviceBehaviors>
  </behaviors>
</system.serviceModel>
```  
  
## <a name="see-also"></a>请参阅

- <xref:System.ServiceModel.Configuration.ServiceSecurityAuditElement>
- <xref:System.ServiceModel.Description.ServiceSecurityAuditBehavior>
- [安全行为](../../../wcf/feature-details/security-behaviors-in-wcf.md)
- [审核](../../../wcf/feature-details/auditing-security-events.md)
- [如何：审核安全事件](../../../wcf/feature-details/how-to-audit-wcf-security-events.md)
- [服务审核行为](../../../wcf/samples/service-auditing-behavior.md)
