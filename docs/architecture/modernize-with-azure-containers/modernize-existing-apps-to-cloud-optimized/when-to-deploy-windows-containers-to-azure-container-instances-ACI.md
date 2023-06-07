---
title: 何时将 Windows 容器部署到 Azure 容器实例 (ACI)
description: 通过 Azure 云和 Windows 容器现代化现有 .NET 应用程序 | 何时将 Windows 容器部署到 Azure 容器实例 (ACI)
ms.date: 04/29/2018
ms.openlocfilehash: 6c889db6f0475f24a196144c7fb62faec4c173ed
ms.sourcegitcommit: e3cbf26d67f7e9286c7108a2752804050762d02d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/09/2020
ms.locfileid: "80989150"
---
# <a name="when-to-deploy-windows-containers-to-azure-container-instances-aci"></a>何时将 Windows 容器部署到 Azure 容器实例 (ACI)

Azure 容器实例的主要价值主张是，你可以立即将容器部署到该环境中，而无需维护该环境，无需升级/修补基础操作系统或 VM，所有这些都是透明的，你只需将容器部署到随时可用的环境中即可。

想要使用 ACI 的原因和方案类似于将 Azure VM 与容器一起使用时的主要方案，因此，使用 Azure 容器实例的主要方案基本如下：

- **开发/测试方案**
- **任务自动化**
- **CI/CD 代理**
- **小/大规模批处理**
- **简单的 Web 应用**

简单的 Web 应用方案是一种公平的 ACI 方案，但请考虑这一点：因为在 ACI 中，每个容器映像只能有一个容器实例，你没有高可用性，并且仅具有有限的可伸缩性。

不过，即使 ACI 被视为基础结构，因为它只提供单个容器实例，与使用 Windows Server 的常规 Azure VM 相比，具有巨大的优势。 使用 ACI，只需将容器部署到自行维护的环境中即可，只需为这些容器付费。 你无需维护/更新/修补 VM，因此，在大多数你可能会将 VM 与容器一起使用的情况下，这是一个更好的平台。 使用 ACI 相当直接，只需部署容器，无需创建仅部署容器的 VM 环境。

Azure 容器实例 (ACI) 的主要优点是：

- 在不管理服务器的情况下运行容器
- 根据需要提高容器的灵活性
- 使用单一命令将容器部署到具有空前简易性和速度的云。
- 通过虚拟机监控程序隔离保护应用程序

简而言之，借助 ACI，可以快速开发应用，无需管理虚拟机，也无需了解新的工具。 它只是在云中运行的容器中的应用程序。

> [!div class="step-by-step"]
> [上一页](when-to-deploy-windows-containers-to-azure-vms-iaas-cloud.md)
> [下一页](when-to-deploy-windows-containers-to-azure-container-service-kubernetes.md)
