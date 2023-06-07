---
title: 与非托管代码进行交互操作
description: 查看与非托管代码的互操作。 CLR 对客户端和服务器隐藏 .NET 组件对象模型与非托管代码的区别。
ms.date: 01/17/2018
helpviewer_keywords:
- unmanaged code, interoperation
- managed code, interoperation with unmanaged code
- .NET Framework, interoperation with unmanaged code
- unmanaged code
- interoperation with unmanaged code
- interoperation with unmanaged code, about interoperation
- components [.NET Framework], interoperation with unmanaged code
ms.assetid: ccb68ce7-b0e9-4ffb-839d-03b1cd2c1258
ms.openlocfilehash: 1cebd75907fd202715cb337593186d248107bdbb
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/01/2020
ms.locfileid: "85621868"
---
# <a name="interoperating-with-unmanaged-code"></a>与非托管代码进行交互操作

.NET Framework 提升与 COM 组件、COM+ 服务、外部类型库和许多操作系统服务的交互。 托管和非托管对象模型之间的数据类型、方法签名和错误处理机制有所不同。 要简化 .NET Framework 组件和非托管代码之间的互操作并简化迁移路径，公共语言运行时需对客户端和服务器隐藏这些对象模型中的差异。

在运行时控制下执行的代码称为托管代码。 反之，在运行时以外运行的代码称为非托管代码。 COM 组件、ActiveX 接口和 Windows API 函数都是非托管代码的示例。

## <a name="in-this-section"></a>本节内容

[向 .NET Framework 公开 COM 组件](exposing-com-components.md)  
描述如何使用 .NET Framework 应用程序的 COM 组件。

[向 COM 公开 .NET Framework 组件](exposing-dotnet-components-to-com.md)  
描述如何使用 COM 应用程序的 .NET Framework 组件。

[使用非托管 DLL 函数](consuming-unmanaged-dll-functions.md)  
描述如何使用平台调用调用非托管 DLL 函数。

[互操作封送处理](interop-marshaling.md)  
描述 COM 互操作和平台调用的封送处理。

[如何：映射 HRESULT 和异常](how-to-map-hresults-and-exceptions.md)  
描述异常和 HRESULT 之间的映射。

[类型等效性和嵌入的互操作类型](type-equivalence-and-embedded-interop-types.md)  
介绍如何在程序集中嵌入 COM 类型的类型信息，以及公共语言运行时如何确定嵌入的 COM 类型的等效性。

[如何：使用 Tlbimp.exe 生成主互操作程序集](how-to-generate-primary-interop-assemblies-using-tlbimp-exe.md)  
介绍如何使用 Tlbimp.exe （类型库导入程序）生成主要互操作程序集。

[如何：注册主互操作程序集](how-to-register-primary-interop-assemblies.md)  
介绍如何注册主互操作程序集，然后才能在项目中引用它们。

[免注册 COM 互操作](registration-free-com-interop.md)  
介绍 COM 互操作如何在不使用 Windows 注册表的情况下激活组件。

[如何：配置基于 .NET Framework 的 COM 组件以进行免注册激活](configure-net-framework-based-com-components-for-reg.md)  
介绍如何创建应用程序清单以及如何创建并嵌入组件清单。

## <a name="related-sections"></a>相关章节

[COM 包装](../../standard/native-interop/com-wrappers.md)  
介绍 COM 互操作提供的包装器。
