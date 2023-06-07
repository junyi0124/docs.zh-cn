---
title: CorOpenFlags 枚举
ms.date: 03/30/2017
api_name:
- CorOpenFlags
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- CorOpenFlags
helpviewer_keywords:
- CorOpenFlags enumeration [.NET Framework metadata]
ms.assetid: e27a83b5-2698-4996-9032-1e0fed8b91ca
topic_type:
- apiref
ms.openlocfilehash: e676547d20dc9535241150d24b65e1fbaf9e89ab
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95725100"
---
# <a name="coropenflags-enumeration"></a>CorOpenFlags 枚举

包含一些标志值，用于控制打开清单文件时的元数据行为。  
  
## <a name="syntax"></a>语法  
  
```cpp  
typedef enum CorOpenFlags  
{  
    ofRead              =   0x00000000,  
    ofWrite             =   0x00000001,  
    ofReadWriteMask     =   0x00000001,  
    ofCopyMemory        =   0x00000002,  
    ofCacheImage        =   0x00000004,  
    ofManifestMetadata  =   0x00000008,  
    ofReadOnly          =   0x00000010,  
    ofTakeOwnership     =   0x00000020,  
    ofCacheImage        =   0x00000004,  
    ofNoTypeLib         =   0x00000080,  
    ofNoTransform       =   0x00001000,  
    ofReserved1         =   0x00000100,  
    ofReserved2         =   0x00000200,  
    ofReserved          =   0xffffff40  
} CorOpenFlags;  
```  
  
## <a name="members"></a>成员  
  
|成员|说明|  
|------------|-----------------|  
|`ofRead`|指示该文件应在只读状态下打开。|  
|`ofWrite`|指示该文件应在可写入状态下打开。<br /><br /> 如果在打开 .winmd 文件时使用 `ofWrite` 标志，则你还应该传递 `ofNoTransform` 标志。|  
|`ofReadWriteMask`|读写操作的掩码。|  
|`ofCopyMemory`|指示应将该文件读入内存。 元数据应保持自己的副本。|  
|`ofCacheImage`|已过时。 将忽略此标志。|  
|`ofManifestMetadata`|已过时。 将忽略此标志。|  
|`ofReadOnly`|指示应打开文件进行读取，并且 `QueryInterface` 无法对 [IMetaDataEmit](imetadataemit-interface.md) 进行调用。|  
|`ofTakeOwnership`|指示内存是使用对 [CoTaskMemAlloc](/windows/desktop/api/combaseapi/nf-combaseapi-cotaskmemalloc) 的调用分配的，并将由元数据释放。|  
|`ofNoTypeLib`|已过时。 将忽略此标志。|  
|`ofNoTransform`|指示应该禁用 .winmd 文件的自动转换。 也就是说，应该禁用从 Windows 运行时类型到 .NET Framework 类型的投影。 有关详细信息，请参阅 [.net 和 Windows 运行时中的 Windows 运行时和 CLR](/archive/msdn-magazine/2012/windows-8-special-issue/windows-runtime-and-the-clr-underneath-the-hood-with-net-and-the-windows-runtime)。|  
|`ofReserved1`|保留以供内部使用。|  
|`ofReserved2`|保留以供内部使用。|  
|`ofReserved`|保留以供内部使用。|  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Corhdr。h  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [元数据枚举](metadata-enumerations.md)
