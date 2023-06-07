---
title: 打包用于 COM 的 .NET Framework 程序集
description: 打包用于 COM 的 .NET 程序集。 收集 COM 应用程序可使用的类型列表、版本控制和部署说明以及类型库。
ms.date: 03/30/2017
helpviewer_keywords:
- exposing .NET Framework components to COM
- COM interop, packaging assemblies
- packaging assemblies for COM
- Tlbexp.exe
- TypeLibConverter class
- .NET Services Installation tool
- Assembly Registration tool
- Type Library Exporter
- Reqsvcs.exe
- interoperation with unmanaged code, exposing .NET Framework components
- interoperation with unmanaged code, packaging assemblies
- COM interop, exposing COM components
- Reqasm.exe
ms.assetid: 39dc55aa-f2a1-4093-87bb-f1c0edb6e761
ms.openlocfilehash: 5fde7f7f00aadf4d941d4ffe522453970b67e9e2
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90554125"
---
# <a name="packaging-a-net-framework-assembly-for-com"></a>打包用于 COM 的 .NET Framework 程序集

COM 开发人员可从其计划纳入应用程序的托管类型相关信息中受益：

- COM 应用程序可使用的类型列表

  某些托管类型对 COM 不可见；某些可见但不可创建；某些既可见又可创建。 程序集可以包括不可见、可见、不可创建和可创建类型的任何组合。 为实现完整性，请确定打算公开到 COM 的程序集中的类型，尤其是当这些类型是公开到 .NET framework 中类型的子集时。

  有关其他信息，请参阅[为互操作限定 .NET 类型](../../standard/native-interop/qualify-net-types-for-interoperation.md)。

- 版本控制说明

  实现类接口（COM 互操作生成的接口）的托管类受版本控制限制。

  有关如何使用类接口的指南，请参阅[类接口简介](../../standard/native-interop/com-callable-wrapper.md#introducing-the-class-interface)。

- 部署说明

  发布者签名的具有强名称的程序集可安装到全局程序集缓存。 未签名程序集必须作为专用程序集安装在用户计算机上。

  有关其他信息，请参阅[程序集安全注意事项](../../standard/assembly/security-considerations.md)。

- 类型库所含内容

  大多数类型在由 COM 应用程序使用时都需要类型库。 可生成类型库或使 COM 开发人员执行此任务。 Windows SDK 为生成类型库提供以下选项：

  - [类型库导出程序](#cpconpackagingassemblyforcomanchor1)

  - [TypeLibConverter 类](#cpconpackagingassemblyforcomanchor2)

  - [程序集注册工具](#cpconpackagingassemblyforcomanchor3)

  - [.NET 服务安装工具](#cpconpackagingassemblyforcomanchor4)

  无论选择的机制如何，生成的类型库中仅包含所提供程序集中定义的公共类型。

有关说明，请参阅[如何：将类型库作为 Win32 资源嵌入基于 .NET 的应用程序](/previous-versions/dotnet/netframework-4.0/ww9a897z(v=vs.100))。

<a name="cpconpackagingassemblyforcomanchor1"></a>

## <a name="type-library-exporter"></a>类型库导出程序

[类型库导出程序 (Tlbexp.exe)](../tools/tlbexp-exe-type-library-exporter.md) 是命令行工具，可将程序集中包含的类和接口转换为 COM 类型库。 一旦类的类型信息可用，COM 客户端就可创建 .NET 类的实例，并调用实例的方法，就像它是 COM 对象一样。 Tlbexp.exe 一次转换整个程序集。 不能使用 Tlbexp.exe 生成程序集中定义的类型子集的类型信息。

<a name="cpconpackagingassemblyforcomanchor2"></a>

## <a name="typelibconverter-class"></a>TypeLibConverter 类

位于 System.Runtime.Interop 命名空间中的 <xref:System.Runtime.InteropServices.TypeLibConverter> 类将程序集中包含的类和接口转换为 COM 类型库。 此 API 生成与类型库导出程序相同的类型信息，如上节所述。

TypeLibConverter 类实现 <xref:System.Runtime.InteropServices.ITypeLibConverter>。

<a name="cpconpackagingassemblyforcomanchor3"></a>

## <a name="assembly-registration-tool"></a>程序集注册工具

[程序集注册工具 (Regasm.exe)](../tools/regasm-exe-assembly-registration-tool.md) 可在应用 /tlb: 选项时生成并注册类型库。 COM 客户端要求在 Windows 注册表中安装类型库。 如果不使用此选项，Regasm.exe 仅在程序集而非类型库中注册类型。 在程序集中注册类型和注册类型库是不同的活动。

<a name="cpconpackagingassemblyforcomanchor4"></a>

## <a name="net-services-installation-tool"></a>.NET 服务安装工具

[.NET 服务安装工具 (Regsvcs.exe)](../tools/regsvcs-exe-net-services-installation-tool.md) 将托管类添加到 Windows 2000 组件服务，并在单个工具中组合多个任务。 除加载和注册程序集外，Regsvcs.exe 还可将类型库生成、注册和安装到现有 COM+ 1.0 应用程序。

## <a name="see-also"></a>请参阅

- <xref:System.Runtime.InteropServices.TypeLibConverter>
- <xref:System.Runtime.InteropServices.ITypeLibConverter>
- [向 COM 公开 .NET Framework 组件](exposing-dotnet-components-to-com.md)
- [为互操作限定 .NET 类型](../../standard/native-interop/qualify-net-types-for-interoperation.md)
- [类接口简介](../../standard/native-interop/com-callable-wrapper.md#introducing-the-class-interface)
- [程序集安全注意事项](../../standard/assembly/security-considerations.md)
- [Tlbexp.exe（类型库导出程序）](../tools/tlbexp-exe-type-library-exporter.md)
- [向 COM 注册程序集](registering-assemblies-with-com.md)
- [如何：将类型库作为 Win32 资源嵌入应用程序](/previous-versions/dotnet/netframework-4.0/ww9a897z(v=vs.100))
