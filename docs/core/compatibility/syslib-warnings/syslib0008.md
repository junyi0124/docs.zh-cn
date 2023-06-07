---
title: SYSLIB0008 警告
description: 了解有关生成编译时警告 SYSLIB0008 的过时信息。
ms.topic: reference
ms.date: 10/20/2020
ms.openlocfilehash: 2b573f4b28cdf79107395f5cb38a4226d0ccc11e
ms.sourcegitcommit: e301979e3049ce412d19b094c60ed95b316a8f8c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/16/2020
ms.locfileid: "97596271"
---
# <a name="syslib0008-createpdbgenerator-is-not-supported"></a>SYSLIB0008：不支持 CreatePdbGenerator

从 .NET 5.0 开始，<xref:System.Runtime.CompilerServices.DebugInfoGenerator.CreatePdbGenerator?displayProperty=nameWithType> API 标记为已过时。 使用此 API 会在编译时生成警告 `SYSLIB0008`。

## <a name="suppress-the-warning"></a>禁止显示警告

如果无法更改代码，可以通过 `#pragma` 指令或 `<NoWarn>` 项目设置来禁止显示警告。 有关示例，请参阅[禁止显示警告](../syslib-obsoletions.md#suppress-warnings)。
