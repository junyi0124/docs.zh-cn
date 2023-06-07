---
title: NETSDK1022：包含重复的项。
description: 如何根据默认包含项解决重复项消息。
author: marcpopMSFT
ms.topic: error-reference
ms.date: 10/9/2020
f1_keywords:
- NETSDK1022
ms.openlocfilehash: c2bdbbb5503e5e4f6e6826f95f5a2cb08c908ceb
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95717860"
---
# <a name="netsdk1022-duplicate-items-were-included"></a>NETSDK1022：包含重复的项。

本文适用于：✔️ .NET Core 2.1.100 SDK 及更高版本

从 Visual Studio 2017 / MSBuild 版本 15.3 开始，默认情况下 .NET SDK 会自动包括项目目录中的项。  这包括 `Compile` 和 `Content` 目标。  这应该能够很好地清理你的项目文件，并降低其中的复杂性。

通过将正确的属性设置为 false，可以还原到以前的行为。

`Compile` 项的示例：

```xml
<PropertyGroup>
    <EnableDefaultCompileItems>false</EnableDefaultCompileItems>
</PropertyGroup>
```
