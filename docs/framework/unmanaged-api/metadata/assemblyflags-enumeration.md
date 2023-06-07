---
title: AssemblyFlags 枚举
ms.date: 03/30/2017
api_name:
- AssemblyFlags
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- AssemblyFlags
helpviewer_keywords:
- AssemblyFlags enumeration [.NET Framework metadata]
ms.assetid: 40f9bd9e-16ec-447e-81b0-168c875e9866
topic_type:
- apiref
ms.openlocfilehash: 561b4d68a574a2859286fb5f2e2d950518a9d29d
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95732770"
---
# <a name="assemblyflags-enumeration"></a>AssemblyFlags 枚举

包含用于描述程序集的运行时功能的值。  
  
## <a name="syntax"></a>语法  
  
```cpp  
typedef enum {  
    afImplicitExportedTypes = 0x0001,  
    afImplicitResources = 0x0002,  
    afNonSideBySideAppDomain = 0x0010,  
    afNonSideBySideProcess = 0x0020,  
    afNonSideBySideMachine = 0x0030  
} AssemblyFlags;  
```  
  
## <a name="members"></a>成员  
  
|成员|说明|  
|------------|-----------------|  
|`afImplicitExportedTypes`|指定导出的类型定义在构成程序集的文件中是隐式的。 在 .NET Framework 版本1.0 和1.1 中，始终假定此值已设置。|  
|`afImplicitResources`|指定资源定义在构成程序集的文件中是隐式的。 在 .NET Framework 1.0 和1.1 中，始终假定此值已设置。|  
|`afNonSideBySideAppDomain`|指定程序集不能与其他版本在同一应用程序域中一起执行。|  
|`afNonSideBySideProcess`|指定程序集不能与其他版本在同一进程中一起执行。|  
|`afNonSideBySideMachine`|如果程序集在同一台计算机上运行，则指定该程序集无法与其他版本一起执行。|  
  
## <a name="remarks"></a>注解  

 0x0010 和0x0070 （含）之间的值用于描述所引用程序集的并行兼容性功能。 如果未设置这些值，则假定程序集是并行兼容的。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Mscoree.dll  
  
 **库：** 作为中的资源包含 MsCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [元数据枚举](metadata-enumerations.md)
- [IMetaDataAssemblyEmit 接口](imetadataassemblyemit-interface.md)
