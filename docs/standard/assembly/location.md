---
title: 程序集位置
description: .NET 程序集的位置决定了 CLR 查找它的方式，以及它是否可以与其他程序集共享。
ms.date: 08/20/2019
helpviewer_keywords:
- locating assemblies
- assemblies [.NET], location
ms.assetid: 9f1f41a7-2954-49d3-a2c0-62b6ef4d40ab
ms.openlocfilehash: 1fa1c486c0cddce4ddcfae7f2df27e2e85c88e66
ms.sourcegitcommit: 279fb6e8d515df51676528a7424a1df2f0917116
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2020
ms.locfileid: "92687602"
---
# <a name="assembly-location"></a>程序集位置

程序集的位置决定公共语言运行时是否可以在引用该程序集时找到它，也可以决定是否可与其他程序集共享该程序集。 可以在以下位置部署程序集：

- 应用程序的目录或子目录。

     这是部署程序集最常用的位置。 应用程序根目录的子目录可以基于语言或区域性。 如果程序集具有 culture 特性中的信息，则它必须位于带有该区域性名称的应用程序目录下的子目录中。

- 全局程序集缓存。

     这是安装于公共语言运行时安装位置的计算机范围内的代码缓存。 大多数情况下，如果要与多个应用程序共享程序集，应将程序集部署到全局程序集缓存中。

- 在 HTTP 服务器上。

     部署在 HTTP 服务器上的程序集必须具有强名称，请在应用程序配置文件的基本代码节中指向此程序集。

## <a name="see-also"></a>请参阅

- [创建程序集](create.md)
- [全局程序集缓存](../../framework/app-domains/gac.md)
- [运行时如何定位程序集](../../framework/deployment/how-the-runtime-locates-assemblies.md)
