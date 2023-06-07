---
title: 何时将 Windows 容器部署到 Azure 容器服务（即 Kubernetes）
description: 通过 Azure 云和 Windows 容器现代化现有 .NET 应用程序 | 何时将 Windows 容器部署到 Azure 容器服务（即 Kubernetes）
ms.date: 04/30/2018
ms.openlocfilehash: 3ff51a70d4e158b831155ab4992ec32f5d98cedb
ms.sourcegitcommit: e3cbf26d67f7e9286c7108a2752804050762d02d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/09/2020
ms.locfileid: "80987759"
---
# <a name="when-to-deploy-windows-containers-to-azure-container-service-that-is-kubernetes"></a>何时将 Windows 容器部署到 Azure 容器服务（即 Kubernetes）

Azure 容器服务优化了专门针对 Azure 的常用开源工具和技术的配置。 你可以获得一个开放的解决方案，该解决方案为容器和应用程序配置提供可移植性。 你选择主机的大小和数量以及业务流程协调程序工具。 Azure 容器服务可为你处理基础结构。

如果你已使用开源业务流程协调程序（如 Kubernetes、Docker Swarm 或 DC/OS），则无需更改现有的管理实践即可将容器工作负载迁移到云。 针对所选的业务流程协调程序，使用熟悉的应用程序管理工具并通过标准 API 终结点连接。

如果你使用的是 Linux Docker 容器，则所有这些业务流程协调程序都是成熟环境；但对于 Windows 容器，它们可能只是处于预览版状态。

例如，在 Kubernetes 中，对容器的支持是本机的（一等公民），因此，在 Kubernetes 上使用 Windows 容器也是有效的（截止到 2018 年初在 ACS 中处于预览版状态）。

重要说明：AKS（Azure Kubernetes 服务）是适用于 Kubernetes 的 ACS（Azure 容器服务）的进化和“更多 PaaS”版本，不过，截止到 2018 年第 2 季度 Windows 容器仍不受支持，但不久就会受支持。

>[!div class="step-by-step"]
>[上一页](when-to-deploy-windows-containers-to-azure-container-instances-ACI.md)
>[下一页](choosing-azure-compute-options-for-container-based-applications.md)
