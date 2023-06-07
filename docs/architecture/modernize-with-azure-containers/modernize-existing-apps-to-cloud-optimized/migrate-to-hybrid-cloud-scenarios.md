---
title: 迁移到混合云方案
description: 通过 Azure 云和 Windows 容器现代化现有 .NET 应用程序 | 迁移到混合云方案
ms.date: 04/30/2018
ms.openlocfilehash: dcbb799a45609f8bb811866c4041951abf1fda8b
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/14/2020
ms.locfileid: "75937367"
---
# <a name="migrate-to-hybrid-cloud-scenarios"></a>迁移到混合云方案

由于法规或其自身的策略，某些组织和企业无法将部分应用程序迁移到公有云，如 Microsoft Azure 或任何其他公有云。 但是，任何组织都可能从公有云中的部分应用程序和本地的其他应用程序中获益。 但混合环境可能会导致环境变得过度复杂，因为公有云和本地环境使用的平台和技术不同。

Microsoft 提供了最佳的混合云解决方案，在此解决方案中，你可以在本地和公有云中优化现有资产，同时确保 Azure 混合云中的一致性。 你可以最大限度地利用现有技能，并获得一种灵活的统一方法来生成可在云中或本地运行的应用，这归功于 Azure Stack（本地）和 Azure（公有云）。

在安全方面，你可以在混合云中集中管理和安全性。 可以通过提供对本地和云应用程序的单一登录，来控制从数据中心到云的所有资产。 为此，可将 Active Directory 扩展到混合云，并使用标识管理。

最后，你可以无缝分发和分析数据，对云和本地资产使用相同的查询语言，并在 Azure 中应用分析和深度学习来丰富数据，而不考虑数据源。

## <a name="azure-stack"></a>Azure Stack

Azure Stack，一种混合云平台，可从组织数据中心提供 Azure 服务。 Azure Stack 旨在支持关键方案（如边缘和未连接环境）中现代应用程序的新选项，或者满足特定的安全性和符合性要求。

图 4-13 显示了 Microsoft 提供的真正混合云平台的概述。

![提供 Azure Stack 和 Azure 的 Microsoft 混合云平台关系图。](./media/migrate-to-hybrid-cloud-scenarios/microsoft-hybrid-cloud-platform.png)

**图 4-13.** 提供 Azure Stack 和 Azure 的 Microsoft 混合云平台

通过两个部署选项提供 Azure Stack 来满足你的需求：

- Azure Stack 集成系统

- Azure Stack 开发工具包

### <a name="azure-stack-integrated-systems"></a>Azure Stack 集成系统

Azure Stack 集成系统通过 Microsoft 和硬件合作伙伴的合作关系提供。 合作关系创建了一个解决方案，它提供了云进度的创新，与管理中的简单性进行平衡。 由于 Azure Stack 是作为硬件和软件的集成系统提供的，因此用户可以获得适当的灵活性和控制能力，同时仍然采用来自云的创新。 Azure Stack 集成系统的大小从 4 个节点到 12 个节点不等，由硬件合作伙伴和 Microsoft 共同提供支持。 使用 Azure Stack 集成系统可实施针对生产工作负载的新方案。

### <a name="azure-stack-development-kit"></a>Azure Stack 开发工具包

Microsoft Azure Stack 开发工具包是 Azure Stack 的单节点部署，可用于评估和了解 Azure Stack。 还可以将 Azure Stack 开发工具包用作开发人员环境，在其中使用与 Azure 一致的 API 和工具进行开发。 Azure Stack 开发工具包不应作为生产环境使用。

### <a name="additional-resources"></a>其他资源

- **Azure 混合云**

    <https://azure.microsoft.com/overview/hybrid-cloud/>

- **Azure Stack**

    <https://azure.microsoft.com/overview/azure-stack/>

- **Windows 容器的 Active Directory 服务帐户**

    <https://docs.microsoft.com/virtualization/windowscontainers/manage-containers/manage-serviceaccounts>

- **借助 Active Directory 支持创建容器**

    <https://docs.microsoft.com/archive/blogs/containerstuff/create-a-container-with-active-directory-support>

- **Azure 混合权益许可**

    <https://azure.microsoft.com/pricing/hybrid-benefit/>

>[!div class="step-by-step"]
>[上一页](life-cycle-ci-cd-pipelines-devops-tools.md)
>[下一页](../walkthroughs-technical-get-started-overview.md)
