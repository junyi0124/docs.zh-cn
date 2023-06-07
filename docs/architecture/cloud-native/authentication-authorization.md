---
title: 云本机应用中的身份验证和授权
description: 构建适用于 Azure 的云本机 .NET 应用 |云本机应用中的身份验证和授权
ms.date: 05/13/2020
ms.openlocfilehash: bbd2df110dd7a7dc7363e9c07d87f1fa12f4e464
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91161134"
---
# <a name="authentication-and-authorization-in-cloud-native-apps"></a>云本机应用中的身份验证和授权

*身份验证* 是确定安全主体的标识的过程。 *授权* 是指向经过身份验证的主体授予执行操作或访问资源的权限。 有时，身份验证将缩短为 `AuthN` ，授权会缩短到 `AuthZ` 。 云本机应用程序需要依赖开放式 HTTP 协议对安全主体进行身份验证，因为客户端和应用程序都可以在任何平台或设备上的任何位置运行。 唯一的常见因素是 HTTP。

许多组织仍依赖本地身份验证服务，如 Active Directory 联合身份验证服务 (ADFS) 。 虽然这种方法在传统上能够满足本地身份验证需求，但云本机应用程序也受益于专门针对云设计的系统。 最近的2019英国国家网络安全中心 (NCSC) 咨询表明 "使用 Azure AD 作为其主要身份验证源的组织实际上会降低他们与 ADFS 相比的风险。" [此分析](https://oxfordcomputergroup.com/resources/o365-security-native-cloud-authentication/)概述的一些原因包括：

- 访问全套 Microsoft 凭据保护技术。
- 大多数组织已经依赖于 Azure AD 一定程度上。
- NTLM 哈希的双重哈希可确保泄露不会允许在本地 Active Directory 中使用的凭据。

## <a name="references"></a>参考

- [身份验证基础知识](/azure/active-directory/develop/authentication-scenarios)
- [访问令牌和声明](/azure/active-directory/develop/access-tokens)
- [可能需要 ditch 本地身份验证服务](https://oxfordcomputergroup.com/resources/o365-security-native-cloud-authentication/)

>[!div class="step-by-step"]
>[上一页](identity.md)
>[下一页](azure-active-directory.md)
