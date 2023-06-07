---
title: SYSLIB0012 警告
description: 了解有关生成编译时警告 SYSLIB0012 的过时信息。
ms.topic: reference
ms.date: 10/20/2020
ms.openlocfilehash: 9b3fa94029efb2343e6dbbeb3ae36195f0956523
ms.sourcegitcommit: e301979e3049ce412d19b094c60ed95b316a8f8c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/16/2020
ms.locfileid: "97596270"
---
# <a name="syslib0012-assemblycodebase-and-assemblyescapedcodebase-are-obsolete"></a>SYSLIB0012：Assembly.CodeBase 和 Assembly.EscapedCodeBase 已过时

从 .NET 5.0 开始，以下 API 标记为已过时。 在代码中使用这些 API 会在编译时生成警告 `SYSLIB0012`。

- <xref:System.Reflection.Assembly.CodeBase?displayProperty=nameWithType>
- <xref:System.Reflection.Assembly.EscapedCodeBase?displayProperty=nameWithType>

## <a name="workarounds"></a>工作区

请改用 <xref:System.Reflection.Assembly.Location?displayProperty=nameWithType>。

[!INCLUDE [suppress-syslib-warning](../../../../includes/suppress-syslib-warning.md)]
