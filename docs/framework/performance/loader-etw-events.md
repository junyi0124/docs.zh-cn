---
title: 加载程序 ETW 事件
description: 查看加载器 ETW 事件，其中包括应用程序域事件、CLR 加载程序程序集事件、模块事件、CLR 域模块事件和模块范围事件。
ms.date: 03/30/2017
helpviewer_keywords:
- loader events [.NET Framework]
- ETW, loader events (CLR)
ms.assetid: cb403cc6-56f8-4609-b467-cdfa09f07909
ms.openlocfilehash: 7de4ad48ae275b4119f05a5269e9819c201027fd
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96240584"
---
# <a name="loader-etw-events"></a>加载程序 ETW 事件

这些事件将收集与加载和卸载应用程序域、程序集和模块相关的信息。  
  
 所有加载程序事件均在 `LoaderKeyword` (0x8) 关键字下引发。 `DCStart` 和 `DCEnd` 事件在 `LoaderRundownKeyword` (0x8) 下引发（启用了 `StartRundown`/`EndRundown`）。 （有关详细信息，请参阅 [CLR ETW Keywords and Levels](clr-etw-keywords-and-levels.md)。）  

## <a name="application-domain-events"></a>应用程序域事件

 下表显示了关键字和级别。  
  
|引发事件的关键字|事件|Level|  
|-----------------------------------|-----------|-----------|  
|`LoaderKeyword` (0x8)|`AppDomainLoad_V1` 和 `AppDomainUnLoad_V1`|信息性 (4)|  
|`LoaderRundownKeyword` (0x8) +<br /><br /> `StartRundownKeyword`|`AppDomainDCStart_V1`|信息性 (4)|  
|`LoaderRundownKeyword` (0x8) +<br /><br /> `EndRundownKeyword`|`AppDomainDCEnd_V1`|信息性 (4)|  
  
 下表显示了事件信息。  
  
|事件|事件 ID|描述|  
|-----------|--------------|-----------------|  
|`AppDomainLoad_V1` （为所有应用程序域记录）|156|每当在进程生存期内创建应用程序域时引发。|  
|`AppDomainUnLoad_V1`|157|每当在进程生存期内销毁应用程序域时引发。|  
|`AppDomainDCStart_V1`|157|在启动断开期间枚举应用程序域。|  
|`AppDomainDCEnd_V1`|158|在结束断开期间枚举应用程序域。|  
  
 下表显示了事件数据。  
  
|字段名称|数据类型|说明|  
|----------------|---------------|-----------------|  
|AppDomainID|win:UInt64|应用程序域的唯一标识符。|  
|AppDomainFlags|win:UInt32|0x1：默认域。<br /><br /> 0x2：可执行。<br /><br /> 0x4：应用程序域，位 28-31：共享此域的策略。<br /><br /> 0：一个共享域。|  
|AppDomainName|win:UnicodeString|友好的应用程序域名。 可能会在进程生存期内更改。|  
|AppDomainIndex|win:UInt32|此应用程序域的索引。|  
|ClrInstanceID|win:UInt16|CLR 或 CoreCLR 的实例的唯一 ID。|  

## <a name="clr-loader-assembly-events"></a>CLR 加载程序程序集事件  

 下表显示了关键字和级别。  
  
|引发事件的关键字|事件|Level|  
|-----------------------------------|-----------|-----------|  
|`LoaderKeyword` (0x8)|`AssemblyLoad` 和 `AssemblyUnload`|信息性 (4)|  
|`LoaderRundownKeyword` (0x8) +<br /><br /> `StartRundownKeyword`|`AssemblyDCStart`|信息性 (4)|  
|`LoaderRundownKeyword` (0x8) +<br /><br /> `EndRundownKeyword`|`AssemblyDCEnd`|信息性 (4)|  
  
 下表显示了事件信息。  
  
|事件|事件 ID|描述|  
|-----------|--------------|-----------------|  
|`AssemblyLoad_V1`|154|在加载程序集时引发。|  
|`AssemblyUnload_V1`|155|在卸载程序集时引发。|  
|`AssemblyDCStart_V1`|155|在启动断开期间枚举程序集。|  
|`AssemblyDCEnd_V1`|156|在结束断开期间枚举程序集。|  
  
 下表显示了事件数据。  
  
|字段名称|数据类型|说明|  
|----------------|---------------|-----------------|  
|AssemblyID|win:UInt64|程序集的唯一 ID。|  
|AppDomainID|win:UInt64|此程序集的域的 ID。|  
|BindingID|win:UInt64|唯一地标识程序集绑定的 ID。|  
|AssemblyFlags|win:UInt32|0x1：非特定于域的程序集。<br /><br /> 0x2：动态程序集。<br /><br /> 0x4：程序集具有本机映像。<br /><br /> 0x8：可回收程序集。|  
|AssemblyName|win:UnicodeString|完全限定程序集名称。|  
|ClrInstanceID|win:UInt16|CLR 或 CoreCLR 的实例的唯一 ID。|

