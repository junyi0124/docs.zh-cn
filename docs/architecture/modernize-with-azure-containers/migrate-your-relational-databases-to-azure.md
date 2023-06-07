---
title: 将关系数据库迁移到 Azure
description: 通过 Azure 云和 Windows 容器现代化现有 .NET 应用程序 | 将关系数据库迁移到 Azure
ms.date: 04/28/2018
ms.openlocfilehash: efd1548c3f74fc27450f4949d71a1c4d61907ba5
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/14/2020
ms.locfileid: "73093617"
---
# <a name="migrate-your-relational-databases-to-azure"></a>将关系数据库迁移到 Azure

愿景：Azure 提供最全面的数据库迁移。

在 Azure 中，可以直接将数据库服务器迁移到 IaaS VM（纯粹的直接迁移），也可以迁移到 Azure SQL 数据库，以获得更多益处。 Azure SQL 数据库提供托管实例和完整的数据库即服务 (DBaaS) 选项。 图 3-1 显示了 Azure 中提供的多个关系数据库迁移路径。

![Azure 中的数据库迁移路径](./media/image3-1.png)

**图 3-1。** Azure 中的数据库迁移路径

## <a name="when-to-migrate-to-azure-sql-database-managed-instance"></a>何时将 Azure SQL 数据库迁移到托管实例

在大多数情况下，在将数据迁移到 Azure 时，Azure SQL 数据库托管实例是要考虑的最佳选择。 如果你正在迁移 SQL Server 数据库，并且需要几乎 100% 的保证无需重塑应用程序或更改数据或数据访问代码，请选择 Azure SQL 数据库的托管实例功能。

如果对 SQL Server 实例级别的功能有额外要求或有超出标准 Azure SQL 数据库（单数据库模型）中提供的功能之外的隔离要求，Azure SQL 数据库托管实例是最佳选项。 这最后一种方法是最适合 PaaS 的选择，但它并不提供与传统 SQL Server 相同的功能。 迁移可能引发摩擦。

例如，在实例级 SQL Server 功能方面进行深层投资的组织将从迁移到 SQL 托管实例中受益。 实例级 SQL Server 功能的示例包括 SQL 公共语言运行时 (CLR) 集成、SQL Server 代理和跨数据库查询。 对这些功能的支持在标准 Azure SQL 数据库（单数据库模型）中不可用。

在高度管控行业中运营并且出于安全目的而需要维护隔离的组织，还可以从选择 SQL 托管实例模型中获益。

Azure SQL 数据库中的托管实例具有以下特征：

- 通过 Azure 虚拟网络进行安全隔离

- 应用程序表面兼容性，其中包含以下功能：

  - SQL Server 代理和 SQL Server Profiler

  - 跨数据库引用和查询、SQL CLR、复制、变更数据捕获 (CDC) 和 Service Broker

- 数据库大小最高为 35 TB

- 最少停机时间迁移，其中包含以下功能：

  - Azure 数据库迁移服务

  - 本机备份和还原以及日志传送

借助这些功能，在将现有应用程序数据库迁移到 Azure SQL 数据库时，托管实例模型为 SQL Server 提供 PaaS 的将近 100% 的益处。 托管实例是一种 SQL Server 环境，在此环境中，你可以继续使用实例级功能，而无需更改应用程序设计。

托管实例可能最适合当前正在使用 SQL Server 并且需要在云中的网络安全灵活性的企业。 这就像是为 SQL 数据库提供专用虚拟网络。

## <a name="when-to-migrate-to-azure-sql-database"></a>何时迁移到 Azure SQL 数据库

如前所述，标准 Azure SQL 数据库是完全托管的关系 DBaaS。 SQL 数据库当前在世界各地跨 38 个数据中心管理数百万个生产数据库。 它支持广泛的应用程序和工作负荷，从管理简单的事务数据，到驱动需要在全球范围内进行高级数据处理的数据密集型、任务关键型应用程序。

由于它具有完整 PaaS 功能、更好的定价和最终更低的成本，因此如果应用程序使用基本的标准 SQL 数据库且不使用其他实例功能，你应当将迁移到标准 Azure SQL 数据库作为“默认选择”。 标准 Azure SQL 数据库中不支持 SQL CLR 集成、SQL Server 代理和跨数据库查询等 SQL Server 功能。 这些功能仅在 Azure SQL 数据库托管实例模型中提供。

Azure SQL 数据库是为应用程序开发人员构建的唯一智能云数据库服务。 它也是唯一一种无需停机即可实时缩放的云数据库服务，可帮助你高效地交付多租户应用。 最后，Azure SQL 数据库可让你有更多的时间来创新，并加速上市时间。 可以使用你喜欢的语言和平台来构建安全的应用并连接到你的 SQL 数据库。

Azure SQL 数据库提供以下益处：

- 可学习和适应应用的内置智能（机器学习）

- 按需数据库预配

- 针对所有工作负载的一系列产品/服务

- 99.99% 的可用性 SLA，零维护

