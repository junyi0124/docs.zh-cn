---
title: 云本机复原能力
description: 构建适用于 Azure 的云本机 .NET 应用 |云本机复原
author: robvet
ms.date: 05/13/2020
ms.openlocfilehash: 5c4fb261515c151fd666cc33cbb020447716c814
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91163552"
---
# <a name="cloud-native-resiliency"></a>云本机复原能力

复原能力是指系统能够应对故障，并仍能正常工作。 这并不是为了避免故障，而是接受故障，并构建云本机服务来响应它。 你需要尽快返回到完全正常运行的状态。

与传统的单一应用程序不同，在一个进程中，所有内容都一起运行，云本机系统采用分布式体系结构，如图6-1 所示：

![分布式云-本机环境](./media/distributed-cloud-native-environment.png)

**图 6-1**。 分布式云-本机环境

在上图中，每个微服务和基于云的 [支持服务](https://12factor.net/backing-services) 在单独的进程中执行，跨服务器基础结构，通过基于网络的调用进行通信。

在此环境中操作，服务必须对许多不同的难题保密：

- 意外的网络延迟-服务请求到达接收方和背面的时间。

- [暂时性故障](/azure/architecture/best-practices/transient-faults) -生存期较短的网络连接错误。

- 由长时间运行的同步操作所阻塞。

- 已崩溃并且正在重新启动或移动的主机进程。

- 一段时间后无法响应的重载微服务。

- 正在进行的 orchestrator 操作，例如滚动升级或将服务从一个节点移到另一个节点。

- 硬件故障。

云平台可以检测并缓解其中许多基础结构问题。 它可以重启、横向扩展，甚至可将你的服务重新分发到另一个节点。  但是，若要充分利用此内置保护，你必须将你的服务设计为对其做出反应并在此动态环境中发展。

在以下部分中，我们将探讨你的服务和托管云资源可利用的防御性技术，以最大程度地减少停机时间和中断时间。

>[!div class="step-by-step"]
>[上一页](elastic-search-in-azure.md)
>[下一页](application-resiliency-patterns.md)