## <a name="module-events"></a>模块事件

 下表显示了关键字和级别。  
  
|引发事件的关键字|事件|Level|  
|-----------------------------------|-----------|-----------|  
|`LoaderKeyword` (0x8)|`ModuleLoad_V2` 和 `ModuleUnload_V2`|信息性 (4)|  
|`LoaderRundownKeyword` (0x8) +<br /><br /> `StartRundownKeyword`|`ModuleDCStart_V2`|信息性 (4)|  
|`LoaderRundownKeyword` (0x8) +<br /><br /> `EndRundownKeyword`|`ModuleDCEnd_V2`|信息性 (4)|  
||||  
  
 下表显示了事件信息。  
  
|事件|事件 ID|描述|  
|-----------|--------------|-----------------|  
|`ModuleLoad_V2`|152|在进程的生存期内加载模块时引发。|  
|`ModuleUnload_V2`|153|在进程的生存期内卸载模块时引发。|  
|`ModuleDCStart_V2`|153|在启动断开期间枚举模块。|  
|`ModuleDCEnd_V2`|154|在结束断开期间枚举模块。|  
  
 下表显示了事件数据。  
  
|字段名称|数据类型|说明|  
|----------------|---------------|-----------------|  
|ModuleID|win:UInt64|模块的唯一 ID。|  
|AssemblyID|win:UInt64|此模块所驻留的程序集的 ID。|  
|ModuleFlags|win:UInt32|0x1：非特定于域的模块。<br /><br /> 0x2：模块具有本机映像。<br /><br /> 0x4：动态模块。<br /><br /> 0x8：清单模块。|  
|Reserved1|win:UInt32|保留的字段。|  
|ModuleILPath|win:UnicodeString|模块的 Microsoft 中间语言 (MSIL) 映像的路径；如果是动态程序集（以 null 结尾），则为动态模块名。|  
|ModuleNativePath|win:UnicodeString|如果存在（以 null 结尾），则为模块本机映像的路径。|  
|ClrInstanceID|win:UInt16|CLR 或 CoreCLR 的实例的唯一 ID。|  
|ManagedPdbSignature|win:GUID|匹配此模块的托管程序数据库 (PDB) 的 GUID 签名。 （请参阅“备注”。）|  
|ManagedPdbAge|win:UInt32|写入匹配此模块的托管 PDB 的年限数。 （请参阅“备注”。）|  
|ManagedPdbBuildPath|win:UnicodeString|生成匹配此模块的托管 PDB 的位置的路径。 在某些情况下，这可能只是一个文件名。 （请参阅“备注”。）|  
|NativePdbSignature|win:GUID|匹配此模块的本机映像生成器 (NGen) PDB 的 GUID 签名（如果适用）。 （请参阅“备注”。）|  
|NativePdbAge|win:UInt32|写入匹配此模块的 NGen PDB 的年限数（如果适用）。 （请参阅“备注”。）|  
|NativePdbBuildPath|win:UnicodeString|生成匹配此模块的 NGen PDB 的位置的路径（如果适用）。 在某些情况下，这可能只是一个文件名。 （请参阅“备注”。）|  
  
### <a name="remarks"></a>备注  
  
- 可通过分析工具使用名称中具有“Pdb”的字段，以便查找匹配分析会话期间所加载的模块的 PDB。 这些字段的值对应写入模块 IMAGE_DIRECTORY_ENTRY_DEBUG 部分的数据，调试器通常使用此模块来帮助查找匹配已加载模块的 PDB。  
  
- 以“ManagedPdb”开头的字段名是指对应于由托管编译器 （如 C# 或 Visual Basic 编译器）生成的 MSIL 模块的托管 PDB。 此 PDB 使用托管的 PDB 格式，并介绍原始托管源代码中的元素（如文件、行号和符号名）如何映射到被编译到 MSIL 模块中的 MSIL 元素。  
  
- 以“NativePdb”开头的字段名是指通过调用 `NGEN createPDB`而生成的 NGen PDB。 此 PDB 使用本机 PDB 格式，并介绍元素如何从原始托管的源代码中（如文件、行号和符号名）映射到被编译到 NGen 模块的本机元素。  

## <a name="clr-domain-module-events"></a>CLR 域模块事件

 下表显示了关键字和级别。  
  
