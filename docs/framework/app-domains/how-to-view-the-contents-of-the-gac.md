---
title: 如何：查看全局程序集缓存的内容
description: 了解如何使用全局程序集缓存 (GAC) 工具 (gacutil.exe) 查看 .NET 中的全局程序集缓存的内容。
ms.date: 03/30/2017
helpviewer_keywords:
- assemblies [.NET Framework], global assembly cache
- global assembly cache, viewing contents
- viewing assemblies in global assembly cache
- Gacutil.exe
- strong-named assemblies, global assembly cache
- GAC (global assembly cache), viewing contents
- list of assemblies in global assembly cache
- Global Assembly Cache tool
ms.assetid: c5f786a0-969b-4f14-9f02-e77c3384d9af
ms.openlocfilehash: 8b81f78f4ea28b3b9fca374029fe49f809826d8e
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90558558"
---
# <a name="how-to-view-the-contents-of-the-global-assembly-cache"></a>如何：查看全局程序集缓存的内容

使用[全局程序集缓存工具 (Gacutil.exe)](../tools/gacutil-exe-gac-tool.md) 可查看全局程序集缓存 (GAC) 的内容。

## <a name="view-the-assemblies-in-the-gac"></a>查看 GAC 中的程序集

要查看全局程序集缓存中的程序集列表，请打开 [Visual Studio 适用的开发人员命令提示](../tools/developer-command-prompt-for-vs.md)，然后输入以下命令：

```shell
gacutil -l
```

\- 或 -

```shell
gacutil /l
```

> [!NOTE]
> 在 .NET Framework 的早期版本中，可通过 [Shfusion.dll](/previous-versions/dotnet/netframework-4.0/34149zk3(v=vs.100)) Windows shell 扩展在文件资源管理器中查看全局程序集缓存。 从 .NET Framework 4 开始，Shfusion.dll 已淘汰。

## <a name="see-also"></a>请参阅

- [使用程序集和全局程序集缓存](working-with-assemblies-and-the-gac.md)
- [Gacutil.exe（全局程序集缓存工具）](../tools/gacutil-exe-gac-tool.md)
