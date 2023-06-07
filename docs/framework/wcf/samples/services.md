---
title: 服务示例
ms.date: 03/30/2017
ms.assetid: 462a2218-f8c6-4fb7-95bc-64765459c429
ms.openlocfilehash: a225d9bfb4d0ab70904a6e03c22269d69122f00b
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/09/2020
ms.locfileid: "84591453"
---
# <a name="services"></a>服务

本节包含演示 Windows Communication Foundation （WCF）服务的示例。

## <a name="in-this-section"></a>本节内容

- [提供](../feature-details/hosting.md)\
演示如何承载 WCF 服务。

- [服务互操作性](service-interoperability.md)\
说明 WCF 与其他服务技术之间的交互。

- [行为](behaviors.md)\
演示 WCF 服务行为。

- [服务安全](service-security.md)\
说明 WCF 服务安全性。

- [WCF 服务的简化配置](simplified-configuration-for-wcf-services.md)\
演示如何使用 WCF 实现和配置典型的服务和客户端。

- [标准终结点的用法](usage-of-standard-endpoints.md)\
演示如何在服务配置文件中使用标准终结点。

- [扩展保护策略](extended-protection-policy.md)\
演示扩展保护，这是一种针对中间人 (MITM) 攻击而提供保护的安全计划。

- [配置通道工厂](configuration-channel-factory.md)\
演示 <xref:System.ServiceModel.Configuration.ConfigurationChannelFactory%601> 的用法。

- [寻址](addressing.md)\
演示终结点地址的各个方面和功能。

- [命令性](imperative.md)\
演示如何使用代码为服务定义 <xref:System.ServiceModel.WSHttpBinding>，而不是在配置中定义 `wsHttpBinding` 绑定。

- [多个协定](multiple-contracts.md)\
演示如何在一个服务上实现多个协定和如何配置终结点以便与每个实现的协定通信。

- [多个终结点](multiple-endpoints.md)\
演示如何在一个服务上配置多个终结点，以及如何从客户端与每个终结点通信。

- [单个 ListenUri 上的多个终结点](multiple-endpoints-at-a-single-listenuri.md)\
演示一个在单个 `ListenUri` 中承载多个终结点的服务。

- [OperationContextScope](operationcontextscope.md)\
演示如何使用标头在 WCF 调用上发送额外的信息。

- [服务说明](service-description.md)\
演示服务如何在运行时检索其服务说明信息。

- [ConcurrencyMode](concurrencymode-reentrant.md)\
演示如何在服务实现上使用可重入并发模式。