- 用于数据保护的异地复制和还原服务

- Azure SQL 数据库时间点还原功能

- 与 SQL Server 2016 兼容，包括混合和迁移

标准 Azure SQL 数据库比 Azure SQL 数据库托管实例更接近于 PaaS。 首选标准 Azure SQL 数据库，因为你将从托管云获得更多益处。 但是，Azure SQL 数据库与常规和本地 SQL Server 实例有一些重要差异。 根据你的现有应用程序的数据库要求以及企业需求和策略，在计划迁移到云时，它可能不是最佳选择。

## <a name="when-to-move-your-original-rdbms-to-a-vm-iaas"></a>何时将原始 RDBMS 移到 VM (IaaS)

迁移选项之一是将原始关系数据库管理系统 (RDBMS)（包括 Oracle、IBM DB2、MySQL、PostgreSQL 或 SQL Server）移到在 Azure VM 上运行的类似服务器。 如果现有应用程序需要在更改最少或根本没有任何更改的情况下以最快速度迁移到云，则直接迁移到云中的 IaaS 可能是一个合理的选择。 它可能不是充分利用云的所有益处的最佳方法，但它可能是最快的初始路径。

目前，Microsoft Azure 最多可支持将 [331 个不同数据库服务器](https://azuremarketplace.microsoft.com/marketplace/apps/category/databases?page=1&subcategories=databases-all)部署为 IaaS VM。 其中包括 SQL Server、Oracle、MySQL、PostgreSQL 和 IBM DB2 等常见 RDBMS，以及 MongoDB、Cassandra、DataStax、MariaDB 和 Cloudera 等许多其他 NoSQL 数据库。

> [!NOTE]
> 尽管将 RDBMS 移到 Azure VM 可能是将数据迁移到云的最快方法（因为它是 IaaS），但这种方法需要在 IT 团队（数据库管理员和 IT 专业人员）中投入大量投资。 企业团队需要能够为 SQL Server 设置和管理高可用性、灾难恢复和修补。 此上下文需要一个具有完全管理权限的自定义环境。

## <a name="when-to-migrate-to-sql-server-as-a-vm-iaas"></a>何时迁移到作为 VM (IaaS) 的 SQL Server

在某些情况下，你可能仍需要迁移到作为常规 VM 的 SQL Server。 例如，如果需要使用 SQL Server Reporting Services，则会出现这种情况。 但在大多数情况下，Azure SQL 数据库托管实例可以提供从本地 SQL server 迁移所需的所有内容，因此，迁移到 SQL Server VM 应当是你最终的尝试方法。

## <a name="use-azure-database-migration-service-to-migrate-your-relational-databases-to-azure"></a>使用 Azure 数据库迁移服务将关系数据库迁移到 Azure

你可以使用 Azure 数据库迁移服务将 SQL Server、Oracle 和 MySQL 等关系数据库迁移到 Azure，无论目标数据库是 Azure SQL 数据库、Azure SQL 数据库托管实例还是 Azure VM 上的 SQL Server。

带有评估报表功能的自动工作流将指导你完成在迁移数据库之前需要进行的更改。 准备就绪后，服务会将源数据库迁移到 Azure。

只要更改原始 RDBMS，就可能需要重新测试。 你还可能需要更改应用程序中的 SQL 语句或对象关系映射 (ORM) 代码，具体取决于测试结果。

如果你有任何其他数据库（例如，IBM DB2），并且选择了直接迁移方法，则可能需要继续将这些数据库用作 Azure 中的 IaaS VM，除非你愿意执行更复杂的数据迁移。 更复杂的数据迁移需要额外的精力，因为你将迁移到具有新架构和不同编程库的不同数据库类型。

若要了解如何使用 Azure 数据库迁移服务来迁移数据库，请参阅[通过 Azure SQL 数据库托管实例和 Azure 数据库迁移服务更快地访问云](https://channel9.msdn.com/Events/Build/2017/P4008)。

## <a name="additional-resources"></a>其他资源

- **选择一个云 SQL Server 选项：Azure SQL 数据库 (PaaS) 或 Azure VM 上的 SQL Server (IaaS)**

    <https://docs.microsoft.com/azure/sql-database/sql-database-paas-vs-sql-server-iaas>

- **通过 Azure SQL DB 托管实例和数据库迁移服务更快地访问云**

    <https://channel9.msdn.com/Events/Build/2017/P4008>

- **将 SQL Server 数据库迁移到云中的 SQL 数据库**

    <https://docs.microsoft.com/azure/sql-database/sql-database-cloud-migrate>

- **Azure SQL 数据库**

    <https://azure.microsoft.com/services/sql-database/?v=16.50>

- **虚拟机上的 SQL Server**

    <https://azure.microsoft.com/services/virtual-machines/sql-server/>

> [!div class="step-by-step"]
> [上一页](lift-and-shift-existing-apps-azure-iaas.md)
> [下一页](modernize-existing-apps-to-cloud-optimized/index.md) <!-- Next Chapter -->
