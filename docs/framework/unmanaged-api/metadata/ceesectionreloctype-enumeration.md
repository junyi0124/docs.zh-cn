---
title: CeeSectionRelocType 枚举
ms.date: 03/30/2017
api_name:
- CeeSectionRelocType
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- CeeSectionRelocType
helpviewer_keywords:
- CeeSectionRelocType enumeration [.NET Framework metadata]
ms.assetid: 124656f6-0dad-4ceb-9043-d3869ab65cde
topic_type:
- apiref
ms.openlocfilehash: f7aa9699e9929608c90020c6b2d66c301fc11955
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95732705"
---
# <a name="ceesectionreloctype-enumeration"></a>CeeSectionRelocType 枚举

提供一些值，以影响对 `reloc` [ICeeGen：： AddSectionReloc](iceegen-addsectionreloc-method.md)的调用中发出的指令的类型。  
  
## <a name="syntax"></a>语法  
  
```cpp  
typedef enum  {  
    srRelocAbsolute,  
    srRelocHighLow          = 3,  
    srRelocHighAdj,
    srRelocMapToken,  
    srRelocRelative,  
    srRelocFilePos,  
    srRelocCodeRelative,  
    srRelocIA64Imm64,  
    srRelocDir64,  
    srRelocIA64PcRel25,  
    srRelocIA64PcRel64,    srRelocAbsoluteTagged,    srRelocSentinel,    srNoBaseReloc       = 0x4000,  
    srRelocPtr          = 0x8000,  
    srRelocAbsolutePtr      = srRelocPtr + srRelocAbsolute,  
    srRelocHighLowPtr       = srRelocPtr + srRelocHighLow,  
    srRelocRelativePtr      = srRelocPtr + srRelocRelative,  
    srRelocIA64Imm64Ptr     = srRelocPtr + srRelocIA64Imm64,  
    srRelocDir64Ptr         = srRelocPtr + srRelocDir64  
    } CeeSectionRelocType;  
```  
  
## <a name="members"></a>成员  
  
|成员|说明|  
|------------|-----------------|  
|`srRelocAbsolute`|仅生成一个相对节 `reloc` ，不向 .reloc 节发送任何内容。|  
|`srRelocHighLow`|`reloc`为指针大小的位置生成。 这会根据平台转换为 BASED_HIGHLOW 或 BASED_DIR64。|  
|`srRelocHighAdj`|`reloc`为32位数字的前16位生成，其中后16位包含在 .reloc 表中的下一个单词中。|  
|`srRelocMapToken`|生成一个标记映射重定位，无需将其发送到 .reloc 部分。|  
|`srRelocRelative`|指示值是相对地址链接地址。|  
|`srRelocFilePos`|仅生成一个相对节 `reloc` ，不向 .reloc 节发送任何内容。 这与 `reloc` 节的文件位置相关，而不是部分的虚拟地址。|  
|`srRelocCodeRelative`|指定代码相对地址链接地址。|  
|`srRelocIA64Imm64`|`reloc`为 ia64 指令中的64位地址生成 `movl` 。|  
|`srRelocDir64`|`reloc`为64位地址生成。|  
|`srRelocIA64PcRel25`|`reloc`为 ia64 指令中的25位 PC 相对地址生成 `br.call` 。|  
|`srRelocIA64PcRel64`|`reloc`为 ia64 指令中的64位电脑相对地址生成 `brl.call` 。|  
|`srRelocAbsoluteTagged`|生成一个与标记的指针值相对应的30位部分 `reloc` 。|  
|`srRelocSentinel`|一个 sentinel 值，用于帮助确保对此枚举的任何添加都反映到内部 `reloc` 名称数组中。|  
|`srNoBaseReloc`|指定不发出 base `reloc` 。|  
|`srRelocPtr`|一个值，该值指示内存的修正内容是指针而不是节偏移量。|  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Cor  
  
 **库：** 作为中的资源包含 MsCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [元数据枚举](metadata-enumerations.md)
- [ICeeGen 接口](iceegen-interface.md)
- [AddSectionReloc 方法](iceegen-addsectionreloc-method.md)
