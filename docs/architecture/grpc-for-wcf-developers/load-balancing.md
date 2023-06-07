---
title: 针对 WCF 开发人员的负载均衡 gRPC-gRPC
description: 选择要使用 gRPC services 的负载均衡器。
ms.date: 12/15/2020
ms.openlocfilehash: 55f61608dce1f159b11d7265a47938ba49e9e188
ms.sourcegitcommit: 655f8a16c488567dfa696fc0b293b34d3c81e3df
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/06/2021
ms.locfileid: "97938580"
---
# <a name="load-balancing-grpc"></a>负载均衡 gRPC

GRPC 应用程序的典型部署包含多个相同的服务实例，提供复原能力和水平可扩展性。 负载均衡将传入请求分布到这些实例上，以充分利用所有可用资源。 若要使此负载平衡对客户端不可见，通常使用代理负载均衡器服务器来处理来自客户端的请求，并将这些请求路由到后端实例。

负载均衡器根据其运行的 *层* 进行分类。 第4层负载均衡器适用于 *传输* 级别，例如，使用 TCP 套接字、连接和数据包。 第7层负载均衡器在 *应用程序* 级别工作，专用于处理 gRPC 应用程序的 HTTP/2 请求。

## <a name="l4-load-balancers"></a>L4 负载均衡器

L4 负载均衡器接受来自客户端的 TCP 连接请求，打开与某个后端实例的另一个连接，并在两个连接之间复制数据，无实际处理。 L4 提供出色的性能和低延迟，但控制或智能更少。 只要客户端使连接保持打开状态，所有请求都将定向到相同的后端实例。

 [Azure 负载均衡器](https://azure.microsoft.com/services/load-balancer/) 是 L4 负载均衡器的一个示例。

## <a name="l7-load-balancers"></a>L7 负载均衡器

L7 负载均衡器分析传入的 HTTP/2 请求，并按请求将这些请求按请求传递到后端实例，无论客户端保持连接的时间长短。

L7 负载均衡器的示例：

- [NGINX](https://www.nginx.com/)
- [HAProxy](https://www.haproxy.com/)
- [Traefik](https://traefik.io/)

作为经验法则，L7 负载均衡器是 gRPC 和其他 HTTP/2 应用程序的最佳选择，对于 HTTP 应用程序 (而言，通常) 。 L4 负载均衡器 *适用* 于 gRPC 应用程序，但在低延迟和低开销很重要的情况下，它们主要是非常有用的。

> [!IMPORTANT]
> 撰写本文时，某些 L7 负载均衡器不支持 gRPC 服务所需的 HTTP/2 规范的所有部分，例如尾随标头。

如果使用的是 TLS 加密，负载均衡器可以终止 TLS 连接并将未加密的请求传递到后端应用程序，也可以传递加密的请求。 无论采用哪种方式，都需要将负载均衡器配置为具有服务器的公钥和私钥，使其可以解密处理请求。

请参阅你喜欢的负载均衡器的文档，了解如何将其配置为通过后端服务处理 HTTP/2 请求。

## <a name="load-balancing-within-kubernetes"></a>Kubernetes 中的负载均衡

有关 Kubernetes 上内部服务之间的负载平衡的讨论，请参阅 [服务网格部分](service-mesh.md) 。

>[!div class="step-by-step"]
>[上一页](service-mesh.md)
>[下一页](application-performance-management.md)
