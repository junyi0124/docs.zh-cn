---
title: SQL 工作流实例存储
ms.date: 03/30/2017
ms.assetid: 8cd2f8a5-4bf8-46ea-8909-c7fdb314fabc
ms.openlocfilehash: e0989e4ed5d9e256d3570b0c3ee2bb35a95b410a
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96261743"
---
# <a name="sql-workflow-instance-store"></a>SQL 工作流实例存储

[!INCLUDE[netfx_current_short](../../../includes/netfx-current-short-md.md)] 附带的 SQL 工作流实例存储允许工作流在 SQL Server 2005 或 SQL Server 2008 数据库中持久保存有关工作流实例的状态信息。 此功能主要以 <xref:System.Activities.DurableInstancing.SqlWorkflowInstanceStore> 类的形式实现，该类是从持久性框架的抽象 <xref:System.Runtime.DurableInstancing.InstanceStore> 类派生的。 SQL 工作流实例存储功能包含一个 SQL 持久性提供程序，该提供程序是持久性 API 的具体实现，宿主将使用此持久性 API 向存储发送持久性命令。  
  
 SQL 工作流实例存储支持自承载工作流或使用 <xref:System.Activities.WorkflowApplication> 或 <xref:System.ServiceModel.WorkflowServiceHost> 的工作流服务以及 WAS 中承载的使用 <xref:System.ServiceModel.WorkflowServiceHost> 的服务。 可以使用 SQL 工作流实例存储功能公开的对象模型以编程方式为自承载服务配置该功能。 可以使用对象模型，也可以使用 XML 配置文件以编程方式为由 <xref:System.ServiceModel.WorkflowServiceHost> 承载的服务配置此功能。  
  
 SQL 工作流实例存储区功能 (**SqlWorkflowInstanceStore** 类) 不实现 <xref:System.ServiceModel.Persistence.PersistenceProviderFactory> ，因此不为持久的非工作流 WCF 服务提供持久性支持。 它也不实现 <xref:System.Workflow.Runtime.Hosting.WorkflowPersistenceService>，因此不提供对 3.x 工作流的持久性支持。 该功能仅对 WF 4.0（和更高版本）工作流和工作流服务提供持久性支持。 该功能也不支持除了 SQL Server 2005 和 SQL Server 2008 之外的任何数据库。  
  
 本节中的主题介绍 SQL 工作流实例存储的属性和功能，并且提供有关配置该存储的详细信息。  
  
 Windows Server App Fabric 提供自己的实例存储区和工具以便简化实例存储区的配置和使用。 有关详细信息，请参阅 [Windows Server App Fabric 实例存储](/previous-versions/appfabric/ff383417(v=azure.10))。 有关 App Fabric SQL Server 持久性数据库的详细信息，请参阅 [应用构造 SQL Server 持久性数据库](/previous-versions/appfabric/ee790819(v=azure.10))  
  
## <a name="in-this-section"></a>本节内容  
  
- [SQL 工作流实例存储的属性](properties-of-sql-workflow-instance-store.md)  
  
- [如何：对工作流和工作流服务启用 SQL 持久性](how-to-enable-sql-persistence-for-workflows-and-workflow-services.md)  
  
- [实例激活](instance-activation.md)  
  
- [查询支持](support-for-queries.md)  
  
- [存储扩展性](store-extensibility.md)  
  
- [安全性](security.md)  
  
- [SQL Server 持久性数据库](sql-server-persistence-database.md)  
  
## <a name="see-also"></a>另请参阅

- [Persistence Samples（持久性示例）](/previous-versions/dotnet/netframework-4.0/dd699769(v=vs.100))
