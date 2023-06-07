---
title: 通过 F# 实现 Azure 表格存储入门
description: 使用 Azure 表存储或 Azure Cosmos DB 将结构化数据存储在云中。
author: sylvanc
ms.date: 03/26/2018
ms.custom: devx-track-fsharp
ms.openlocfilehash: bf4f2e63c847e18d253fe5b6cf5dd7773c320fb7
ms.sourcegitcommit: a8a205034eeffc7c3e1bdd6f506a75b0f7099ebf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/06/2020
ms.locfileid: "91756203"
---
# <a name="get-started-with-azure-table-storage-and-the-azure-cosmos-db-table-api-using-f"></a>开始使用 Azure 表存储和 Azure Cosmos DB 表 API 使用 F\#

Azure 表存储是一种将结构化的 NoSQL 数据存储在云中的服务。 表存储是采用无架构设计的键/属性存储。 因为表存储无架构，因此可以很容易地随着应用程序需求的发展使数据适应存储。 对于所有类型的应用程序，都可以快速并经济高效地访问数据。 对于相似的数据量，表存储的成本通常显著低于传统的 SQL。

可以使用表存储来存储灵活的数据集，例如 Web 应用程序的用户数据、通讯簿、设备信息，以及服务需要的任何其他类型的元数据。 可以在表中存储任意数量的实体，并且一个存储帐户可以包含任意数量的表，直至达到存储帐户的容量极限。

Azure Cosmos DB 提供了为 Azure 表存储编写的应用程序的表 API，以及需要高级功能的应用程序，例如：

- 统包式全局分发。
- 全球范围内专用的吞吐量。
- 99% 的情况下低至个位数的毫秒级延迟。
- 保证高可用性。
- 自动编制辅助索引。

为 Azure 表存储编写的应用程序无需更改代码便可使用表 API 迁移到 Azure Cosmos DB，并可充分利用高级功能。 表 API 包含可用于 .NET、Java、Python 和 Node.js 的客户端 SDK。

有关详细信息，请参阅 [Azure Cosmos DB 表 API 简介](/azure/cosmos-db/table-introduction)。

## <a name="about-this-tutorial"></a>关于本教程

本教程演示如何使用 Azure 表存储或 Azure Cosmos DB 表 API 编写 F # 代码来执行一些常见任务，包括创建和删除表以及插入、更新、删除和查询表数据。

## <a name="prerequisites"></a>必备知识

