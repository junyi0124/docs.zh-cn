---
title: SYSLIB0005 警告
description: 了解有关生成编译时警告 SYSLIB0005 的过时信息。
ms.topic: reference
ms.date: 10/20/2020
ms.openlocfilehash: 1263a4d327c735268f77ed56bdcea6a4cbed4bfa
ms.sourcegitcommit: e301979e3049ce412d19b094c60ed95b316a8f8c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/16/2020
ms.locfileid: "97596289"
---
# <a name="syslib0005-the-global-assembly-cache-gac-is-not-supported"></a>SYSLIB0005：不支持全局程序集缓存 (GAC)

.NET Core 和 .NET 5.0 及更高版本消除了 .NET Framework 中存在的全局程序集缓存 (GAC) 这一概念。 为帮助开发人员摒弃这些 API，从 .NET 5.0 开始，一些 GAC 相关的 API 标记为已过时。 使用这些 API 会在编译时生成警告 `SYSLIB0005`。

以下与 GAC 相关的 API 标记为已过时：

- <xref:System.Reflection.Assembly.GlobalAssemblyCache?displayProperty=nameWithType>

  库和应用不应使用 <xref:System.Reflection.Assembly.GlobalAssemblyCache> API 来确定运行时行为，因为它在 .NET Core 和 .NET 5+ 中始终返回 `false`。

## <a name="workarounds"></a>工作区

如果你的应用程序查询 <xref:System.Reflection.Assembly.GlobalAssemblyCache> 属性，请考虑删除该调用。 如果在运行时使用 <xref:System.Reflection.Assembly.GlobalAssemblyCache> 值在“GAC 中的程序集”流与“不在 GAC 中的程序集”流之间进行选择，请重新考虑流对于 .NET 5+ 应用程序是否仍然有意义。

[!INCLUDE [suppress-syslib-warning](../../../../includes/suppress-syslib-warning.md)]

## <a name="see-also"></a>请参阅

- [全局程序集缓存](../../../framework/app-domains/gac.md)
