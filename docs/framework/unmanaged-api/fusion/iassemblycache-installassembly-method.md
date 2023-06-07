---
title: IAssemblyCache::InstallAssembly 方法
ms.date: 03/30/2017
api_name:
- IAssemblyCache.InstallAssembly
api_location:
- fusion.dll
api_type:
- COM
f1_keywords:
- IAssemblyCache::InstallAssembly
helpviewer_keywords:
- InstallAssembly method [.NET Framework fusion]
- IAssemblyCache::InstallAssembly method [.NET Framework fusion]
ms.assetid: 33c1d269-c85e-4cb1-b0e6-1c510c8fb5fa
topic_type:
- apiref
ms.openlocfilehash: 230b904dd1cca1a1289713e3df7a709bd1c3a22b
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95696877"
---
# <a name="iassemblycacheinstallassembly-method"></a>IAssemblyCache::InstallAssembly 方法

在全局程序集缓存中安装指定的程序集。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT InstallAssembly (  
    [in] DWORD dwFlags,  
    [in] LPCWSTR pszManifestFilePath,  
    [in] LPCFUSION_INSTALL_REFERENCE pRefData  
);  
```  
  
## <a name="parameters"></a>参数  

 `dwFlags`  
 中在合成 .idl 中定义的标志。 支持以下值：  
  
-  (0x00000001) IASSEMBLYCACHE_INSTALL_FLAG_REFRESH  
  
- IASSEMBLYCACHE_INSTALL_FLAG_FORCE_REFRESH (0x00000002)   
  
 `pszManifestFilePath`  
 中要安装的程序集的清单的路径。  
  
 `pRefData`  
 中一个 [FUSION_INSTALL_REFERENCE](fusion-install-reference-structure.md) 结构，它包含安装的数据。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** 合成。h  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [IAssemblyCache 接口](iassemblycache-interface.md)
