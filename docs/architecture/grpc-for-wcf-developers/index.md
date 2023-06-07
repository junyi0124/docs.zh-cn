---
title: 适用于 WCF 开发人员的 ASP.NET Core gRPC - 适用于 WCF 开发人员的 gRPC
description: 在适用于 WCF 开发人员的 ASP.NET Core 3.0 中构建 gRPC 服务的简介
ms.date: 09/02/2019
ms.openlocfilehash: c9cc5ef9c06d5262fb85850f8a3b178d46e5c6fa
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95689272"
---
# <a name="aspnet-core-grpc-for-wcf-developers"></a>适用于 WCF 开发人员的 ASP.NET Core gRPC

![封面图像](./media/cover.png)

发布者

Microsoft 开发人员部门、.NET 和 Visual Studio 产品团队

Microsoft Corporation 的一个部门

One Microsoft Way

Redmond, Washington 98052-6399

版权所有 © 2019 Microsoft Corporation

保留所有权利。 未经发布者书面许可，不得以任何形式或任何方式复制或传播本书中的任何内容。

本书“按原样”提供，表达作者的观点和看法。 本书中表达的观点、看法和信息（包括 URL 和其他 Internet 网站引用）如有更改，恕不另行通知。

本书中提及的一些示例仅用于说明，纯属虚构。 不存在任何实际关联或联系，请勿妄加推断。

Microsoft 和 <https://www.microsoft.com> 上“商标”网页列出的商标是 Microsoft 集团公司的商标。

Docker 的鲸鱼徽标是 Docker Inc. 的注册商标经许可方可使用。

所有其他标记和徽标均为其各自所有者的财产。

作者：

> **Mark Rendle** -首席技术官 - [视觉对象重新编码](https://visualrecode.com)
>
> **Miranda Steiner** - 技术作者

编辑器：

> **Maira Wenzel** - 资深内容开发人员 - Microsoft

## <a name="introduction"></a>介绍

gRPC 是用于构建网络服务和分布式应用程序的新式框架。 假设结合了 SOAP 跨平台互操作性的 Windows Communication Foundation (WCF) NetTCP 绑定的性能。 gRPC 建立在 HTTP/2 和 Protobuf 消息编码协议的基础上，在应用程序和服务之间提供高性能、低带宽的通信。 它支持跨最常用的编程语言和平台（包括 .NET、Java、Python、Node.js、Go、C++）生成服务器和客户端代码。 凭借对 ASP.NET Core 3.0 中的 gRPC 的卓越支持，外加适用于 .NET Framework 4.x 的现有 gRPC 工具和库，WCF 是希望在组织中采用 .NET Core 的开发团队的一种很好的替代方法。

## <a name="who-should-use-this-guide"></a>本指南的目标读者

本指南面向正在使用 .NET Framework 或 .NET Core、以前使用过 WCF 并且正在寻求将应用程序迁移到 .NET Core 3.0 及更高版本的新式 RPC 环境中的开发人员。 更常见的情况是，如果你要升级或考虑升级到 .NET Core 3.0，并且想要使用内置的 gRPC 工具，则本指南非常有用。

## <a name="how-you-can-use-this-guide"></a>如何使用本指南

这是在 ASP.NET Core 3.0 中构建 gRPC 服务的简短介绍，还将 WCF 作为类似平台进行了介绍。 它解释了 gRPC 的原理，将每个概念与 WCF 的等效功能相关联，并提供了有关将现有 WCF 应用程序迁移到 gRPC 的指导。 对于具有 WCF 经验并想要学习通过 gRPC 构建新服务的开发人员来说，它也很有用。 你可使用示例应用程序作为自己项目的模板或参考，并可随意复制和重复使用本书或其示例中的代码。

请将本指南转发到团队中，这有助于确保对这些注意事项和机会的共同理解。 通过让每个人都使用同一组术语和基础原则，帮助确保构建模式和做法的一致应用。

## <a name="references"></a>reference

- **gRPC 网站**
  <https://grpc.io>
- **为服务器应用选择 .NET Core 或 .NET Framework**
  <https://docs.microsoft.com/dotnet/standard/choosing-core-framework-server>

>[!div class="step-by-step"]
>[下一页](introduction.md)
