---
title: 关系与NoSQL 数据
description: 了解云本机应用程序中的关系数据和 NoSQL 数据
author: robvet
ms.date: 05/17/2020
ms.openlocfilehash: 11db5cdca06b9c2c8ce12598456c4b147ac379ba
ms.sourcegitcommit: 98d20cb038669dca4a195eb39af37d22ea9d008e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/22/2020
ms.locfileid: "92434869"
---
# <a name="relational-vs-nosql-data"></a>关系与NoSQL 数据

关系型和 NoSQL 是通常在云本机应用程序中实现的两种类型的数据库系统。 它们的构建方式不同，以不同的方式存储数据，并以不同的方式进行访问。 在本部分中，我们将介绍这两种情况。 本章稍后将介绍一种名为 *NewSQL*的新兴数据库技术。

*关系数据库* 是数十年的普遍技术。 它们是成熟、经过验证且广泛实现的。 竞争的数据库产品、工具和专业知识 abound。 关系数据库提供相关数据表的存储区。 这些表具有固定架构，使用 SQL (结构化查询语言) 管理数据，并支持 ACID 保证。

非*SQL 数据库指的*是高性能、非关系型数据存储。 它们采用易用性、可伸缩性、复原能力和可用性特征提供了方便。 NoSQL 存储非结构化或半结构化数据，而不是联接规范化数据的表，通常在键值对或 JSON 文档中。 非 SQL 数据库通常不会提供超出单一数据库分区范围的 ACID 保证。 需要第二次响应时间的高容量服务倾向于 NoSQL 数据存储。

