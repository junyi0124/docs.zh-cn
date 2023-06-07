---
title: 选择正确的 Azure 托管选项
description: 了解 ASP.NET Web 应用程序适合使用哪个 Azure 迁移路径。
author: CESARDELATORRE
ms.author: cesardl
ms.date: 03/01/2020
ms.openlocfilehash: 162dc8eb87dfd78d050b93b1c24ac573d7092126
ms.sourcegitcommit: cb27c01a8b0b4630148374638aff4e2221f90b22
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/09/2020
ms.locfileid: "86174291"
---
# <a name="choose-the-right-azure-hosting-option"></a>选择正确的 Azure 托管选项

本文提供将现有 .NET Framework 应用程序从本地迁移到 Azure 时，可在 Azure 中使用多个选项的注意事项及其比较。

将现有 .NET 应用程序迁移到 Azure 时要考虑的基本方面包括：

1. 计算选项
1. 数据库选项
1. 网络和安全注意事项
1. 身份验证和授权注意事项

## <a name="compute-choices"></a>计算选项

将现有的 .NET Framework 应用程序迁移到 Azure 时，可以使用多个选项。 但是，由于 .NET Framework 依赖于 Windows，只能对基于 Windows 的计算服务使用以下选项。

下表显示了一些比较和建议，帮助你为现有的 .NET 应用程序选择适当的计算迁移路径。

|                 | Azure VM | Azure 应用服务 | Windows 容器 |
|-----------------|-----------|-------------------|--------------------|
|何时使用      |<ul><li>应用程序严重依赖于服务器和本地 .msi 安装。</li><li>想要使用最简单的应用程序迁移路径</li></ul>|应用在服务器上没有任何依赖项，只是一个能够访问数据库服务器的简洁 ASP.NET Web 应用（MVC、Web 窗体）或 N 层应用（Web API、WCF）。 |<ul><li>应用程序在原始服务器上存在依赖项，但可以在 Docker Windows 映像中包含这些依赖项。</li><li>想要将应用现代化，使其[随时可用于云 DevOps](../../architecture/modernize-with-azure-containers/modernize-existing-apps-to-cloud-optimized/reasons-to-modernize-existing-net-apps-to-cloud-optimized-applications.md)</li></ul>|
|优点和好处  |<ul><li>最简单的迁移路径</li><li>熟悉的环境。 部署环境是一个 VM，因此类似于本地服务器。</li></ul> |持续进行 PaaS 维护，在 Azure 中管理和缩放应用的最简单方法。 |<ul><li>应对未来需要，随时可用于云 DevOps，且在应用的容器中包含依赖项。</li><li>几乎不需要重构 .NET/C# 代码。</li></ul> |
|缺点             |属于 IaaS。 维护费用高昂。 必须管理 VM 基础结构的网络、负载均衡器、横向扩展和 IIS 管理等方方面面。 |<ul><li>并非[支持](https://appmigration.microsoft.com/assessment)所有应用</li><li>某些应用可能需要重构，甚至稍微调整体系结构，这样才能支持 Azure 应用服务。</li></ul> |<ul><li>Docker 技能学习曲线</li><li>需要更改某些代码和应用配置设置</li></ul>|
|要求 |Windows Server VM 的要求与本地应用相同 | [准备情况检查](https://github.com/Azure/App-Service-Migration-Assistant/wiki/Readiness-Checks)中指定的 Azure 应用服务要求。 |<ul><li>[Docker 引擎 - Windows Server 2019 企业版](https://azuremarketplace.microsoft.com/marketplace/apps/cloud-infrastructure-services.docker-windows-2019)<br />or</li><li>[Azure 容器服务 (AKS)](https://azure.microsoft.com/services/container-service/)（即 Kubernetes 业务流程协调程序）<br />or<li>[Azure Service Fabric](https://azure.microsoft.com/services/service-fabric/) 业务流程协调程序</li></ul> |
|如何迁移 |请参阅[迁移到 Azure 虚拟机](vm.md) | 请参阅[迁移 Azure 应用服务](app-service.md) | 遵循[使用 Azure 和 Windows 容器将现有 .NET 应用现代化电子书](https://aka.ms/liftandshiftwithcontainersebook)中的注意事项、方案和演练 |

以下流程图显示了在规划向 Azure 迁移现有 .NET Framework 应用程序时的决策树。 如果可行，请先尝试选项 A，但选项 B 是最容易执行的路径。

![显示托管决策树的流程图](../media/migration/choose/decision-tree.png)

## <a name="database-choices"></a>数据库选项

将关系型数据库迁移到 Azure 时，可以使用多个选项。 请参阅[将 SQL Server 数据库迁移到 Azure](sql.md)，帮助自己选择适合现有 .NET 应用程序的数据库迁移路径。

## <a name="networking-and-security-considerations"></a>网络和安全注意事项

将应用程序部署到 Microsoft Azure 等公有云时，可能需要通过[创建外围网络](/azure/architecture/reference-architectures/dmz/)（例如，[Azure 与本地之间的外围网络](/azure/architecture/reference-architectures/dmz/secure-vnet-hybrid)，或 [Azure 与 Internet 之间的外围网络](/azure/architecture/reference-architectures/dmz/secure-vnet-dmz)），来隔离并保护特定的网络。 可以使用 [Azure 虚拟网络](/azure/virtual-network/virtual-networks-overview)实施外围网络。

使用 Azure 虚拟网络可以：

- 构建可控的混合基础结构
- 自带 IP 地址和 DNS 服务器
- 借助 IPsec VPN 或 ExpressRoute 确保连接安全
- 精细控制子网之间的流量
- 使用虚拟设备创建复杂的网络拓扑
- 向应用程序提供独立且高度安全的环境

若要开始构建自己的虚拟网络，请参阅 [Azure 虚拟网络文档](/azure/virtual-network/)。

## <a name="authentication-and-authorization-considerations-when-migrating-to-azure"></a>迁移到 Azure 时的身份验证和授权注意事项

对于任何组织而言，迁移到云的最重要考虑因素是安全性。 大多数公司已投入大量的时间、资金和工程力量来设计和开发安全模型，并且必须能够利用现有投资，例如标识存储和单一登录解决方案。

本地运行的许多现有企业 B2E .NET 应用程序使用 Active Directory 进行身份验证和标识管理。 使用 Azure AD Connect 可将本地目录与 Azure Active Directory 集成。 若要开始集成，请参阅[将本地目录与 Azure Active Directory 集成](/azure/active-directory/connect/active-directory-aadconnect)。

请参阅[混合标识解决方案的标识要求](/azure/active-directory/active-directory-hybrid-identity-design-considerations-business-needs)进行 Azure Active Directory 相关的其他规划。

其他身份验证协议选项包括消费型应用程序中经常使用的 [OAuth](https://en.wikipedia.org/wiki/OAuth) 和 [OpenID](https://en.wikipedia.org/wiki/OpenID)。 使用自治标识数据库（例如，IdentityServer4 使用 OAuth 包装的 ASP.NET 标识 SQL 数据库）时，通常不需要与本地数据库或目录建立连接。

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [将 ASP.NET Web 应用程序迁移到 Azure 应用服务](app-service.md)
