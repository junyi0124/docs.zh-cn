---
title: .NET 入门
description: 创建 Hello World .NET 应用。
author: adegeo
ms.author: adegeo
ms.date: 09/29/2020
ms.custom: vs-dotnet
ms.openlocfilehash: 6ad2b96e668c52ee80b707e31a63eac2433f3c9e
ms.sourcegitcommit: a8a205034eeffc7c3e1bdd6f506a75b0f7099ebf
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/06/2020
ms.locfileid: "91756086"
---
# <a name="get-started-with-net"></a>.NET 入门

本文介绍如何创建和运行“Hello World!” .NET 应用。

如果不确定什么是 .NET，请从 [.NET 简介](introduction.md)开始。

## <a name="create-an-application"></a>创建应用程序

首先，在计算机上下载并安装 [.NET SDK](https://dotnet.microsoft.com/download/dotnet-core)。

然后，打开某一终端，如 PowerShell、命令提示符或 Bash  。 输入以下 `dotnet` 命令，创建并运行 C# 应用程序：

```dotnetcli
dotnet new console --output sample1
dotnet run --project sample1
```

可以看到以下输出：

```output
Hello World!
```

祝贺你！ 你现已创建了一个简单的 .NET 应用程序。

## <a name="next-steps"></a>后续步骤

按照[分步教程](../standard/get-started.md)或观看 YouTube 上的 [.NET 101 视频](https://www.youtube.com/playlist?list=PLdo4fOcmZ0oWoazjhXQzBKMrFuArxpW80)，开始开发 .NET 应用程序。
