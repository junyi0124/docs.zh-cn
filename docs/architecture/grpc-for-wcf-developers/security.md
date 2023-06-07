---
title: GRPC 应用程序中的安全性-WCF 开发人员 gRPC
description: GRPC 中的呼叫和通道身份验证和授权的概述。
ms.date: 12/15/2020
ms.openlocfilehash: a5c16a4a4f7567e23bf4f9e3049fe116086daf12
ms.sourcegitcommit: 655f8a16c488567dfa696fc0b293b34d3c81e3df
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/06/2021
ms.locfileid: "97938086"
---
# <a name="security-in-grpc-applications"></a>gRPC 应用程序中的安全性

在任何实际情况下，确保应用程序和服务的安全至关重要。 安全性涵盖三个关键领域：

* 加密网络流量可防止恶意黑客拦截该流量。
* 对客户端和服务器进行身份验证，以建立标识和信任。
* 授权客户端控制对系统的访问并根据标识应用权限。

> [!NOTE]
> *身份验证* 涉及建立客户端或服务器的标识。 *授权* 与确定客户端是否有权访问资源或发出命令相关。

本章将介绍在 gRPC 中用于 ASP.NET Core 的身份验证和授权功能。 它还将通过 TLS 加密连接讨论网络安全。

## <a name="wcf-authentication-and-authorization"></a>WCF 身份验证和授权

在 Windows Communication Foundation (WCF) ，身份验证和授权的处理方式不同，具体取决于所使用的传输和绑定。 WCF 支持各种 WS \* 安全标准。 它还支持 windows 身份验证，适用于在 IIS 中运行的 HTTP 服务或 Windows 系统之间 Wcf-nettcp 的服务。

## <a name="grpc-authentication-and-authorization"></a>gRPC 身份验证和授权

gRPC 身份验证和授权适用于两个级别：

* 调用级身份验证/授权通常通过在调用时应用于元数据的令牌进行处理。
* 通道级身份验证使用在连接级别应用的客户端证书。 它还可以包含调用级别的身份验证/授权凭据，以自动将其应用到通道上的每个调用。

你可以使用其中一个或两个机制来帮助保护你的服务。

GRPC 的 ASP.NET Core 实现通过大多数标准 ASP.NET Core 机制支持身份验证和授权：

- 调用身份验证
  - Azure Active Directory
  - IdentityServer
  - JWT 持有者令牌
  - OAuth 2.0
  - OpenID Connect
  - WS-Federation
- 通道身份验证
  - 客户端证书

呼叫身份验证方法都是基于 *令牌* 的。 唯一的区别在于如何生成令牌和用于验证 ASP.NET Core 服务中的令牌的库。

有关详细信息，请参阅 [身份验证和授权](/aspnet/core/grpc/authn-and-authz) 一文。

> [!NOTE]
> 当你在 TLS 加密的 HTTP/2 连接上使用 gRPC 时，客户端和服务器之间的所有流量都将加密，即使你不使用通道级身份验证。

本章介绍如何将调用凭据和通道凭据应用于 gRPC 服务。 它还将演示如何使用 .NET gRPC 客户端提供的凭据对服务进行身份验证。

>[!div class="step-by-step"]
>[上一页](client-libraries.md)
>[下一页](call-credentials.md)
