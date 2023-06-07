---
title: 工作流安全性
ms.date: 03/30/2017
helpviewer_keywords:
- programming [WF], workflow security
ms.assetid: d712a566-f435-44c0-b8c0-49298e84b114
ms.openlocfilehash: 6253a0d76d8b1db938e789f19d2cdd5abba9b700
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96273888"
---
# <a name="workflow-security"></a>工作流安全性

Windows Workflow Foundation (WF) 与几种不同的技术（例如 Microsoft SQL Server 和 Windows Communication Foundation (WCF) 集成在一起。 如果操作不当，采用这些技术可能会给工作流带来安全问题。

## <a name="persistence-security-concerns"></a>持久性安全问题

1. 使用 <xref:System.Activities.Statements.Delay> 活动和持久性的工作流需要由服务重新激活。 Windows AppFabric 使用工作流管理服务 (WMS) 重新激活计时器已过期的工作流。 WMS 创建 <xref:System.ServiceModel.WorkflowServiceHost> 来承载重新激活的工作流。 如果 WMS 服务停止，则持久化的工作流在其计时器过期时将不会重新激活。

2. 对持久性实例化的访问应当防止应用程序域外的恶意实体。 此外，开发人员应确保恶意代码不能在与持久性实例化代码相同的应用程序域中执行。

3. 不应使用提升的（管理员）权限来运行持久性实例化。

4. 要在应用程序域的外部处理的数据应受到保护。

5. 需要安全隔离的应用程序不应共享架构抽象的同一实例。 此类应用程序应使用不同的存储提供程序，或者使用配置为使用不同存储实例化的存储提供程序。

## <a name="sql-server-security-concerns"></a>SQL Server 安全问题

- 使用大量子活动、位置、书签、宿主扩展或作用域时，或使用负载很大的书签时，可能会耗尽内存，或在持久化过程中分配过多的数据库空间。 可使用对象级和数据级的安全措施来缓解该问题。

- 使用 <xref:System.Activities.DurableInstancing.SqlWorkflowInstanceStore> 时，必须保护实例存储区的安全。

- 应为实例存储区中的敏感数据加密。 有关详细信息，请参阅 [SQL Server Encryption](/sql/relational-databases/security/encryption/sql-server-encryption)。

- 因为数据库连接字符串通常包含在配置文件中，所以，Windows 级别的安全性 (ACL) 应该用于确保配置文件（通常是 Web.Config）是安全的，并且登录名和密码信息不包含在连接字符串中。 应改为在数据库和 Web 服务器之间使用 Windows 身份验证。

## <a name="considerations-for-workflowservicehost"></a>WorkflowServiceHost 的注意事项

- 工作流中使用 Windows Communication Foundation (WCF) 终结点应受到保护。 有关详细信息，请参阅 [WCF 安全概述](../wcf/feature-details/security-overview.md)。

- 宿主级授权可以使用 <xref:System.ServiceModel.ServiceAuthorizationManager> 来实现。 有关详细信息，请参阅 [如何：为服务创建自定义授权管理器](../wcf/extending/how-to-create-a-custom-authorization-manager-for-a-service.md) 。

- 也可通过访问 OperationContext 从工作流内提供传入消息的 ServiceSecurityContext。

## <a name="wf-security-pack-ctp"></a>WF Security Pack CTP

 Microsoft WF Security Pack 社区技术预览版 (CTP) 1 是一组活动及其实现，它们基于[.NET Framework 4](/previous-versions/dotnet/netframework-4.0/w0x726c2(v=vs.100)) (WF 4) 和[Windows IDENTITY Foundation (WIF) ](/previous-versions/dotnet/framework/security/index)中的[Windows Workflow Foundation](index.md) 。 Microsoft WF Security Pack CTP 1 包含活动及其设计器，阐释如何使用工作流轻松地实现各种与安全相关的方案，包括：

1. 在工作流中模拟客户端标识

2. 工作流中授权，例如 PrincipalPermission 和声明的验证

3. 使用在工作流中指定的 ClientCredentials 的经过身份验证的消息传递，例如从安全令牌服务 (STS) 检索的用户名/密码或令牌。

4. 使用 WS-Trust ActAs 将客户端安全令牌流动到后端服务（基于声明的委托）

有关详细信息以及下载 WF Security Pack CTP，请参阅： [Wf Security PACK ctp](https://archive.codeplex.com/?p=wf)