若要使用本指南，必须先 [创建 Azure 存储帐户](/azure/storage/storage-create-storage-account) 或 [Azure Cosmos DB 帐户](https://azure.microsoft.com/try/cosmosdb/)。

## <a name="create-an-f-script-and-start-f-interactive"></a>创建 F # 脚本并开始 F# 交互窗口

本文中的示例可用于 F # 应用程序或 F # 脚本。 若要创建 F # 脚本，请创建 `.fsx` 扩展名为的文件，例如 `tables.fsx` f # 开发环境中的文件。

接下来，使用 [包管理器](package-management.md) （如 [Paket](https://fsprojects.github.io/Paket/) 或 [NuGet](https://www.nuget.org/) ） `WindowsAzure.Storage` `WindowsAzure.Storage.dll` 通过指令在脚本中安装包和引用 `#r` 。 为 `Microsoft.WindowsAzure.ConfigurationManager` 获取 Microsoft Azure 命名空间，请再次执行此操作。

### <a name="add-namespace-declarations"></a>添加命名空间声明

将下列 `open` 语句添加到 `tables.fsx` 文件顶部：

[!code-fsharp[TableStorage](~/samples/snippets/fsharp/azure/table-storage.fsx#L1-L5)]

### <a name="get-your-azure-storage-connection-string"></a>获取 Azure 存储连接字符串

如果要连接到 Azure 存储表服务，则需要为本教程提供连接字符串。 可以从 Azure 门户复制连接字符串。 有关连接字符串的详细信息，请参阅 [配置存储连接字符串](/azure/storage/storage-configure-connection-string)。

### <a name="get-your-azure-cosmos-db-connection-string"></a>获取 Azure Cosmos DB 连接字符串

如果要连接到 Azure Cosmos DB，则需要此教程的连接字符串。 可以从 Azure 门户复制连接字符串。 在 Azure 门户的 Cosmos DB 帐户中， **Settings**  >  选择 "设置" "**连接字符串**"，并选择 "**复制**" 按钮复制主连接字符串。

对于本教程，请在脚本中输入连接字符串，如下面的示例所示：

[!code-fsharp[TableStorage](~/samples/snippets/fsharp/azure/table-storage.fsx#L11-L11)]

但对于实际项目， **不建议这样做** 。 存储帐户密钥类似于存储帐户的根密码。 始终要小心保护存储帐户密钥。 避免将其分发给其他用户、对其进行硬编码或将其保存在其他人可以访问的纯文本文件中。 如果你认为密钥可能已泄漏，你可以使用 Azure 门户重新生成密钥。

对于实际应用程序，维护存储连接字符串的最佳方式是在配置文件中。 若要从配置文件中提取连接字符串，可以执行以下操作：

[!code-fsharp[TableStorage](~/samples/snippets/fsharp/azure/table-storage.fsx#L13-L15)]

不一定要使用 Azure Configuration Manager。 你还可以使用 API，例如 .NET Framework 的 `ConfigurationManager` 类型。

### <a name="parse-the-connection-string"></a>解析连接字符串

若要分析连接字符串，请使用：

[!code-fsharp[TableStorage](~/samples/snippets/fsharp/azure/table-storage.fsx#L21-L22)]

这会返回一个 `CloudStorageAccount` 。

### <a name="create-the-table-service-client"></a>创建表服务客户端

`CloudTableClient`利用类，您可以检索表存储中的表和实体。 下面是创建服务客户端的一种方法：

[!code-fsharp[TableStorage](~/samples/snippets/fsharp/azure/table-storage.fsx#L28-L29)]

现在，已准备好编写从表存储读取数据并将数据写入表存储的代码。

### <a name="create-a-table"></a>创建表

此示例演示如何创建表（如果表已经不存在）：

[!code-fsharp[TableStorage](~/samples/snippets/fsharp/azure/table-storage.fsx#L35-L39)]

### <a name="add-an-entity-to-a-table"></a>将实体添加到表

实体必须具有从继承的类型 `TableEntity` 。 您可以 `TableEntity` 采用任何喜欢的方式进行扩展，但您的类型 *必须* 具有无参数的构造函数。 只有具有和的属性 `get` `set` 存储在 Azure 表中。

实体的分区和行键唯一标识表中的实体。 查询分区键相同的实体的速度快于查询分区键不同的实体的速度，但使用不同的分区键可实现更高的并行操作可伸缩性。

下面的示例 `Customer` 使用 `lastName` 作为分区键，并使用 `firstName` 作为行键。

[!code-fsharp[TableStorage](~/samples/snippets/fsharp/azure/table-storage.fsx#L45-L52)]

现在将添加 `Customer` 到表中。 为此，请创建一个 `TableOperation` 对表执行的。 在这种情况下，你将创建一个 `Insert` 操作。

[!code-fsharp[TableStorage](~/samples/snippets/fsharp/azure/table-storage.fsx#L54-L55)]

### <a name="insert-a-batch-of-entities"></a>插入一批实体

您可以使用单个写入操作将一批实体插入到表中。 批处理操作允许将操作组合成单个执行，但有一些限制：

- 可以在同一批处理操作中执行更新、删除和插入操作。
- 一个批处理操作最多可包含100个实体。
- 批处理操作中的所有实体都必须具有相同的分区键。
- 尽管可以在批处理操作中执行查询，但它必须是批处理中的唯一操作。

下面是将两个插入合并为一个批处理操作的一些代码：

[!code-fsharp[TableStorage](~/samples/snippets/fsharp/azure/table-storage.fsx#L62-L71)]

### <a name="retrieve-all-entities-in-a-partition"></a>检索分区中的所有实体

若要查询表以获取某个分区中的所有实体，请使用 `TableQuery` 对象。 在这里，您将筛选 "Smith" 为分区键的实体。

[!code-fsharp[TableStorage](~/samples/snippets/fsharp/azure/table-storage.fsx#L77-L82)]

现在可以打印结果：

[!code-fsharp[TableStorage](~/samples/snippets/fsharp/azure/table-storage.fsx#L84-L85)]

### <a name="retrieve-a-range-of-entities-in-a-partition"></a>检索分区中的一部分实体

如果不想查询分区中的所有实体，则可以通过结合使用分区键筛选器与行键筛选器来指定一个范围。 在这里，您将使用两个筛选器来获取 "Smith" 分区中的行键 (名) 以字母 "M" 前面的字母开头的所有实体。

[!code-fsharp[TableStorage](~/samples/snippets/fsharp/azure/table-storage.fsx#L91-L100)]

现在可以打印结果：

[!code-fsharp[TableStorage](~/samples/snippets/fsharp/azure/table-storage.fsx#L102-L103)]

### <a name="retrieve-a-single-entity"></a>检索单个实体

可以编写查询以检索单个特定实体。 在这里，您将使用 `TableOperation` 来指定客户 "Ben Smith"。 返回，而不是集合 `Customer` 。 在查询中同时指定分区键和行键是从表服务中检索单个实体的最快方法。

[!code-fsharp[TableStorage](~/samples/snippets/fsharp/azure/table-storage.fsx#L109-L111)]

现在可以打印结果：

[!code-fsharp[TableStorage](~/samples/snippets/fsharp/azure/table-storage.fsx#L113-L115)]

### <a name="replace-an-entity"></a>替换实体

若要更新实体，请从表服务中检索它，修改实体对象，然后使用操作将更改保存回表服务 `Replace` 。 这会导致在服务器上完全替换该实体，除非服务器上的该实体自检索后已更改，在这种情况下，该操作将失败。 此失败是为了防止应用程序无意中覆盖其他源的更改。

[!code-fsharp[TableStorage](~/samples/snippets/fsharp/azure/table-storage.fsx#L121-L128)]

### <a name="insert-or-replace-an-entity"></a>插入或替换实体

有时，您不知道实体是否存在于表中。 如果是这样，则不再需要存储在其中的当前值。 `InsertOrReplace`如果实体存在，则可以使用创建实体，而不考虑其状态。

[!code-fsharp[TableStorage](~/samples/snippets/fsharp/azure/table-storage.fsx#L134-L141)]

### <a name="query-a-subset-of-entity-properties"></a>查询一部分实体属性

表查询可以只检索实体中的几个属性，而不是所有这些属性。 此方法称为 "投影"，可提高查询性能，尤其适用于大型实体。 此处，使用和仅返回电子邮件 `DynamicTableEntity` 地址 `EntityResolver` 。 本地存储模拟器不支持投影，因此此代码仅在使用表服务中的帐户时运行。

[!code-fsharp[TableStorage](~/samples/snippets/fsharp/azure/table-storage.fsx#L147-L158)]

### <a name="retrieve-entities-in-pages-asynchronously"></a>以异步方式检索页中的实体

如果正在读取大量实体，并且想要在检索它们时处理它们，而不是等待返回全部实体，则可以使用分段查询。 在这里，你将使用异步工作流返回页面结果，以便在你等待返回大量结果时不会阻止执行。

[!code-fsharp[TableStorage](~/samples/snippets/fsharp/azure/table-storage.fsx#L163-L178)]

现在，你可以同步执行此计算：

[!code-fsharp[TableStorage](~/samples/snippets/fsharp/azure/table-storage.fsx#L180-L180)]

### <a name="delete-an-entity"></a>删除实体

检索实体后，可以将其删除。 与更新实体一样，如果实体在检索后发生更改，则此操作将失败。

[!code-fsharp[TableStorage](~/samples/snippets/fsharp/azure/table-storage.fsx#L186-L187)]

### <a name="delete-a-table"></a>删除表

可以从存储帐户中删除表。 在删除表之后的一段时间内无法重新创建它。

[!code-fsharp[TableStorage](~/samples/snippets/fsharp/azure/table-storage.fsx#L193-L193)]

## <a name="next-steps"></a>后续步骤

现在，你已了解有关表存储的基础知识，请单击下面的链接了解更复杂的存储任务和 Azure Cosmos DB 表 API。

- [Azure Cosmos DB 简介表 API](/azure/cosmos-db/table-introduction)
- [.NET 存储客户端库参考](/dotnet/api/overview/azure/storage)
- [Azure 存储类型提供程序](https://fsprojects.github.io/AzureStorageTypeProvider/)
- [Azure 存储团队博客](/archive/blogs/windowsazurestorage/)
- [配置连接字符串](/azure/storage/common/storage-configure-connection-string)
