---
title: 集中式配置
description: 使用 Azure 应用配置和 AzureKey 保管库集中使用云本机应用程序的配置。
ms.date: 05/13/2020
ms.openlocfilehash: 0d40c5b2d70f30beb17489dfd55900f7c5fc1a75
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91160874"
---
# <a name="centralized-configuration"></a>集中式配置

不同于单个应用，其中的所有内容都在单个实例中运行，云本机应用程序由跨虚拟机、容器和地理区域分布的独立服务构成。 管理数十个相互依赖的服务的配置设置可能非常困难。 不同位置上的配置设置重复的副本容易出错并且难于管理。 集中式配置是分布式云本机应用程序的关键要求。

如 [第1章](introduction.md)所述，十二因素应用建议要求严格分隔代码和配置。 必须根据需要从应用程序外部存储配置并进行读取。 将配置值存储为代码中的常量或文本值是一个冲突。 同一应用程序中的许多服务通常使用相同的配置值。 此外，我们必须跨多个环境（如开发、测试和生产）支持相同的值。 最佳做法是将它们存储在集中式配置存储中。

Azure 云提供了几个不错的选项。

## <a name="azure-app-configuration"></a>Azure 应用配置

[Azure 应用配置](/azure/azure-app-configuration/overview) 是一项完全托管的 Azure 服务，可将非机密配置设置存储在一个安全的集中位置。 存储的值可以在多个服务和应用程序之间共享。

此服务易于使用，并提供了若干优点：

- 灵活键/值表示和映射
- 用 Azure 标签标记
- 用于管理的专用 UI
- 敏感信息的加密
- 查询和批量检索

Azure 应用配置维护7天对键值设置所做的更改。 使用时间点快照功能，您可以重新构造设置的历史记录，甚至可以为失败的部署进行回滚。

应用配置会自动缓存每个设置，以避免过多调用配置存储。 刷新操作会等待，直到某项设置的已缓存值过期才更新该设置，即使其值在配置存储中发生了更改。 默认缓存过期时间为 30 秒。 可以覆盖过期时间。

应用配置对传输中和静态的所有配置值进行加密。 键名称和标签充当检索配置数据的索引，且不进行加密。

尽管应用配置提供强化的安全性，但 Azure Key Vault 仍是存储应用程序机密的最佳位置。 Key Vault 提供硬件级加密、粒度访问策略和管理操作（如证书轮换）。 你可以创建引用存储在 Key Vault 中的密钥的应用配置值。

## <a name="azure-key-vault"></a>Azure Key Vault

Key Vault 是一种托管服务，用于安全地存储和访问机密。 机密是你希望严格控制对其的访问的任何东西，例如 API 密钥、密码或证书。 保管库是机密的逻辑组。

Key Vault 可以大大减少机密意外泄露的可能性。 有了 Key Vault，应用程序开发人员就再也不需要将安全信息存储在应用程序中。 这种做法无需在代码中存储此信息。 例如，如果某个应用程序需要连接到数据库， 则可将连接字符串安全地存储在 Key Vault 中，而不是存储在应用代码中。

应用程序可以使用 URI 安全访问其所需的信息。 这些 URI 允许应用程序检索特定版本的机密。 无需编写自定义代码来保护 Key Vault 中存储的任何机密信息。

访问 Key Vault 需要正确的调用方身份验证和授权。 通常，每个云本机微服务使用 ClientId/ClientSecret 组合。 将这些凭据保留在源代码管理范围内是非常重要的。 最佳做法是在应用程序的环境中对其进行设置。 使用 [Key Vault FlexVolume](https://github.com/Azure/kubernetes-keyvault-flexvol)可以实现从 AKS 的直接访问 Key Vault。

## <a name="configuration-in-eshop"></a>EShop 中的配置

EShopOnContainers 应用程序包含每个微服务的本地应用程序设置文件。 这些文件签入到源代码管理中，但不包括诸如连接字符串或 API 密钥之类的生产机密。 在生产环境中，可能会覆盖每个服务环境变量的各个设置。 在环境变量中注入机密是对托管应用程序的一种常见做法，但并不提供中央配置存储。 为了支持集中管理的配置设置，每个微服务都包含一个设置以在其使用本地设置或 Azure Key Vault 设置之间切换。

## <a name="references"></a>参考

- [EShopOnContainers 体系结构](https://github.com/dotnet-architecture/eShopOnContainers/wiki/Architecture)
- [协调安排微服务和多容器应用程序，实现高可伸缩性和高可用性](../microservices/architect-microservice-container-applications/scalable-available-multi-container-microservice-applications.md)
- [Azure API 管理](/azure/api-management/api-management-key-concepts)
- [Azure SQL 数据库概述](/azure/sql-database/sql-database-technical-overview)
- [用于 Redis 的 Azure 缓存](https://azure.microsoft.com/services/cache/)
- [Azure Cosmos DB 的 API for MongoDB](/azure/cosmos-db/mongodb-introduction)
- [Azure 服务总线](/azure/service-bus-messaging/service-bus-messaging-overview)
- [Azure Monitor 概述](/azure/azure-monitor/overview)
- [eShopOnContainers：在 AKS 中创建 Kubernetes 群集](https://github.com/dotnet-architecture/eShopOnContainers/wiki/Deploy-to-Azure-Kubernetes-Service-(AKS)#create-kubernetes-cluster-in-aks)
- [eShopOnContainers： Azure Dev Spaces](https://github.com/dotnet-architecture/eShopOnContainers/wiki/Azure-Dev-Spaces)
- [Azure Dev Spaces](/azure/dev-spaces/about)

>[!div class="step-by-step"]
>[上一页](deploy-eshoponcontainers-azure.md)
>[下一页](scale-applications.md)
