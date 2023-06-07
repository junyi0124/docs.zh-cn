---
title: 如何：审核 Windows Communication Foundation 安全事件
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- security [WCF], auditing events
ms.assetid: e71e9587-3336-46a2-9a9e-d72a1743ecec
ms.openlocfilehash: 67ab5d4a4592a8b772cfdd70befe32f339062b8c
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96257556"
---
# <a name="how-to-audit-windows-communication-foundation-security-events"></a>如何：审核 Windows Communication Foundation 安全事件

Windows Communication Foundation (WCF) 允许您将安全事件记录到 Windows 事件日志，可以使用 Windows 事件查看器来查看这些事件日志。 本主题说明如何设置应用程序以使其记录安全事件。 有关 WCF 审核的详细信息，请参阅 [审核](auditing-security-events.md)。  
  
### <a name="to-audit-security-events-in-code"></a>通过代码审核安全事件  
  
1. 指定审核日志位置。 为此，请将 <xref:System.ServiceModel.Description.ServiceSecurityAuditBehavior.AuditLogLocation%2A> 类的 <xref:System.ServiceModel.Description.ServiceSecurityAuditBehavior> 属性设置为 <xref:System.ServiceModel.AuditLogLocation> 枚举值之一，如下面的代码中所示。  
  
     [!code-csharp[AuditingSecurityEvents#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/auditingsecurityevents/cs/auditingsecurityevents.cs#2)]
     [!code-vb[AuditingSecurityEvents#2](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/auditingsecurityevents/vb/auditingsecurityevents.vb#2)]  
  
     <xref:System.ServiceModel.AuditLogLocation>枚举具有三个值： `Application` 、 `Security` 或 `Default` 。 该值指定在事件查看器中可见的日志之一：安全日志或应用程序日志。 如果您使用 `Default` 值，则实际的日志将取决于运行应用程序的操作系统。 如果启用审核，但未指定日志位置，则对于支持写入安全日志的平台，默认值为 `Security` 日志；对于其他平台，则写入 `Application` 日志。 默认情况下，只有 Windows Server 2003 和 Windows Vista 支持写入安全日志。  
  
2. 设置要审核的事件的类型。 您可以同时审核服务级事件或消息级授权事件。 为此，请将 <xref:System.ServiceModel.Description.ServiceSecurityAuditBehavior.ServiceAuthorizationAuditLevel%2A> 属性或 <xref:System.ServiceModel.Description.ServiceSecurityAuditBehavior.MessageAuthenticationAuditLevel%2A> 属性设置为 <xref:System.ServiceModel.AuditLevel> 枚举值之一，如下面的代码所示。  
  
     [!code-csharp[AuditingSecurityEvents#3](../../../../samples/snippets/csharp/VS_Snippets_CFX/auditingsecurityevents/cs/auditingsecurityevents.cs#3)]
     [!code-vb[AuditingSecurityEvents#3](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/auditingsecurityevents/vb/auditingsecurityevents.vb#3)]  
  
3. 指定是向应用程序隐匿还是公开日志审核失败事件。 将 <xref:System.ServiceModel.Description.ServiceSecurityAuditBehavior.SuppressAuditFailure%2A> 属性设置为 `true` 或 `false`，如下面的代码所示。  
  
     [!code-csharp[AuditingSecurityEvents#4](../../../../samples/snippets/csharp/VS_Snippets_CFX/auditingsecurityevents/cs/auditingsecurityevents.cs#4)]
     [!code-vb[AuditingSecurityEvents#4](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/auditingsecurityevents/vb/auditingsecurityevents.vb#4)]  
  
     默认 `SuppressAuditFailure` 属性为 `true`，因此审核失败不会影响应用程序。 否则会引发异常。 对于任何成功的审核，都将写入详细跟踪。 对于任何失败的审核，都将在错误级别写入跟踪。  
  
4. 从在 <xref:System.ServiceModel.Description.ServiceSecurityAuditBehavior> 的说明中找到的行为集合中移除现有 <xref:System.ServiceModel.ServiceHost>。 该行为集合通过 <xref:System.ServiceModel.Description.ServiceDescription.Behaviors%2A> 属性来访问，而该属性又通过 <xref:System.ServiceModel.ServiceHostBase.Description%2A> 属性来访问。 然后，向同一集合中添加新的 <xref:System.ServiceModel.Description.ServiceSecurityAuditBehavior>，如下面的代码所示。  
  
     [!code-csharp[AuditingSecurityEvents#5](../../../../samples/snippets/csharp/VS_Snippets_CFX/auditingsecurityevents/cs/auditingsecurityevents.cs#5)]
     [!code-vb[AuditingSecurityEvents#5](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/auditingsecurityevents/vb/auditingsecurityevents.vb#5)]  
  
### <a name="to-set-up-auditing-in-configuration"></a>通过配置方式设置审核  
  
1. 若要在配置中设置审核，请将 [\<behavior>](../../configure-apps/file-schema/wcf/behavior-of-endpointbehaviors.md) 元素添加到 [\<behaviors>](../../configure-apps/file-schema/wcf/behaviors.md) web.config 文件的部分。 然后添加 [\<serviceSecurityAudit>](../../configure-apps/file-schema/wcf/servicesecurityaudit.md) 元素并设置各种属性，如下面的示例中所示。  
  
    ```xml  
    <behaviors>  
       <behavior name="myAuditBehavior">  
          <serviceSecurityAudit auditLogLocation="Application"  
                suppressAuditFailure="false"
                serviceAuthorizationAuditLevel="None"
                messageAuthenticationAuditLevel="SuccessOrFailure" />  
          </behavior>  
    </behaviors>  
    ```  
  
2. 您必须为服务指定行为，如下面的示例所示。  
  
    ```xml  
    <services>  
        <service type="WCS.Samples.Service.Echo"
        behaviorConfiguration=" myAuditBehavior">  
           <endpoint address=""  
                    binding="wsHttpBinding"  
                    bindingConfiguration="CertificateDefault"
                    contract="WCS.Samples.Service.IEcho" />  
        </service>  
    </services>  
    ```  
  
## <a name="example"></a>示例  

 下面的代码创建 <xref:System.ServiceModel.ServiceHost> 类的一个实例，然后向其行为集合添加一个新的 <xref:System.ServiceModel.Description.ServiceSecurityAuditBehavior>。  
  
 [!code-csharp[AuditingSecurityEvents#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/auditingsecurityevents/cs/auditingsecurityevents.cs#1)]
 [!code-vb[AuditingSecurityEvents#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/auditingsecurityevents/vb/auditingsecurityevents.vb#1)]  
  
## <a name="net-framework-security"></a>.NET Framework 安全性  

 将 <xref:System.ServiceModel.Description.ServiceSecurityAuditBehavior.SuppressAuditFailure%2A> 属性设置为 `true`，就会隐匿任何生成安全审核失败（如果设置为 `false`，则会引发异常）。 但是，如果启用以下 "Windows **本地安全设置** " 属性，则生成审核事件失败将导致 Windows 立即关闭：  
  
 **审核：如果无法记录安全审核则立即关闭系统**  
  
 若要设置该属性，请打开 " **本地安全设置** " 对话框。 在 " **安全设置**" 下，单击 " **本地策略**"。 然后单击 " **安全选项**"。  
  
 如果将 <xref:System.ServiceModel.AuditLogLocation> 属性设置为 <xref:System.ServiceModel.AuditLogLocation.Security> ，并且未在 **本地安全策略** 中设置 **审核对象访问**，则不会将审核事件写入安全日志。 请注意，虽然不返回任何失败记录，但审核项不会写入安全日志。  
  
## <a name="see-also"></a>另请参阅

- <xref:System.ServiceModel.Description.ServiceSecurityAuditBehavior.AuditLogLocation%2A>
- <xref:System.ServiceModel.Description.ServiceSecurityAuditBehavior>
- <xref:System.ServiceModel.AuditLogLocation>
- [审核](auditing-security-events.md)
