---
title: 网络协议-适用于 WCF 开发人员的 gRPC
description: GRPC 网络协议的概述。
ms.date: 09/02/2019
ms.openlocfilehash: 801d57c95aec748e5dcf667ca480775ff945b55c
ms.sourcegitcommit: 655f8a16c488567dfa696fc0b293b34d3c81e3df
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/06/2021
ms.locfileid: "97938554"
---
# <a name="network-protocols"></a>网络协议

与 Windows Communication Foundation (WCF) 不同，gRPC 使用 HTTP/2 作为其网络的基础。 与 WCF 和 SOAP 相比，此协议具有明显的优势，只能在 HTTP/1.1 上操作。 对于想要使用 gRPC 的开发人员，如果没有 HTTP/2 的替代方法，则似乎更详细地探讨 HTTP/2，并识别使用 gRPC 的其他优点。

HTTP/2 （由2015中的 Internet 工程任务组发布）派生自 Google 已在使用的实验 SPDY 协议。 它经过专门设计，比 HTTP/1.1 更高效、更快、更安全。

## <a name="key-features-of-http2"></a>HTTP/2 的主要功能

此列表显示了 HTTP/2 的一些主要功能和优势：

### <a name="binary-protocol"></a>二进制协议

请求/响应周期不再需要文本命令。 此活动简化并加速了命令的实现。 具体而言，分析数据的速度更快，使用的内存更少，网络延迟会因速度的明显相关改进而降低，同时还能提供更好的网络资源。

### <a name="streams"></a>流

流允许你在发送方和接收方之间创建长时间连接，可以通过这些连接以异步方式发送多个消息或帧。 多个流可以独立于单个 HTTP/2 连接进行操作。

### <a name="request-multiplexing-over-a-single-tcp-connection"></a>通过单个 TCP 连接请求多路复用

此功能是 HTTP/2 最重要的一项创新。 因为它允许对数据进行多次并行请求，所以现在可以从一台服务器同时下载 web 文件。 网站加载速度更快，并减少了优化需求。 线路外 (住) 阻止的情况下，如果已准备就绪的响应，则必须等待发送 (，直至之前的请求完成为止，即使在 TCP 传输级别) ，仍可进行阻止。

### <a name="nettcp-like-performance-cross-platform"></a>Net.tcp 类似的性能，跨平台

从根本上讲，gRPC 和 HTTP/2 的组合为开发人员提供了与 WCF 的 Net.tcp 绑定相同的速度和效率，在某些情况下，其速度更快，效率更高。 但与 Net.tcp 不同，HTTP/2 上的 gRPC 不会被限制为 .NET 应用程序。

>[!div class="step-by-step"]
>[上一页](interface-definition-language.md)
>[下一页](why-grpc.md)
