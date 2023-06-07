---
title: 应用互操作特性
description: 本文汇总了 System.Runtime.InteropServices 命名空间的 COM 互操作特性，其中包括设计时和转换工具特性。
ms.date: 03/30/2017
helpviewer_keywords:
- design-time attributes
- .NET, exposing components to COM
- attributes [.NET], design-time functionality
- conversion-tool attributes
- attributes [.NET], interop-specific
- attributes [.NET], conversion-tool
- interoperation with unmanaged code, applying attributes
- interoperation with unmanaged code, exposing .NET components
- COM interop, exposing COM components
- COM interop, applying attributes
ms.assetid: b6014613-641c-4912-9e2f-83a99210a037
ms.openlocfilehash: 38632c5a1f462c3a7b537978fde81424916746da
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95706289"
---
# <a name="applying-interop-attributes"></a>应用互操作特性

<xref:System.Runtime.InteropServices> 命名空间提供三类特定于互操作的特性：在设计时由你应用的特性、在转换进程中由 COM 互操作工具和 API 应用的特性以及由你或 COM 互操作应用的特性。  
  
 如果不熟悉将特性应用到托管代码的任务，请参阅[利用特性扩展元数据](../attributes/index.md)。 如其他自定义特性一样，可以将特定于互操作的特性应用于类型、方法、属性、参数、字段和其他成员。  
  
## <a name="design-time-attributes"></a>设计时特性  

 可以使用设计时特性调整由 COM 互操作工具和 API 执行的转换进程的结果。 下表介绍了可以应用到托管源代码的特性。 有时，COM 互操作工具也可能应用此表中所述的特性。  
  
|特性|描述|  
|---------------|-----------------|  
|<xref:System.Runtime.InteropServices.AutomationProxyAttribute>|指定应使用自动化封送处理程序还是自定义代理和存根对类型进行封送处理。|  
|<xref:System.Runtime.InteropServices.ClassInterfaceAttribute>|控制为类生成的接口类型。|  
|<xref:System.Runtime.InteropServices.CoClassAttribute>|标识从类型库导入的原始组件类的 CLSID。<br /><br /> COM 互操作工具通常应用此特性。|  
|<xref:System.Runtime.InteropServices.ComImportAttribute>|指示组件类或接口定义是从 COM 类型库导入的。 运行时使用此标记来确定如何对类型进行激活和封送处理。 此特性禁止将类型导入回类型库。<br /><br /> COM 互操作工具通常应用此特性。|  
|<xref:System.Runtime.InteropServices.ComRegisterFunctionAttribute>|指定在从 COM 注册程序集以使用时应调用的方法，这样在注册过程中可以执行用户编写的代码。|  
|<xref:System.Runtime.InteropServices.ComSourceInterfacesAttribute>|标识类的事件源的接口。<br /><br /> COM 互操作工具可以应用此特性。|  
|<xref:System.Runtime.InteropServices.ComUnregisterFunctionAttribute>|指示在从 COM 取消注册程序集时应调用的方法，这样用户编写的代码可以在过程中执行。|  
|<xref:System.Runtime.InteropServices.ComVisibleAttribute>|当特性值为“false”时，将使类型对 COM 不可见。 此特性可以应用于单个类型或整个程序集，以控制 COM 的可见性。 默认情况下，所有托管的公共类型都是可见的；不需要使用此特性来使它们可见。|  
|<xref:System.Runtime.InteropServices.DispIdAttribute>|指定方法或字段的 COM 调度标识符 (DISPID)。 此特性包含适用于它所述的方法、字段或属性的 DISPID。<br /><br /> COM 互操作工具可以应用此特性。|
|<xref:System.Runtime.InteropServices.ComDefaultInterfaceAttribute>|指示在 .NET 中实现的默认 COM 类接口。<br /><br /> COM 互操作工具可以应用此特性。|
|<xref:System.Runtime.InteropServices.FieldOffsetAttribute>|指示当用于 StructLayoutAttribute 时类中每个字段的物理位置，并且将 LayoutKind 设置为 Explicit 。|  
|<xref:System.Runtime.InteropServices.GuidAttribute>|指定类、接口或整个类型库的全局唯一标识符 (GUID)。 传递到此特性的字符串必须是 System.Guid 类型可接受的构造函数参数的格式。<br /><br /> COM 互操作工具可以应用此特性。|  
|<xref:System.Runtime.InteropServices.IDispatchImplAttribute>|指示当向 COM 公开双重接口和调度时，公共语言运行时使用哪个 IDispatch 接口实现。|  
|<xref:System.Runtime.InteropServices.InAttribute>|指示数据应封送到调用方。 可用于特性参数。|  
|<xref:System.Runtime.InteropServices.InterfaceTypeAttribute>|控制如何向 COM 客户端公开托管接口（仅限于双重、IUnknown-derived 或 IDispatch）。<br /><br /> COM 互操作工具可以应用此特性。|  
|<xref:System.Runtime.InteropServices.LCIDConversionAttribute>|指示非托管的方法签名应具有 LCID 参数。<br /><br /> COM 互操作工具可以应用此特性。|  
|<xref:System.Runtime.InteropServices.MarshalAsAttribute>|指示应如何在托管和非托管代码之间封送处理字段或参数中的数据。 此特性始终是可选的，因为每个数据类型都具有默认的封送处理行为。<br /><br /> COM 互操作工具可以应用此特性。|  
|<xref:System.Runtime.InteropServices.OptionalAttribute>|指示参数是可选的。<br /><br /> COM 互操作工具可以应用此特性。|  
|<xref:System.Runtime.InteropServices.OutAttribute>|指示字段或参数中的数据必须从调用的对象被封送回其调用方。|  
|<xref:System.Runtime.InteropServices.PreserveSigAttribute>|取消一般在互操作调用过程中发生的 HRESULT 或 retval 签名转换。 特性会影响封送处理以及类型库导出。<br /><br /> COM 互操作工具可以应用此特性。|  
|<xref:System.Runtime.InteropServices.ProgIdAttribute>|指定 .NET 类的 ProgID。 可用于特性类。|  
|<xref:System.Runtime.InteropServices.StructLayoutAttribute>|控制类的字段的物理布局。<br /><br /> COM 互操作工具可以应用此特性。|  
  
