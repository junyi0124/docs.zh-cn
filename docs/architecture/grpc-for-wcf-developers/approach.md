---
title: GRPC 如何为 WCF 开发人员提供 RPC gRPC
description: 比较 WCF 的主要功能和 gRPC。
ms.date: 09/02/2019
ms.openlocfilehash: b924d2f20b54de2d6476a0b27626d5dd7fd0986f
ms.sourcegitcommit: 5fb5b6520b06d7f5e6131ec2ad854da302a28f2e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/03/2019
ms.locfileid: "74711480"
---
# <a name="how-grpc-approaches-rpc"></a>gRPC 如何处理 RPC

Windows Communication Foundation （WCF）和 gRPC 都是*远程过程调用*（RPC）模式的两种实现。 此模式旨在调用在不同的计算机上或在不同进程中运行的服务，如客户端应用程序中的方法调用。 尽管 WCF 和 gRPC 的目标是相同的，但实现的详细信息是完全不同的。

下表设置了 WCF 的主要功能与 gRPC 的关系，以及在何处可以找到更详细的说明。

| 特征 | WCF | gRPC |
| -------- | --- | ---- |
| 目标 | 独立于网络实现的业务代码。 | 将业务代码与接口定义和网络实现分离。 |
| 定义服务和消息（章节3-4）  | 服务协定、操作协定和数据协定。 | 使用 proto 文件声明服务和消息。 |
| 语言（章节3-5） | 用或 Visual Basic C#编写的协定。 | 协议缓冲区语言。 |
| 线路格式（第3章） | 可配置，包括 SOAP/XML、纯 XML、JSON 和 .NET Binary。 | 协议缓冲区二进制格式（尽管可以使用其他格式）。
| 互操作性（第4章） | 使用 SOAP over HTTP 时。 | 官方支持： .NET、Java、Python、JavaScript、C/C++、中转、Rust、Ruby、Swift、DART、PHP。 来自社区的其他语言的非正式支持。 |
| 网络（第4章） | 在运行时配置。 在 Wcf-nettcp、HTTP 和 MSMQ 之间切换。 | HTTP/2，当前通过 TCP ASP.NET Core gRPC。 |
| 方法（第4章） | 基类中序列化、反序列化和网络代码的运行时生成。 | 生成时生成序列化、反序列化和基类中的网络代码。 |
| 安全性（第6章） | 身份验证、WS 安全和消息加密。 | 凭据、ASP.NET Core 安全性、TLS 网络。 |

>[!div class="step-by-step"]
>[上一页](grpc-overview.md)
>[下一页](interface-definition-language.md)
