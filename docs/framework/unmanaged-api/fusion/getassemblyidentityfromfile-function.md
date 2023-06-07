---
title: GetAssemblyIdentityFromFile 函数
ms.date: 03/30/2017
api_name:
- GetAssemblyIdentityFromFile
api_location:
- fusion.dll
- clr.dll
- mscorwks.dll
api_type:
- COM
f1_keywords:
- GetAssemblyIdentityFromFile
helpviewer_keywords:
- GetAssemblyIdentityFromFile function [.NET Framework fusion]
ms.assetid: 2c32da53-76c7-4048-84d0-d05207333004
topic_type:
- apiref
ms.openlocfilehash: 9580dd3bc5a7279549e8deadac95d35a33da74f8
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95724476"
---
# <a name="getassemblyidentityfromfile-function"></a>GetAssemblyIdentityFromFile 函数

获取一个指针，该指针指向在 `IUnknown` `IID` 指定文件路径的程序集中具有指定的对象。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetAssemblyIdentityFromFile (  
    [in]  LPCWSTR   pwzFilePath,  
    [in]  REFIID    riid,  
    [out] IUnknown  **ppIdentity  
 );  
```  
  
## <a name="parameters"></a>参数  

 `pwzFilePath`  
 中请求的程序集的有效路径。  
  
 `riid`  
 中 `IID` 要返回的接口的。  
  
 `ppIdentity`  
 弄返回的接口指针。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** 合成。h  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [IUnknown](/cpp/atl/iunknown)
- [合成全局静态函数](fusion-global-static-functions.md)
