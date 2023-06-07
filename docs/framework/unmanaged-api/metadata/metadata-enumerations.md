---
title: 元数据枚举
ms.date: 03/30/2017
helpviewer_keywords:
- enumerations [.NET Framework metadata]
- metadata enumerations [.NET Framework]
- unmanaged enumerations [.NET Framework], metadata
ms.assetid: 711ab251-cfdb-4280-aaa6-9bc1b341cdc3
ms.openlocfilehash: d0a146ff5b6887e6678ee69d7e39731bd109c6d5
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95672898"
---
# <a name="metadata-enumerations"></a>元数据枚举

本节描述元数据 API 使用的非托管枚举。  
  
## <a name="in-this-section"></a>本节内容  

 [AssemblyFlags 枚举](assemblyflags-enumeration.md)  
 包含一些值，用于描述程序集的运行时函数。  
  
 [AssemblyRefFlags 枚举](assemblyrefflags-enumeration.md)  
 包含一些值，用于描述程序集引用的函数。  
  
 [CeeSectionAttr 枚举](ceesectionattr-enumeration.md)  
 提供指定节的属性的值，供 [ICeeGen](iceegen-interface.md) 接口使用。  
  
 [CeeSectionRelocType 枚举](ceesectionreloctype-enumeration.md)  
 提供一些值，以影响对 `reloc` [ICeeGen：： AddSectionReloc](iceegen-addsectionreloc-method.md) 方法的调用中发出的指令的类型。  
  
 [COINITICOR 枚举](coiniticor-enumeration.md)  
 指定初始化公共语言运行时 [CoInitializeCor](../hosting/coinitializecor-function.md) 使用的常量。  
  
 [COINITIEE 枚举](coinitiee-enumeration.md)  
 指定初始化公共语言运行时 [CoInitializeEE](../hosting/coinitializeee-function.md) 使用的常量。  
  
 [CorArgType 枚举](corargtype-enumeration.md)  
 包含一些值，用于描述运行时句柄的本机类型。  
  
 [CorAssemblyFlags 枚举](corassemblyflags-enumeration.md)  
 包含一些值，用于描述应用于程序集编译的元数据。  
  
 [CorAttributeTargets 枚举](corattributetargets-enumeration.md)  
 指定可应用属性的应用程序元素。  
  
 [CorCallingConvention 枚举](corcallingconvention-enumeration.md)  
 包含一些值，用于描述托管代码中执行的调用约定类型。  
  
 [CorCheckDuplicatesFor 枚举](corcheckduplicatesfor-enumeration.md)  
 包含检查重复期间使用的值。  
  
 [CorDeclSecurity 枚举](cordeclsecurity-enumeration.md)  
 包含一些值，用于描述公共语言运行时使用的声明性安全类型。  
  
 CorElementType  
 包含一些值，用于描述公共语言运行时的基础本机类型 <xref:System.Type>。  
  
 [CorErrorIfEmitOutOfOrder 枚举](corerrorifemitoutoforder-enumeration.md)  
 包含一些标志值，用于指示无序发出元数据时应生成错误消息的条件。  
  
 [CorEventAttr 枚举](coreventattr-enumeration.md)  
 包含一些值，用于描述事件的元数据。  
  
 [CorFieldAttr 枚举](corfieldattr-enumeration.md)  
 包含一些值，用于描述字段的相应元数据。  
  
 [CorFileFlags 枚举](corfileflags-enumeration.md)  
 包含一些值，这些值描述在对 [IMetaDataAssemblyEmit：:D efinefile](imetadataassemblyemit-definefile-method.md) 方法的调用中定义的文件的类型。  
  
 [CorFileMapping 枚举](corfilemapping-enumeration.md)  
 包含一些值，这些值描述从对 [IMetaDataInfo：： GetFileMapping](imetadatainfo-getfilemapping-method.md) 方法的调用返回的文件映射的类型。  
  
 [CorGenericParamAttr 枚举](corgenericparamattr-enumeration.md)  
 包含一些值，这些值描述 <xref:System.Type> 泛型类型的参数， [IMetaDataEmit2：:D efinegenericparam](imetadataemit2-definegenericparam-method.md) 方法的调用中使用。  
  
 [CorImportOptions 枚举](corimportoptions-enumeration.md)  
 包含一些标志值，用于在导入当前作用域范围外的程序集的过程中控制行为。  
  
 [CorLinkerOptions 枚举](corlinkeroptions-enumeration.md)  
 指定用于选择元数据链接器的选项的标志。  
  
 [CorLocalRefPreservation 枚举](corlocalrefpreservation-enumeration.md)  
 包含用于处理本地引用的标志值。  
  
 [CorManifestResourceFlags 枚举](cormanifestresourceflags-enumeration.md)  
 包含一些值，用于描述程序集清单中编码的资源的可见性。  
  
 [CorMethodAttr 枚举](cormethodattr-enumeration.md)  
 包含一些值，用于描述方法的相应元数据。  
  
 [CorMethodImpl 枚举](cormethodimpl-enumeration.md)  
 包含一些值，用于描述方法实现功能。  
  
 [CorMethodSemanticsAttr 枚举](cormethodsemanticsattr-enumeration.md)  
 包含一些值，用于描述方法和关联属性或事件之间的关系。  
  
 [CorNativeLinkFlags 枚举](cornativelinkflags-enumeration.md)  
 提供链接本机代码时链接器使用的标志值。  
  
 [CorNativeLinkType 枚举](cornativelinktype-enumeration.md)  
 提供一些值，用于指示本机代码中链接的类型。  
  
 [CorNativeType 枚举](cornativetype-enumeration.md)  
 包含一些值，用于描述本机非托管类型。  
  
 [CorNotificationForTokenMovement 枚举](cornotificationfortokenmovement-enumeration.md)  
 包含会影响标记移动的相应通知的标志值。  
  
 [CorOpenFlags 枚举](coropenflags-enumeration.md)  
 包含一些标志值，用于控制打开清单文件时的元数据行为。  
  
 [CorParamAttr 枚举](corparamattr-enumeration.md)  
 包含一些值，用于描述方法参数的元数据。  
  
 [CorPEKind 枚举](corpekind-enumeration.md)  
 包含一些值，这些值描述可移植的可执行文件，该文件是从对 [IMetaDataImport2：： GetPEKind](imetadataimport2-getpekind-method.md) 方法的调用返回的。  
  
 [CorPinvokeMap 枚举](corpinvokemap-enumeration.md)  
 包含一些值，用于描述 PInvoke 调用的功能。  
  
 [CorPropertyAttr 枚举](corpropertyattr-enumeration.md)  
 包含一些值，用于描述属性的元数据。  
  
 [CorRefToDefCheck 枚举](correftodefcheck-enumeration.md)  
 指定用于控制将哪些引用项转换为相应定义以优化代码的标志。  
  
 [CorRegFlags 枚举](corregflags-enumeration.md)  
 提供安装模块或复合项时注册所使用的标志值。  
  
 [CorSaveSize 枚举](corsavesize-enumeration.md)  
 包含一些值，用于指示查询保存操作的大小时所需的精度级别。  
  
 [CorSerializationType 枚举](corserializationtype-enumeration.md)  
 包含一些值，用于描述公共语言运行时如何序列化对象。 这些值通常对应于 CorElementType 值。  
  
 [CorSetENC 枚举](corsetenc-enumeration.md)  
 包含一些值，用于在元数据生成期间影响行为。  
  
 [CorThreadSafetyOptions 枚举](corthreadsafetyoptions-enumeration.md)  
 指定用于选择线程安全性选项的标志。  
  
 [CorTokenType 枚举](cortokentype-enumeration.md)  
 包含一些值，用于指示元数据标记引用的对象类型。  
  
 [CorTypeAttr 枚举](cortypeattr-enumeration.md)  
 包含一些值，用于指示类型元数据。  
  
 [CorUnmanagedCallingConvention 枚举](corunmanagedcallingconvention-enumeration.md)  
 包含一些值，用于描述非托管的调用约定。  
  
 [CorValidatorModuleType 枚举](corvalidatormoduletype-enumeration.md)  
 提供 [IMetaDataValidate](imetadatavalidate-interface.md) 接口用于指定 (PE 文件与 .obj 文件) 模块的类型的值。  
  
 [COUNINITIEE 枚举](couninitiee-enumeration.md)  
 指定初始化公共语言运行时 [CoUninitializeEE](../hosting/couninitializeee-function.md) 使用的常量。  
  
## <a name="related-sections"></a>相关章节  

 [元数据接口](metadata-interfaces.md)  
  
 [元数据全局静态函数](metadata-global-static-functions.md)  
  
 [元数据结构](metadata-structures.md)  
  
 [元数据联合](metadata-unions.md)
