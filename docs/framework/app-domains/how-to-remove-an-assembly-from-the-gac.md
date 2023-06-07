---
title: 如何：从全局程序集缓存中删除程序集
description: 了解如何使用全局程序集缓存工具 (Gacutil.exe) 或 Windows Installer 从 .NET 的全局程序集缓存中删除程序集。
ms.date: 03/30/2017
helpviewer_keywords:
- assemblies [.NET Framework], global assembly cache
- Gacutil.exe
- global assembly cache, removing assemblies
- strong-named assemblies, global assembly cache
- removing assemblies from global assembly cache
- deleting assemblies in global assembly cache
- Global Assembly Cache tool
- GAC (global assembly cache), removing assemblies
ms.assetid: acdcc588-b458-436d-876c-726de68244c1
ms.openlocfilehash: e3a596ea6029ded190c33032e96b601de9d4012d
ms.sourcegitcommit: 1c37a894c923bea021a3cc38ce7cba946357bbe1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/19/2020
ms.locfileid: "85104766"
---
# <a name="how-to-remove-an-assembly-from-the-global-assembly-cache"></a>如何：从全局程序集缓存中删除程序集

有两种方法可以从全局程序集缓存 (GAC) 中移除程序集：

- 通过使用[全局程序集缓存工具 (Gacutil.exe)](../tools/gacutil-exe-gac-tool.md)。 可以使用此选项卸载开发和测试期间已放置在 GAC 中的程序集。

- 通过使用 [Windows Installer](/windows/desktop/Msi/windows-installer-portal)。 应使用此选项在测试安装包期间卸载程序集，并将其用于生产系统。

## <a name="removing-an-assembly-with-gacutilexe"></a>使用 Gacutil.exe 删除程序集

在命令提示符处，键入下列命令：

gacutil –u \<*assembly name*>

在此命令中，“assembly name”是要从全局程序集缓存中删除的程序集的名称。

> [!WARNING]
> 不应使用 Gacutil.exe 来移除生产系统上的程序集，因为一些应用程序可能仍需要该程序集。 应改为使用 Windows Installer，它保留了它在 GAC 中安装的每个程序集的引用计数。

以下示例从全局程序集缓存中删除名为 `hello.dll` 的程序集：

```console
gacutil -u hello
```

## <a name="removing-an-assembly-with-windows-installer"></a>使用 Windows Installer 移除程序集

在“控制面板”的“程序和功能”应用中，选择要卸载的应用 。 如果安装包将程序集放入 GAC，在没有其他应用程序使用这些程序集的情况下，Windows Installer 将移除它们。

> [!NOTE]
> Windows Installer 保留了安装在 GAC 中的程序集的引用计数。 仅当程序集的引用计数为零时才可将其从 GAC 移除，计数为零时意味着它没有被任何 Windows Installer 包安装的应用程序所使用。

## <a name="see-also"></a>请参阅

- [使用程序集和全局程序集缓存](working-with-assemblies-and-the-gac.md)
- [如何：将程序集安装到全局程序集缓存](install-assembly-into-gac.md)
- [Gacutil.exe（全局程序集缓存工具）](../tools/gacutil-exe-gac-tool.md)
