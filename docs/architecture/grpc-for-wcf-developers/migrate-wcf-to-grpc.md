---
title: 将 WCF 解决方案迁移到 WCF 开发人员的 gRPC-gRPC
description: 如何将不同类型的 WCF 服务迁移到 gRPC 中的等效类型。
ms.date: 12/15/2020
ms.openlocfilehash: 3bd35cb6119368ff3db3be9ab5fabf89f2652b29
ms.sourcegitcommit: 655f8a16c488567dfa696fc0b293b34d3c81e3df
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/06/2021
ms.locfileid: "97937956"
---
# <a name="migrate-a-wcf-solution-to-grpc"></a>将 WCF 解决方案迁移到 gRPC

本章介绍如何使用 ASP.NET Core 5.0 gRPC 项目，并演示如何将不同类型的 Windows Communication Foundation (WCF) 服务迁移到 gRPC 等效项：

- 创建 ASP.NET Core 5.0 gRPC 项目。
- 简单的请求-答复操作到 gRPC 一元 RPC。
- 用于 gRPC 客户端流式处理 RPC 的单向操作。
- 全双工服务，gRPC 双向流式处理 RPC。

还比较了如何使用流式处理服务与重复字段来返回数据集，并在本章末尾讨论了如何使用客户端库。

示例 WCF 应用程序是一组股票交易服务的最小存根。 它使用 (IoC) 名为 Autofac 的容器库进行依赖关系注入的开源反转控制。 它包括三个服务，每个服务类型一个。 以下部分将更详细地讨论服务。 可以从 GitHub 上的 [dotnet/grpc-for wcf 开发人员](https://github.com/dotnet-architecture/grpc-for-wcf-developers) 下载解决方案。 服务使用虚设数据来最大程度地减少外部依赖项。

这些示例包括每个服务的 WCF 和 gRPC 实现。

>[!div class="step-by-step"]
>[上一页](ws-protocols.md)
>[下一页](create-project.md)
