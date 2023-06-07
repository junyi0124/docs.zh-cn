---
title: OSINFO 结构
ms.date: 03/30/2017
api_name:
- OSINFO
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- OSINFO
helpviewer_keywords:
- OSINFO structure [.NET Framework metadata]
ms.assetid: fac7b480-7adb-4450-a5e9-690fed81ffae
topic_type:
- apiref
ms.openlocfilehash: 49e29cc0367d5162dffcd641b163fd7b9a56ffd0
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95672872"
---
# <a name="osinfo-structure"></a>OSINFO 结构

包含有关程序集或模块的操作系统的详细信息。  
  
## <a name="syntax"></a>语法  
  
```cpp  
typedef struct {  
    DWORD   dwOSPlatformId;  
    DWORD   dwOSMajorVersion;
    DWORD   dwOSMinorVersion;
} OSINFO;  
```  
  
## <a name="members"></a>成员  
  
|成员|说明|  
|------------|-----------------|  
|`dwOSPlatformId`|Microsoft Windows 平台函数定义的标识符值之一 `GetVersionEx` 。 支持以下值：<br /><br /> -VER_PLATFORM_WIN32s 或0x0000，用于指定 Microsoft Windows 3.1。<br />-VER_PLATFORM_WIN32_WINDOWS 或0x0001，用于指定 Windows 95、Windows 98 或其上的操作系统。<br />-VER_PLATFORM_WIN32_NT 或0x0002，用于指定其上的 Windows NT 或操作系统。|  
|`dwOSMajorVersion`|操作系统主版本，或指示任何版本的 NULL 值。|  
|`dwOSMinorVersion`|操作系统次要版本，或指示任何版本的 NULL 值。|  
  
## <a name="remarks"></a>注解  

 `OSINFO` 基于对 `OSVERSIONINFOEX` Microsoft Windows 平台函数的调用中使用的结构 `GetVersionEx` 。 ASSEMBLYMETADATA 结构使用此结构来指示其操作系统支持。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Cor  
  
 **库：** 用作 MsCorEE.dll 中的资源  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [元数据结构](metadata-structures.md)
- [IMetaDataAssemblyEmit 接口](imetadataassemblyemit-interface.md)
