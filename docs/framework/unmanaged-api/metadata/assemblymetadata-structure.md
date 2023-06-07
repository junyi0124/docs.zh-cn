---
title: ASSEMBLYMETADATA 结构
ms.date: 03/30/2017
api_name:
- ASSEMBLYMETADATA
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- ASSEMBLYMETADATA
helpviewer_keywords:
- ASSEMBLYMETADATA structure [.NET Framework metadata]
ms.assetid: 1af98e57-9145-4d35-bb78-77d1da7c91a5
topic_type:
- apiref
ms.openlocfilehash: 8071c3f43775975de37e3255582b6fc8f13f7de3
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95732775"
---
# <a name="assemblymetadata-structure"></a>ASSEMBLYMETADATA 结构

包含有关被引用程序集的信息，包括其版本及其对区域设置、处理器和操作系统的支持级别。  
  
## <a name="syntax"></a>语法  
  
```cpp  
typedef struct {  
    USHORT  usMajorVersion;  
    USHORT  usMinorVersion;  
    USHORT  usBuildNumber;  
    USHORT  usRevisionNumber;  
    LPWSTR  szLocale;  
    ULONG   cbLocale;  
    DWORD*  rdwProcessor[];  
    ULONG   ulProcessor  
    OSINFO* rOS[];  
    ULONG   ulOS;  
} ASSEMBLYMETADATA;  
```  
  
## <a name="members"></a>成员  
  
|成员|说明|  
|------------|-----------------|  
|`usMajorVersion`|引用的程序集的主版本号。 此值不能为零。 如果设置了的所有位 `usMajorVersion` ，则不指定主版本。|  
|`usMinorVersion`|所引用程序集的次版本号。 此值不能为零。 如果设置了的所有位 `usMinorVersion` ，则不指定次版本。|  
|`usBuildNumber`|引用程序集的生成号。 此值不能为零。 如果设置了的所有位 `usBuildNumber` ，则不指定生成号。|  
|`usRevisionNumber`|引用的程序集的修订号。 此值不能为零。 如果设置了的所有位 `usRevisionNumber` ，则不指定修订号。|  
|`szLocale`|符合 RFC1766 规范（由分号分隔）的区域设置名称的列表，指定被引用程序集支持的区域设置。 空值表示区域设置独立性。 **注意：**  在 .NET Framework 版本1.0 中，不能指定多个区域设置。|  
|`cbLocale`|的大小（以宽字符为大小） `szLocale` 。|  
|`rdwProcessor`|Winnt 中定义的标识符的数组，适用于所引用的程序集所支持的处理器类型。 空值表示处理器独立性。|  
|`ulProcessor`|`rdwProcessor` 数组的长度。|  
|`rOS`|指定被引用的程序集支持的操作系统的 [OSINFO](osinfo-structure.md) 实例的数组。 NULL 值指示与操作系统无关。|  
|`ulOS`|`rOS` 数组的长度。|  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Cor  
  
 **库：** 用作 MsCorEE.dll 中的资源  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [元数据结构](metadata-structures.md)
- [IMetaDataAssemblyEmit 接口](imetadataassemblyemit-interface.md)
- [OSINFO 结构](osinfo-structure.md)
