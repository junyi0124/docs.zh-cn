---
title: CorLaunchApplication 函数
ms.date: 03/30/2017
api_name:
- CorLaunchApplication
api_location:
- mscoree.dll
- clr.dll
api_type:
- COM
f1_keywords:
- CorLaunchApplication
helpviewer_keywords:
- CorLaunchApplication function [.NET Framework hosting]
ms.assetid: 71f362a9-8fe2-47ce-9302-05a645cf3d7d
topic_type:
- apiref
ms.openlocfilehash: ca427439f03d92b20e7714b9724d90b240e9cecb
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95704066"
---
# <a name="corlaunchapplication-function"></a>CorLaunchApplication 函数

使用指定的清单和其他应用程序数据，在指定的网络路径下启动应用程序。  
  
 此函数已在 .NET Framework 4 中弃用。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT CorLaunchApplication (  
    [in]  HOST_TYPE                dwClickOnceHost,  
    [in]  LPCWSTR                  pwzAppFullName,  
    [in]  DWORD                    dwManifestPaths,  
    [in]  LPCWSTR                 *ppwzManifestPaths,  
    [in]  DWORD                    dwActivationData,  
    [in]  LPCWSTR                 *ppwzActivationData,  
    [out] LPPROCESS_INFORMATION    lpProcessInformation  
);  
```  
  
## <a name="parameters"></a>参数  

 `dwClickOnceHost`  
 中一个 [HOST_TYPE](host-type-enumeration.md) 枚举的值，该值指定启动应用程序的主机的类型。  
  
 `pwzAppFullName`  
 中正在启动的应用程序的完整名称。  
  
 `dwManifestPaths`  
 中应用程序的清单路径数。  
  
 `ppwzManifestPaths`  
 中一个字符串数组，其中每个字符串指定要启动的应用程序的清单的路径。  
  
 `dwActivationData`  
 中正在启动的应用程序的激活数据项的数目。  
  
 `ppwzActivationData`  
 中一个字符串数组，其中每个字符串都是要启动的应用程序的激活数据项。  
  
 `lpProcessInformation`  
 弄一个指针，指向有关已加载应用程序的进程的信息。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Mscoree.dll  
  
 **库：** MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [弃用的 CLR 承载函数](deprecated-clr-hosting-functions.md)