## <a name="conversion-tool-attributes"></a>转换工具特性  

 下表介绍了转换过程期间 COM 互操作工具应用的特性。 不要在设计时应用这些特性。  
  
|特性|描述|  
|---------------|-----------------|  
|<xref:System.Runtime.InteropServices.ComAliasNameAttribute>|指示参数或字段类型的 COM 别名。 可用于特性参数、字段或返回值。|  
|<xref:System.Runtime.InteropServices.ComConversionLossAttribute>|指示有关类或接口的信息从类型库被导入到程序集时丢失。|  
|<xref:System.Runtime.InteropServices.ComEventInterfaceAttribute>|标识源接口和实现事件接口的方法的类。|  
|<xref:System.Runtime.InteropServices.ImportedFromTypeLibAttribute>|指示程序集最初是从 COM 类型库导入的。 此特性包含原始类型库的类型库定义。|  
|<xref:System.Runtime.InteropServices.TypeLibFuncAttribute>|包含最初从 COM 类型库为此功能导入的 FUNCFLAGS。|  
|<xref:System.Runtime.InteropServices.TypeLibTypeAttribute>|包含最初从 COM 类型库为此类型导入的 TYPEFLAGS。|  
|<xref:System.Runtime.InteropServices.TypeLibVarAttribute>|包含最初从 COM 类型库为此变量导入的 VARFLAGS。|  
  
## <a name="see-also"></a>请参阅

- <xref:System.Runtime.InteropServices>
- [向 COM 公开 .NET Framework 组件](../../framework/interop/exposing-dotnet-components-to-com.md)
- [特性](../attributes/index.md)
- [为互操作限定 .NET 类型](qualify-net-types-for-interoperation.md)
- [打包用于 COM 的 .NET Framework 程序集](../../framework/interop/packaging-an-assembly-for-com.md)
