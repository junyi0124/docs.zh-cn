---
title: 中断性变更：已从 .NET 运行时中删除 WinHttpHandler
description: 了解 .NET 5.0 中的以下中断性变更：WinHttpHandler 已从 .NET 运行时中删除。
ms.date: 10/18/2020
ms.openlocfilehash: f8b9910ade8d459133791a23704d624a91cc5dff
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95759119"
---
# <a name="winhttphandler-removed-from-net-runtime"></a>已从 .NET 运行时中删除 WinHttpHandler

`WinHttpHandler` 类已从 System.Net.Http.dll 程序集删除。 它现在仅作为带外 (OOB) [NuGet 包](https://www.nuget.org/packages/System.Net.Http.WinHttpHandler/)提供。

## <a name="version-introduced"></a>引入的版本

5.0

## <a name="change-description"></a>更改描述

在以前的 .NET 版本中，<xref:System.Net.Http.WinHttpHandler> 类作为核心 .NET 库的一部分提供。 从 .NET 5.0 开始，<xref:System.Net.Http.WinHttpHandler> 类仅作为单独安装的 [NuGet 包](https://www.nuget.org/packages/System.Net.Http.WinHttpHandler/)提供。

## <a name="recommended-action"></a>建议操作

安装 [System.Net.Http.WinHttpHandler NuGet 包](https://www.nuget.org/packages/System.Net.Http.WinHttpHandler/)。 或者，如果不需要特定于 WinHTTP 的功能，请改用 <xref:System.Net.Http.SocketsHttpHandler>。

## <a name="affected-apis"></a>受影响的 API

- <xref:System.Net.Http.WinHttpHandler?displayProperty=fullName>

<!--

### Affected APIs

- `T:System.Net.Http.WinHttpHandler`

### Category

Networking

-->