|引发事件的关键字|事件|Level|  
|-----------------------------------|-----------|-----------|  
|`LoaderKeyword` (0x8)|`DomainModuleLoad_V1`|信息性 (4)|  
|`LoaderRundownKeyword` (0x8) +<br /><br /> `StartRundownKeyword`|`DomainModuleDCStart_V1`|信息性 (4)|  
|`LoaderRundownKeyword` (0x8) +<br /><br /> `EndRundownKeyword`|`DomainModuleDCEnd_V1`|信息性 (4)|  
  
 下表显示了事件信息。  
  
|事件|事件 ID|描述|  
|-----------|--------------|-----------------|  
|`DomainModuleLoad_V1`|151|在为应用程序域加载模块时引发。|  
|`DomainModuleDCStart_V1`|151|在启动断开期间为应用程序域枚举加载的模块，并为所有应用程序域记录。|  
|`DomainModuleDCEnd_V1`|152|在结束断开期间为应用程序域枚举加载的模块，并为所有应用程序域记录。|  
  
 下表显示了事件数据。  
  
|字段名称|数据类型|说明|  
|----------------|---------------|-----------------|  
|ModuleID|win:UInt64|标识此模块所属的程序集。|  
|AssemblyID|win:UInt64|此模块所驻留的程序集的 ID。|  
|AppDomainID|win:UInt64|其中使用此模块的应用程序域的 ID。|  
|ModuleFlags|win:UInt32|0x1：非特定于域的模块。<br /><br /> 0x2：模块具有本机映像。<br /><br /> 0x4：动态模块。<br /><br /> 0x8：清单模块。|  
|Reserved1|win:UInt32|保留的字段。|  
|ModuleILPath|win:UnicodeString|模块的 MSIL 映像的路径；如果是动态程序集（以 null 结尾），则为动态模块名。|  
|ModuleNativePath|win:UnicodeString|如果存在（以 null 结尾），则为模块本机映像的路径。|  
|ClrInstanceID|win:UInt16|CLR 或 CoreCLR 的实例的唯一 ID。|  

## <a name="module-range-events"></a>模块范围事件

 下表显示了关键字和级别。  
  
|引发事件的关键字|事件|Level|  
|-----------------------------------|-----------|-----------|  
|`PerfTrackKeyWord`)|`ModuleRange`|信息性 (4)|  
|`PerfTrackKeyWord`|`ModuleRangeDCStart`|信息性 (4)|  
|`PerfTrackKeyWord`|`ModuleRangeDCEnd`|信息性 (4)|  
  
 下表显示了事件信息。  
  
|事件|事件 ID|描述|  
|-----------|--------------|-----------------|  
|`ModuleRange`|158|如果加载的本机映像生成器 (NGen) 映像已使用 IBC 进行优化，并且包含有关 NGen 映像热区的信息，则此事件存在。|  
|`ModuleRangeDCStart`|160|`ModuleRange` 事件在断开开始时引发。|  
|`ModuleRangeDCEnd`|161|`ModuleRange` 事件在断开结束时引发。|  
  
 下表显示了事件数据。  
  
|字段名称|数据类型|说明|  
|----------------|---------------|-----------------|  
|ClrInstanceID|win:UInt16|如果加载了 CLR 的多个实例，则唯一标识进程中 CLR 的特定实例。|  
|ModuleID|win:UInt64|标识此模块所属的程序集。|  
|RangeBegin|win:UInt32|模块中表示指定范围类型的范围开始位置的偏移量。|  
|RangeSize|win:UInt32|指定的范围大小（以字节为单位）。|  
|RangeType|win:UInt32|单个值，0x4，表示 Cold IBC 范围。 此字段未来可以表示多个值。|  
|RangeSize1|win:UInt32|0 指示不良数据。|  
|RangeBegin2|win:UnicodeString||  
  
### <a name="remarks"></a>备注  

 如果已使用 IBC 优化了 .NET Framework 进程中加载的 NGen 映像，则包含 NGen 映像中热度范围的 `ModuleRange` 事件将与其 `moduleID` 和 `ClrInstanceID`一起记录。  如果未使用 IBC 优化 NGen 映像，则不记录此事件。 若要确定模块名，则此事件必须与模块加载 ETW 事件进行整理。  
  
 此事件的负载大小可变； `Count` 字段指示事件中包含的范围偏移量数。  此事件必须与 Windows `IStart` 事件进行整理，以确定实际范围。 每当加载映像且包含已加载映像的虚拟地址时，将记录 Windows 映像加载事件。  
  
 在任何大于或等于 4 的 ETW 级别下引发模块范围事件，并将其归类为信息性事件。  
  
## <a name="see-also"></a>另请参阅

- [CLR ETW 事件](clr-etw-events.md)
