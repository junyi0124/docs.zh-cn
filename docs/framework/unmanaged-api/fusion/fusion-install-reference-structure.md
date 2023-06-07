---
title: FUSION_INSTALL_REFERENCE 结构
ms.date: 03/30/2017
api_name:
- FUSION_INSTALL_REFERENCE
api_location:
- fusion.dll
api_type:
- COM
f1_keywords:
- FUSION_INSTALL_REFERENCE
helpviewer_keywords:
- FUSION_INSTALL_REFERENCE structure [.NET Framework fusion]
ms.assetid: ae181ec8-36bf-4ed1-9a02-ca27d417c80b
topic_type:
- apiref
ms.openlocfilehash: d5ff08e62b94d7f164b103ae0535bb62e4e60aea
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95688804"
---
# <a name="fusion_install_reference-structure"></a>FUSION_INSTALL_REFERENCE 结构

表示应用程序对应用程序已安装在全局程序集缓存中的程序集进行的引用。  
  
## <a name="syntax"></a>语法  
  
```cpp  
typedef struct _FUSION_INSTALL_REFERENCE_ {  
    DWORD    cbSize,  
    DWORD    dwFlags,  
    GUID     guidScheme,  
    LPCWSTR  szIdentifier,  
    LPCWSTR  szNonCanonicalData  
} FUSION_INSTALL_REFERENCE, *LPFUSION_INSTALL_REFERENCE;  
```  
  
## <a name="members"></a>成员  
  
|成员|说明|  
|------------|-----------------|  
|`cbSize`|结构的大小（以字节为单位）。|  
|`dwFlags`|保留以供将来进行扩展。 此值必须为 0 (零) 。|  
|`guidScheme`|添加引用的实体。 此字段可以具有以下值之一：<br /><br /> -FUSION_REFCOUNT_MSI_GUID：程序集由使用 Microsoft Windows Installer 安装的应用程序引用。 该 `szIdentifier` 字段设置为 `MSI` ，并且该 `szNonCanonicalData` 字段设置为 `Windows Installer` 。 此方案用于 Windows 并行程序集。<br />-FUSION_REFCOUNT_UNINSTALL_SUBKEY_GUID：程序集由在 " **添加/删除程序** " 界面中显示的应用程序引用。 `szIdentifier`字段提供了用于将应用程序注册到 "**添加/删除程序**" 界面的令牌。<br />-FUSION_REFCOUNT_FILEPATH_GUID：程序集由文件系统中的文件所表示的应用程序引用。 `szIdentifier`字段提供此文件的路径。<br />-FUSION_REFCOUNT_OPAQUE_STRING_GUID：程序集由仅由不透明字符串表示的应用程序引用。 `szIdentifier`字段提供了此不透明字符串。 删除此值时，全局程序集缓存不会检查不透明引用是否存在。<br />-FUSION_REFCOUNT_OSINSTALL_GUID：保留此值。|  
|`szIdentifier`|标识在全局程序集缓存中安装程序集的应用程序的唯一字符串。 其值取决于字段的值 `guidScheme` 。|  
|`szNonCanonicalData`|仅由添加了引用的实体识别的字符串。 全局程序集缓存存储此字符串，但不使用它。|  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** 合成。h  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [合成结构](fusion-structures.md)
- [全局程序集缓存](../../app-domains/gac.md)
