---
title: 何时将 Windows 容器部署到 Azure VM（IaaS 云）
description: 通过 Azure 云和 Windows 容器现代化现有 .NET 应用程序 | 何时将 Windows 容器部署到 Azure VM（IaaS 云）
ms.date: 04/28/2018
ms.openlocfilehash: e9a2903662306b607977a7751018e24161ab80ab
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/14/2020
ms.locfileid: "69577900"
---
# <a name="when-to-deploy-windows-containers-to-azure-vms-iaas-cloud"></a>何时将 Windows 容器部署到 Azure VM（IaaS 云）

如果你的组织使用 Azure VM，则即使你还使用 Windows 容器，仍将处理 IaaS。 这意味着，当你需要在负载均衡的基础结构中部署到多个 VM 时，应处理高度可缩放的应用程序的基础结构操作、VM 操作系统修补程序和基础结构复杂性。 在 Azure VM 中使用 Windows 容器的主要方案是：

- **开发/测试环境**：云中的 VM 非常适合在云中进行开发和测试。 可以根据需要快速创建或停止环境。

- **中小型的可伸缩性需求**：在生产环境可能只需要几个 VM 的方案中，可能负担得起管理少量 VM，直到迁移到更高级的 PaaS 环境（如业务流程协调程序）。

- **包含现有部署工具的生产环境**：可能迁移自本地环境，在该环境中，已对工具进行投资来对 VM 或裸机服务器（如 Puppet 或类似的工具）进行复杂的部署。 若要通过对生产环境部署过程进行少量的更改来迁移到云，可以继续使用这些工具部署到 Azure VM。 但是，你需要将 Windows 容器用作部署单位来改善部署体验。

>[!div class="step-by-step"]
>[上一页](when-to-deploy-windows-containers-in-your-on-premises-iaas-vm-infrastructure.md)
>[下一页](when-to-deploy-windows-containers-to-azure-container-instances-ACI.md)
