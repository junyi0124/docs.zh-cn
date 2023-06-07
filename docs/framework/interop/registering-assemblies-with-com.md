---
title: 向 COM 注册程序集
description: 使用程序集注册工具 (Regasm.exe) 向 COM 注册或取消注册程序集，这会将关于类的信息添加到系统注册表。
ms.date: 03/30/2017
helpviewer_keywords:
- COM interop, registering assemblies
- unregistering assemblies
- interoperation with unmanaged code, registering assemblies
- registering assemblies
ms.assetid: 87925795-a3ae-4833-b138-125413478551
ms.openlocfilehash: 525e3724aec82a74f5b0339296808b41f30d0ddc
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96266371"
---
# <a name="registering-assemblies-with-com"></a>向 COM 注册程序集

可运行名为[程序集注册工具 (Regasm.exe)](../tools/regasm-exe-assembly-registration-tool.md) 的命令行工具注册或取消注册与 COM 一起使用的程序集。 Regasm.exe 将关于类的信息添加到系统注册表，因此 COM 客户端可以透明地使用 .NET Framework 类。 <xref:System.Runtime.InteropServices.RegistrationServices> 类提供等效功能。  
  
 托管组件从 COM 客户端激活前必须在 Windows 注册表中注册。 下表显示 Regasm.exe 通常添加到 Windows 注册表的项。 （000000 表示实际的 GUID 值。）  
  
|GUID|描述|注册表项|  
|----------|-----------------|------------------|  
|CLSID|类标识符|HKEY_CLASSES_ROOT\CLSID\\{000…000}|  
|IID|接口标识符|HKEY_CLASSES_ROOT\Interface\\{000…000}|  
|LIBID|库标识符|HKEY_CLASSES_ROOT\TypeLib\\{000…000}|  
|ProgID|编程标识符|HKEY_CLASSES_ROOT\000…000|  
  
 HKCR\CLSID\\{0000…0000} 项下，默认值设置为类的 ProgID，并添加了两个新的命名值：类和程序集。 运行时从注册表读取程序集值并传递给运行时程序集解析程序。 程序集解析程序尝试基于程序集信息（如名称和版本号）定位程序集。 为了让程序集解析程序找到程序集，程序集必须位于以下位置中：  
  
- 全局程序集缓存（必须为强名称程序集）。  
  
- 应用程序目录中。 从应用程序路径加载的程序集只能从应用程序访问。  
  
- 沿 /codebase 选项指定的文件路径到 Regasm.exe。  
  
 Regasm.exe 也会在 HKCR\CLSID\\{0000…0000} 项下创建 InProcServer32 项。 项的默认值设置为初始化公共语言运行时 (Mscoree.dll) 的 DLL 的名称。  
  
## <a name="examining-registry-entries"></a>检查注册表项  

 COM 互操作提供标准的类工厂实现以创建任意 .NET Framework 类的实例。 客户端可以调用托管 DLL 上的“DllGetClassObject”以获取类工厂并创建对象，与使用任何其他 COM 组件相同。  
  
 关于 `InprocServer32` 子项, 出现在传统 COM 类型库中的对 Mscoree.dll 的引用表示公共语言运行时创建托管对象。  
  
## <a name="see-also"></a>请参阅

- [向 COM 公开 .NET Framework 组件](exposing-dotnet-components-to-com.md)
- [如何：从 COM 引用 .NET 类型](how-to-reference-net-types-from-com.md)
- [调用 .NET 对象](/previous-versions/dotnet/netframework-4.0/8hw8h46b(v=vs.100))
- [为 COM 访问部署应用程序](/previous-versions/dotnet/netframework-4.0/c2850st8(v=vs.100))
