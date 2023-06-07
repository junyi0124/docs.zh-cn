---
title: CorAssemblyFlags 枚举
ms.date: 03/30/2017
api_name:
- CorAssemblyFlags
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- CorAssemblyFlags
helpviewer_keywords:
- CorAssemblyFlags enumeration [.NET Framework metadata]
ms.assetid: bb8db3b6-d81d-49fc-b74c-dbc908a9eab9
topic_type:
- apiref
ms.openlocfilehash: 615c4ac95ab777e8081e630cafb6671e64dea78a
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95718977"
---
# <a name="corassemblyflags-enumeration"></a>CorAssemblyFlags 枚举

包含一些值，用于描述应用于程序集编译的元数据。  
  
## <a name="syntax"></a>语法  
  
```cpp  
typedef enum CorAssemblyFlags {  
  
    afPublicKey             =   0x0001,  
    afPA_None               =   0x0000,  
    afPA_MSIL               =   0x0010,  
    afPA_x86                =   0x0020,  
    afPA_IA64               =   0x0030,  
    afPA_AMD64              =   0x0040,  
    afPA_ARM                =   0x0050,  
    afPA_NoPlatform         =   0x0070,  
    afPA_Specified          =   0x0080,  
    afPA_Mask               =   0x0070,  
    afPA_FullMask           =   0x00F0,  
    afPA_Shift              =   0x0004,  
  
    afEnableJITcompileTracking  =   0x8000,  
    afDisableJITcompileOptimizer=   0x4000,  
  
    afRetargetable          =   0x0100,  
    afContentType_Default        =   0x0000,  
    afContentType_WindowsRuntime =   0x0200,  
    afContentType_Mask           =   0x0E00,  
  
} CorAssemblyFlags;  
```  
  
## <a name="members"></a>成员  
  
|成员|说明|  
|------------|-----------------|  
|`afPublicKey`|指示程序集引用包含经过哈希的完整公钥。|  
|`afPA_None`|指示未指定处理器体系结构。|  
|`afPA_MSIL`|指示处理器体系结构为中性 (PE32) 。|  
|`afPA_x86`|指示处理器体系结构为 x86 (PE32) 。|  
|`afPA_IA64`|指示处理器体系结构为 Itanium (PE32 +) 。|  
|`afPA_AMD64`|指示处理器体系结构为 AMD X64 (PE32 +) 。|  
|`afPA_ARM`|指示处理器体系结构为 ARM (PE32) 。|  
|`afPA_NoPlatform`|指示程序集是引用程序集;也就是说，它适用于任何体系结构，但不能在任何体系结构上运行。 因此，标志与相同 `afPA_Mask` 。|  
|`afPA_Specified`|指示应将处理器体系结构标志传播到 `AssemblyRef` 记录。|  
|`afPA_Mask`|描述处理器体系结构的掩码。|  
|`afPA_FullMask`|指定包含处理器体系结构说明。|  
|`afPA_Shift`|指示处理器体系结构标志中与索引之间的移位计数。|  
|`afEnableJITcompileTracking`|指示的中的对应值 <xref:System.Diagnostics.DebuggableAttribute.DebuggingModes> <xref:System.Diagnostics.DebuggableAttribute> 。|  
|`afDisableJITcompileOptimizer`|指示的中的对应值 <xref:System.Diagnostics.DebuggableAttribute.DebuggingModes> <xref:System.Diagnostics.DebuggableAttribute> 。|  
|`afRetargetable`|指示程序集可在运行时从不同的发布服务器重定向到程序集。|  
|`afContentType_Mask`|描述内容类型的掩码。|  
|`afContentType_Default`|指示默认内容类型。|  
|`afContentType_WindowsRuntime`|指示 Windows 运行时内容类型。|  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Corhdr。h  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [元数据枚举](metadata-enumerations.md)
