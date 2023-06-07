---
title: 适用于 WCF 开发人员的 gRPC 概述-gRPC
description: 了解指导开发 gRPC 的原则集。
ms.date: 12/15/2020
ms.openlocfilehash: 99e1bdb1f49469f444044027c3ac5d927f9cab13
ms.sourcegitcommit: 655f8a16c488567dfa696fc0b293b34d3c81e3df
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/06/2021
ms.locfileid: "97938398"
---
# <a name="grpc-overview"></a>gRPC 概述

查看最后一章中 Windows Communication Foundation (WCF) 和 gRPC 的 genesis 后，本章将介绍 gRPC 的一些主要功能及其与 WCF 的比较方式。

ASP.NET Core 3.0 是 ASP.NET 的第一个版本，该版本本身支持 gRPC 作为第一类公民，由 Microsoft 团队共同提供 gRPC 的官方 .NET 实现。 建议使用 .NET 生成分布式应用程序，这些应用程序可与所有其他主要编程语言和框架互操作。

## <a name="key-principles"></a>关键原则

如第1章所述，Google 希望使用 HTTP/2 的引入来替换 Stubby，它的内部通用 RPC 基础结构。 基于 Stubby 的 gRPC 现在可以利用标准化，并将其适用性扩展到移动计算、云和物联网。

为了实现这一标准化， [云本机计算基础 (CNCF) ](https://www.cncf.io/) 建立了一组可管理 gRPC 的原则。 以下列表显示了最相关的列表，主要涉及到最大限度地提高可访问性和可用性：

- **免费和开放** –所有项目都应该是开放源代码，其中的许可不会限制开发人员采用 gRPC。
- **覆盖面和简易性** – gRPC 应在每个主流平台上可用，并可以轻松地在任何平台上构建。
- **互操作性和联系性** –应使用广泛可用的网络标准，在任何网络上都可以使用 gRPC，而不考虑带宽或延迟。
- "**常规用途" 和**"高性能" –框架应尽可能广泛地用于各种用例，而不会影响性能。
- **流式处理** –协议应为大型数据集或异步消息传送提供流式传输语义。
- **元数据交换** –协议允许与实际的业务数据分开处理非业务数据，例如身份验证令牌。
- **标准化状态代码** –应降低错误代码的可变性，使错误处理决策更清晰。 如果需要更多更丰富的错误处理，则应提供一种机制来管理元数据交换中的行为。

>[!div class="step-by-step"]
>[上一页](introduction.md)
>[下一页](approach.md)