[NoSQL](https://www.geeksforgeeks.org/introduction-to-nosql/)技术对于分布式云本机系统的影响不能言过其实。 这一领域新数据技术的激增导致了只依赖于关系数据库的解决方案。

NoSQL 数据库包含用于访问和管理数据的多个不同的模型，每个模型都适用于特定用例。 图5-9 显示了四个通用模型。

![NoSQL 数据模型](./media/types-of-nosql-datastores.png)

**图 5-9**： NoSQL 数据库的数据模型

| 型号 | 特征 |
| :-------- | :-------- |
| 文档存储 | 数据和元数据以分层形式存储在数据库中基于 JSON 的文档中。 |
| 键值存储 | 最简单的 NoSQL 数据库数据以键值对的集合表示。 |
| Wide-Column 存储 | 相关数据以一组嵌套键/值对的形式存储在单个列中。 |
| 图形存储 | 数据作为节点、边缘和数据属性存储在图形结构中。 |

## <a name="the-cap-theorem"></a>CAP 定理

要了解这两种类型的数据库之间的差异，请考虑使用 CAP 定理，这组原则适用于存储状态的分布式系统。 图5-10 显示了 CAP 定理的三个属性。

![The CAP Theorem（CAP 定理）](./media/cap-theorem.png)

**图 5-10**。 CAP 定理

定理指出，分布式数据系统将在一致性、可用性和分区容差之间提供权衡。 而且，任何数据库只能保证三个属性中的 *两个* ：

- *起见.* 群集中的每个节点都将使用最新的数据做出响应，即使在所有副本更新之前，系统都必须阻止请求。 如果查询当前正在更新的项的 "一致性系统"，则将等待该响应，直到所有副本都成功更新。 不过，您将收到最新的数据。

- *有.* 即使该响应不是最新的数据，每个节点都将返回立即响应。 如果您在 "可用系统" 中查询正在更新的项，则您将获得该服务在此时可以提供的最佳可能的答案。

- *分区容差。* 即使复制的数据节点发生故障或失去与其他复制的数据节点的连接，保证系统仍可继续运行。

关系数据库通常提供一致性和可用性，但不提供分区容差。 通常会将它们预配到单台服务器，并通过将更多资源添加到计算机来进行垂直缩放。

许多关系数据库系统都支持内置的复制功能，在这种情况下，可以将主数据库的副本创建到其他辅助服务器实例。 对主实例进行写入操作，并将其复制到每个辅助数据库。 发生故障时，主实例可以故障转移到辅助数据库，以提供高可用性。 辅助副本也可以用于分发读取操作。 尽管写入操作始终针对主副本进行，但读取操作可路由到任何辅助副本以减少系统负载。

数据还可以在多个节点之间水平分区，例如 with [分片](/azure/sql-database/sql-database-elastic-scale-introduction)。 但是，分片通过 spitting 不能轻松通信的多个部分中的数据来显著提高运营开销。 管理成本高昂且耗时很高。 最终可能会影响性能、表联接和引用完整性。

如果数据副本在 "高度一致" 关系数据库群集中丢失网络连接，则无法写入数据库。 系统将拒绝写入操作，因为它不能将更改复制到其他数据副本。 必须先更新每个数据副本，然后才能完成该事务。

NoSQL 数据库通常支持高可用性和分区容差。 它们通常跨商用服务器横向扩展。 此方法在地理区域内和跨地理区域提供巨大的可用性，同时降低成本。 在这些计算机或节点上对数据进行分区和复制，提供冗余和容错能力。 缺点是一致性。 对一个 NoSQL 节点上的数据所做的更改可能需要一段时间才能传播到其他节点。 通常，NoSQL 数据库节点会立即提供对查询的响应，即使显示的数据已过时并且尚未更新。

如果数据副本在 "高度可用" NoSQL 数据库群集中丢失连接，你仍可以完成对数据库的写操作。 数据库群集将允许写入操作，并在数据副本可用时更新每个数据副本。

这种结果称为最终一致性，即不支持 ACID 事务的分布式数据系统的特征。 在更新数据项目和将更新传播到每个副本节点所用的时间之间，会有短暂的延迟。 正常情况下，滞后时间通常较短，但当出现问题时可能会增加。 例如，如果要更新美国中的 NoSQL 数据库中的产品项，并从欧洲的副本节点中查询同一数据项，会发生什么情况呢？ 在群集用产品更改更新欧盟节点之前，您会收到以前的产品信息。 通过立即返回查询结果而不是等待所有副本节点进行更新，可以获得巨大的规模和量，但可能会显示较旧的数据。

高可用性和巨大的可伸缩性通常比强一致性更为重要。 开发人员可以实现多种方法和模式，如 Sagas、CQRS 和异步消息传递，以实现最终一致性。

> 如今，在考虑 CAP 定理约束时必须小心谨慎。 新类型的数据库（称为 NewSQL）已发生，它扩展了关系数据库引擎，以支持 NoSQL 系统的水平可扩展性和可缩放的性能。

## <a name="considerations-for-relational-vs-nosql-systems"></a>关系与 NoSQL 系统的注意事项

基于特定的数据要求，基于云的本机微服务可以实现关系、NoSQL 数据存储或两者。

|  在以下情况时，请考虑 NoSQL 数据存储： | 在以下情况时，请考虑关系数据库： |
| :-------- | :-------- |
| 具有大量需要大规模的工作负荷 | 工作负荷量一致，需要大中型 |
| 工作负载不需要 ACID 保证 |  需要 ACID 保障 |
| 你的数据是动态的，并且经常更改 | 你的数据是可预测的且具有高结构 |
| 无需关系即可表达数据 | 最 relationally 的数据 |  
| 你需要快速写入并且写入安全并不重要 | 需要编写安全 |  
| 数据检索很简单，往往是平面的 | 使用复杂的查询和报表|
| 你的数据需要广泛的地理分布 | 用户是更集中的 |
| 你的应用程序将部署到商用硬件上，如公有云 | 你的应用程序将部署到大型高端硬件 |

在接下来的部分中，我们将探讨 Azure 云中提供的用于存储和管理云本机数据的选项。

## <a name="database-as-a-service"></a>数据库即服务

若要开始，可以预配 Azure 虚拟机，并为每个服务安装所选数据库。 尽管对环境具有完全控制，但放弃云平台的许多内置功能。 你还负责管理每个服务的虚拟机和数据库。 这种方法很快就会变得非常耗时和昂贵。

取而代之的是，云本机应用程序倾向于以数据库即服务形式公开的数据服务 [ (DBaaS) ](https://www.stratoscale.com/blog/dbaas/what-is-database-as-a-service/)。 这些服务由云供应商完全托管，提供内置的安全性、可伸缩性和监视。 只需将其用作 [后备服务](./definition.md#backing-services)，而不是拥有该服务。 提供程序以大规模方式操作资源，并承担性能和维护责任。

可以跨云可用性区域和区域对其进行配置，以实现高可用性。 它们都支持实时容量和即用即付模型。 Azure 具有不同类型的托管数据服务选项，每个选项都有特定的优点。

首先，我们将了解 Azure 中提供的关系 DBaaS 服务。 你将看到，Microsoft 的旗舰 SQL Server 数据库与多个开源选项一起提供。 接下来，我们将讨论 Azure 中的 NoSQL 数据服务。

## <a name="azure-relational-databases"></a>Azure 关系数据库

对于需要关系数据的云本地微服务，Azure 提供了四个托管的关系数据库即服务 (DBaaS) 产品/服务，如图5-11 所示。

![Azure 中的托管关系数据库](./media/azure-managed-databases.png)

**图 5-11**。 Azure 中提供的托管关系数据库

在上图中，请注意每个 DBaaS 基础结构的功能，该基础结构具有关键功能，无需额外付费。

这些功能对于预配大量数据库的组织尤其重要，但对于管理这些数据库的资源有限。
通过选择处理核心、内存和基础存储量，你可以在数分钟内预配 Azure 数据库。 你可以动态缩放数据库，并动态调整资源，只需很少的停机时间。

## <a name="azure-sql-database"></a>Azure SQL 数据库

Microsoft SQL Server 的专业技能的开发团队应考虑 [AZURE SQL 数据库](/azure/sql-database/)。 这是一种完全托管的关系数据库即服务 (DBaaS) 基于 Microsoft SQL Server 的数据库引擎。 该服务共享 SQL Server 的本地版本中发现的许多功能，并运行 SQL Server 数据库引擎的最新稳定版本。

若要与云本机微服务一起使用，可通过三种部署选项使用 Azure SQL 数据库：

- 单一数据库表示在 Azure 云中的 [AZURE SQL 数据库服务器](/azure/sql-database/sql-database-servers) 上运行的完全托管的 sql 数据库。 该数据库被视为 [*包含*](/sql/relational-databases/databases/contained-databases) ，因为它在基础数据库服务器上没有配置依赖关系。
  
- [托管实例](/azure/sql-database/sql-database-managed-instance)是 Microsoft SQL Server 数据库引擎的完全托管实例，提供与本地 SQL Server 近乎100% 的兼容性。 此选项支持较大的数据库，最大可达 35 TB，并将其放在 [Azure 虚拟网络](/azure/virtual-network/virtual-networks-overview) 中，以便更好地进行隔离。

- [AZURE SQL 数据库无服务器](/azure/sql-database/sql-database-serverless) 是单个数据库的计算层，可根据工作负荷需求自动缩放。 它仅计费每秒使用的计算量。 该服务非常适用于具有间歇、不可预测的使用模式的工作负荷，并与不活动状态发生了一段时间。 无服务器计算层还会在非活动期间自动暂停数据库，以便只对存储费用收费。 当活动返回时，它将自动恢复。

除了传统的 Microsoft SQL Server 堆栈以外，Azure 还提供三个常用开源数据库的托管版本。

## <a name="open-source-databases-in-azure"></a>Azure 中的开源数据库

开源关系数据库已成为云本机应用程序的常用选择。 许多企业都倾向于通过商业数据库产品，尤其是为了节省成本。 许多开发团队喜欢其灵活性、支持社区的开发以及工具和扩展的生态系统。 开源数据库可以跨多个云提供商进行部署，有助于最大程度地减少 "供应商锁定" 这一问题。

开发人员可以轻松地在 Azure VM 上自行托管任何开放源代码数据库。 尽管提供完全控制，但这种方法可让你了解如何管理、监视和维护数据库和 VM。

但是，Microsoft 通过提供几个常用的开源数据库作为 *完全托管* 的 DBaaS 服务，继续致力于使 Azure 成为 "开放平台"。

### <a name="azure-database-for-mysql"></a>Azure Database for MySQL

[MySQL](https://en.wikipedia.org/wiki/MySQL) 是一种开源关系数据库，是针对 [灯泡软件堆栈](https://en.wikipedia.org/wiki/LAMP_(software_bundle))构建的应用程序的支柱。 广泛地选择用于 *读取繁重* 的工作负荷，许多大型组织（包括 Facebook、Twitter 和 YouTube）都将使用它。 社区版免费提供，而 enterprise edition 需要购买许可证。 最初在1995中创建，该产品由的 Sun 2008 Microsystems 购买。 Oracle 在2010中获取 Sun 和 MySQL。

[Azure Database for MySQL](https://azure.microsoft.com/services/mysql/) 是基于开源 MySQL Server 引擎的托管关系数据库服务。 它使用 MySQL 社区版。 Azure MySQL 服务器是服务的管理点。 它是用于本地部署的相同 MySQL 服务器引擎。 引擎可以为每个服务器创建一个数据库，也可以为每个共享资源的服务器创建多个数据库。 您可以使用相同的开源工具继续管理数据，而不必学习新技能或管理虚拟机。

### <a name="azure-database-for-mariadb"></a>Azure Database for MariaDB

[MariaDB](https://mariadb.com/) 服务器是另一个受欢迎的开源数据库服务器。 当 Oracle 购买 Sun Microsystems （拥有 MySQL）时，它是 MySQL 的 *分叉* 。 目的是确保 MariaDB 保持开源。 由于 MariaDB 是 MySQL 的分叉，数据和表定义是兼容的，客户端协议、结构和 Api 是编织的。

MariaDB 具有强大的社区，由许多大型企业使用。 尽管 Oracle 继续维护、增强和支持 MySQL，但 MariaDB foundation 管理 MariaDB，允许对产品和文档进行公开贡献。

[Azure Database for MariaDB](https://azure.microsoft.com/services/mariadb/) 是 Azure 云中完全托管的关系数据库即服务。 此服务基于 MariaDB 社区版服务器引擎。 它可以处理具有可预测性能和动态可伸缩性的任务关键型工作负荷。

### <a name="azure-database-for-postgresql"></a>Azure Database for PostgreSQL

[PostgreSQL](https://www.postgresql.org/) 是一种开源关系数据库，其活动开发超过30年。 PostgresSQL 具有可靠性和数据完整性的强大信誉。 它的功能丰富、兼容 SQL 并且被视为比 MySQL 更高的性能，尤其适用于具有复杂查询和繁重写入的工作负荷。 许多大型企业（包括 Apple、Red Hat 和 Fujitsu）都使用 PostgreSQL 构建了产品。

[Azure Database for PostgreSQL](https://azure.microsoft.com/services/postgresql/) 是一项完全托管的关系数据库服务，它基于开源 Postgres 数据库引擎。 该服务支持许多开发平台，包括 c + +、Java、Python、Node、C \# 和 PHP。 你可以使用 [命令行接口](https://datamigration.microsoft.com/scenario/postgresql-to-azurepostgresql?step=1) 工具或 Azure 数据迁移服务将 PostgreSQL 数据库迁移到它。

Azure Database for PostgreSQL 提供了两个部署选项：

- [单服务器](/azure/postgresql/concepts-servers)部署选项是多个数据库的中心管理点，你可以将多个数据库部署到这些数据库中。 根据内核和存储，定价按服务器进行组织。

- [超大规模 (Citus) 选项](https://azure.microsoft.com/blog/get-high-performance-scaling-for-your-azure-database-workloads-with-hyperscale/)由 Citus 数据技术提供支持。 它通过 *水平缩放* 数百个节点上的单个数据库来实现高性能，从而提供更快的性能和规模。 此选项允许引擎在内存中容纳更多数据、跨数百个节点进行并行化查询，并更快地为数据编制索引。

## <a name="nosql-data-in-azure"></a>Azure 中的 NoSQL 数据

Cosmos DB 是 Azure 云中完全托管的全球分布式 NoSQL 数据库服务。 它已由世界各地的许多大型公司采用，包括可口可乐-可乐配方、Skype、ExxonMobil 和自由双向企业。

如果你的服务需要从世界上的任何地方快速响应，则 Cosmos DB 是一个不错的选择。 图5-12 显示 Cosmos DB。

![Cosmos DB 概述](./media/cosmos-db-overview.png)

**图 5-12**： Cosmos DB 概述

上图显示了 Cosmos DB 中提供的许多内置云本机功能。 在本部分中，我们将详细介绍它们。

### <a name="global-support"></a>全球支持

云本机应用程序通常具有全球性受众，并需要全局规模。

您可以在区域之间或世界各地分布 Cosmos 数据库，将数据放置到用户附近，缩短响应时间并减少延迟。 无需暂停或重新部署服务，即可在区域中添加或删除数据库。 在后台，Cosmos DB 以透明方式将数据复制到每个已配置的区域。

Cosmos DB 支持全局级别的 [主动/主动](https://kemptechnologies.com/white-papers/unfog-confusion-active-passive-activeactive-load-balancing/) 群集，使你能够配置任何数据库区域以支持 *写入和读取*。

[多主机](/azure/cosmos-db/multi-master-benefits)协议是 Cosmos DB 中的一项重要功能，可实现以下功能：

- 无限弹性写入和读取可伸缩性。

- 在全球 99.999% 的读写可用性。

- 在 99% 的时间内，在 10 毫秒内为读写提供服务。

利用 Cosmos DB [多宿主 api](/azure/cosmos-db/distribute-data-globally)，微服务会自动识别最近的 Azure 区域并向其发送请求。 最近的区域由 Cosmos DB 标识，无需进行任何配置更改。 如果某个区域变得不可用，则多宿主功能会自动将请求路由到下一个最近的可用区域。

### <a name="multi-model-support"></a>多模型支持

将单一应用程序 replatforming 到云本机体系结构时，开发团队有时必须迁移开源的 NoSQL 数据存储。 Cosmos DB 可以帮助你在这些 NoSQL 数据存储中保留投资，并将其用于 *多模型* 数据平台。 下表显示了支持的 NoSQL [兼容性 api](https://www.wikiwand.com/en/Cosmos_DB)。

| 提供程序 | 描述  |
| :-------- | :-------- |
| SQL API | 支持 JSON 文档和基于 SQL 的查询的专有 API |
| Mongo DB API | 支持 Mongo DB Api 和 JSON 文档|
| Gremlin API | 支持具有基于图形的节点和边缘数据表示形式的 Gremlin API |
| Cassandra API | 支持用于宽列数据表示形式的 Casandra API |  
| 表 API  | 支持具有高级增强功能的 Azure 表存储 |  
| etcd API | 启用 Cosmos DB 作为 Azure Kubernetes 服务群集的后备存储 |

开发团队可以将现有的 Mongo、Gremlin 或 Cassandra 数据库迁移到 Cosmos DB 对数据或代码进行少量更改。 对于新应用程序，开发团队可以在开源选项或内置 SQL API 模型中进行选择。

> 在内部，Cosmos 将数据存储为由基元数据类型组成的简单结构格式。 对于每个请求，数据库引擎将基元数据转换为您选择的模型表示形式。

在上表中，请记下 [表 API](/azure/cosmos-db/table-introduction) 选项。 此 API 是 Azure 表存储的演变。 两者共享同一个基础表模型，但 Cosmos DB 表 API 添加 Azure 存储 API 中未提供的高级增强功能。 下表对这些功能进行了比较。

|  | Azure 表存储  | Azure Cosmos DB  |
| :-------- | :-------- |:-------- |
| 延迟 | Fast | 在世界各地的任何位置进行读取和写入的一位数毫秒延迟 |
| 吞吐量 | 每个表的20000操作数限制 | 10000000每个表的操作 |
| 全局分发 | 具有可选的单个辅助读取区域的单个区域 | 具有自动故障转移功能的所有区域的全包式分布 |
| 索引 | 仅适用于分区和行键属性 | 所有属性的自动索引 |
| 定价 | 基于存储 | 基于吞吐量 |

使用 Azure 表存储的微服务可以轻松地迁移到 Cosmos DB 表 API。 不需更改代码。

### <a name="tunable-consistency"></a>可调整的一致性

在 *关系与 NoSQL* 部分的前面部分中，我们讨论了 *数据一致性*。 数据一致性是指数据的 *完整性* 。 使用分布式数据的云本机服务依赖于复制，并且必须在读取一致性、可用性和延迟之间做出根本性的权衡。

大多数分布式数据库允许开发人员在两种一致性模型之间进行选择：强一致性和最终一致性。 *强一致性* 是数据可编程性的黄金标准。 它可以保证查询始终返回最新的数据，即使系统必须在等待更新复制到所有数据库副本时产生延迟。 虽然配置为 *最终一致性* 的数据库将立即返回数据，即使该数据不是最新的副本。 后一种方法可实现更高的可用性、更高的规模和更高的性能。

Azure Cosmos DB 提供了五个明确定义的 [一致性模型](/azure/cosmos-db/consistency-levels) ，如图5-13 所示。

![Cosmos DB 一致性图形](./media/cosmos-consistency-level-graph.png)

**图 5-13**： Cosmos DB 一致性级别

 利用这些选项，您可以针对数据的一致性、可用性和性能做出精确的选择和细化的权衡。 下表中显示了这些级别。

| 一致性级别 | 说明  |
| :-------- | :-------- |
| 最终 | 不保证读取的顺序。 副本最终将聚合。 |
| 常量前缀 | 读取仍是最终的，但会按写入数据的顺序返回数据。 |
| 会话 | 保证您可以读取在当前会话期间写入的任何数据。 它是默认的一致性级别。 |
| 有限过期 | 按指定的间隔读取跟踪写入。 |  
| 强  | 读取可保证返回项的最新提交版本。 客户端不会看到未提交或部分读取。 |  

在下面 [的文章9球： Cosmos DB 一致性级别介绍](https://blog.jeremylikness.com/blog/2018-03-23_getting-behind-the-9ball-cosmosdb-consistency-levels/)的文章中，Microsoft Program Manager Jeremy Likness 提供了五种模型的出色说明。

### <a name="partitioning"></a>分区

Azure Cosmos DB 采用自动 [分区](/azure/cosmos-db/partitioning-overview) 来缩放数据库，以满足云本地服务的性能需求。

通过创建数据库、容器和项来管理 Cosmos DB 数据中的数据。

容器位于 Cosmos DB 数据库中，表示项的架构无关分组。 项是添加到容器中的数据。 它们表示为文档、行、节点或边缘。 添加到容器中的所有项都自动编制索引。

若要将容器分区，项被分为称为逻辑分区的不同子集。 逻辑分区基于与容器中每一项关联的分区键的值进行填充。 图5-14 显示了两个容器，每个容器都有一个基于分区键值的逻辑分区。

![Cosmos DB 分区机制](./media/cosmos-db-partitioning.png)

**图 5-14**： Cosmos DB 分区机制

请注意，上图中的每一项都包含 "city" 或 "机场" 的分区键。 键确定项的逻辑分区。 带有城市代码的项将分配给左侧的容器，将带机场代码的项分配给右侧的容器。 将分区键值与 ID 值组合会创建一个项的索引，该索引将唯一标识该项。

在内部，Cosmos DB 自动管理物理分区上的 [逻辑分区](/azure/cosmos-db/partition-data) 位置，以满足容器的可伸缩性和性能需求。 随着应用程序吞吐量和存储要求的增加，Azure Cosmos DB 将逻辑分区重新分配到更多的服务器。 再分发操作由 Cosmos DB 管理，并在不中断或停机的情况下调用。

## <a name="newsql-databases"></a>NewSQL 数据库

*NewSQL* 是一种新兴的数据库技术，它结合了 NoSQL 的分布式可扩展性与关系型数据库的 ACID 保证。 NewSQL 数据库对于必须在跨分布式环境中处理大量数据的业务系统非常重要，具有完整的事务性支持和 ACID 遵从性。 虽然 NoSQL 数据库可以提供巨大的可伸缩性，但它不保证数据一致性。 不一致数据的间歇问题会给开发团队带来负担。 开发人员必须在其微服务代码中构建安全措施，以管理由不一致数据导致的问题。

云本机计算基础 (CNCF) 功能 NewSQL 多个数据库项目。

| Project | 特征 |
| :-------- | :-------- |
| 蟑螂 DB |全局缩放的 ACID 兼容的关系数据库。 将新节点添加到群集，CockroachDB 负责跨实例和地理区域平衡数据。 它创建、管理和分发副本以确保可靠性。 它是开源的，免费提供。  |
| TiDB | 支持混合事务和分析处理的开源数据库 (HTAP) 工作负荷。 它是与 MySQL 兼容的功能，可提供水平伸缩性、强一致性和高可用性。  TiDB 的作用类似于 MySQL 服务器。 你可以继续使用现有 MySQL 客户端库，而无需对应用程序进行大量代码更改。 |
| YugabyteDB | 开源、高性能分布式 SQL 数据库。 它支持较低的查询延迟、针对故障的恢复能力和全局数据分布。 YugabyteDB 是 PostgressSQL 兼容的，可处理横向扩展 RDBMS 和 internet 缩放 OLTP 工作负载。 该产品还支持 NoSQL，并与 Cassandra 兼容。 |
|Vitess | Vitess 是用于部署、缩放和管理 MySQL 实例的大型群集的数据库解决方案。 它可以在公共或私有云体系结构中运行。 Vitess 结合了许多重要的 MySQL 功能和功能，同时提供了垂直和水平分片支持。 源自 YouTube，Vitess 一直在为所有 YouTube 数据库流量提供服务，2011。 |

可从云本机计算基础获取上图中的开源项目。 其中三个产品是完整的数据库产品，其中包括 .NET Core 支持。 另一种是 Vitess，它是一种数据库群集系统，可用于水平缩放 MySQL 实例的大型群集。

NewSQL 数据库的关键设计目标是在 Kubernetes 中以本机方式工作，利用平台的复原能力和可伸缩性。

NewSQL 数据库旨在应对暂时的云环境，在该环境中，基础虚拟机随时可以重新启动或重新计划。 数据库旨在使节点出现故障，而不会丢失数据和停机。 例如，CockroachDB 可以通过在群集中的节点之间维护所有数据的三个一致副本，来经受计算机丢失。

Kubernetes 使用 *服务构造* 来允许客户端从单个 DNS 条目寻址一组完全相同的 NewSQL 数据库进程。 通过将数据库实例与相关联的服务地址进行分隔，可以扩展，而不会中断现有的应用程序实例。 在给定时间向任何服务发送请求将始终产生相同的结果。

在这种情况下，所有数据库实例都相等。 没有主要关系或次要关系。 CockroachDB 中的 *共识复制* 等技术允许任何数据库节点处理任何请求。 如果接收负载平衡请求的节点在本地具有所需的数据，则该节点会立即响应。 如果不是，则该节点将成为网关，并将请求转发到相应的节点以获得正确的答案。 从客户端的角度来看，每个数据库节点都是相同的：它们显示为单一的 *逻辑* 数据库，具有单计算机系统的一致性保证，尽管在幕后甚至有数百个节点在幕后工作。

有关 NewSQL 数据库背后的机制的详细信息，请参阅 " [Kubernetes-Native 数据库的四个属性" 一](https://thenewstack.io/dash-four-properties-of-kubernetes-native-databases/) 文。

## <a name="data-migration-to-the-cloud"></a>数据迁移到云

一个耗时的任务就是将数据从一个数据平台迁移到另一个数据平台。 [Azure 数据迁移服务](https://azure.microsoft.com/services/database-migration/)可帮助加快此类工作。 它可以将数据从多个外部数据库源迁移到 Azure 数据平台，并且停机时间最短。 目标平台包括以下服务：

- Azure SQL 数据库
- Azure Database for MySQL
- Azure Database for MariaDB
- Azure Database for PostgreSQL
- CosmosDB
  
该服务提供了一些建议，指导你完成执行迁移所需的更改，即小型或大型迁移。

>[!div class="step-by-step"]
>[上一页](distributed-data.md)
>[下一页](azure-caching.md)
