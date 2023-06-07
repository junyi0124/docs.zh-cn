---
title: NETSDK1073：未识别 FrameworkReference
description: 如何解决找不到 FrameworkReference 的问题。
author: marcpopMSFT
ms.topic: error-reference
ms.date: 10/9/2020
f1_keywords:
- NETSDK1073
ms.openlocfilehash: 2b2dbf2a0d3e13dca4fe634529b7951f2333ce28
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95713023"
---
# <a name="netsdk1073-the-frameworkreference-was-not-recognized"></a>NETSDK1073：未识别 FrameworkReference

本文适用于：✔️ .NET Core 2.1.100 SDK 及更高版本

此错误通常表示存在 SDK 无法找到的特定 FrameworkReference 的版本。 尝试删除 obj 和 bin 文件夹，并运行 `dotnet restore` 以重新下载最新的目标包 。

或者，由于安装时可能存在问题，因此请确保使用最新版本的 .NET 和 Visual Studio
