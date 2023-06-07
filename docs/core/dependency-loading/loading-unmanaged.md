---
title: 非托管库加载算法 - .NET Core
description: .NET Core 中非托管程序集加载算法的详细说明
ms.date: 10/09/2019
author: sdmaclea
ms.author: stmaclea
ms.openlocfilehash: c651aa6e0f37a968e6f8b26d1909def6fa488ccd
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/14/2020
ms.locfileid: "72303694"
---
# <a name="unmanaged-native-library-loading-algorithm"></a>非托管（本机）库加载算法

非托管库与涉及不同阶段的算法一起定位并加载。

以下算法描述如何通过 `PInvoke` 加载本机库。

## <a name="pinvoke-load-library-algorithm"></a>`PInvoke` 加载库算法

`PInvoke` 在尝试加载非托管程序集时使用以下算法：

1. 确定 `active` <xref:System.Runtime.Loader.AssemblyLoadContext>。 对于非托管加载库，`active` AssemblyLoadContext 是具有定义 `PInvoke` 的程序集的算法。

2. 对于 `active` <xref:System.Runtime.Loader.AssemblyLoadContext>，尝试通过以下方式按优先级排序来查找程序集：
    * 检查其缓存。

    * 调用由 <xref:System.Runtime.InteropServices.NativeLibrary.SetDllImportResolver(System.Reflection.Assembly,System.Runtime.InteropServices.DllImportResolver)?displayProperty=nameWithType> 函数设置的当前 <xref:System.Runtime.InteropServices.DllImportResolver?displayProperty=nameWithType> 委托。

    * 对 `active` AssemblyLoadContext 调用 <xref:System.Runtime.Loader.AssemblyLoadContext.LoadUnmanagedDll%2A?displayProperty=nameWithType> 函数。

    * 检查 <xref:System.AppDomain> 实例的缓存并运行[非托管（本机）库探测](default-probing.md#unmanaged-native-library-probing)逻辑。

    * 引发 `active` AssemblyLoadContext 的 <xref:System.Runtime.Loader.AssemblyLoadContext.ResolvingUnmanagedDll?displayProperty=nameWithType> 事件。
