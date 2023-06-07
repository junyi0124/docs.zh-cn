---
title: IMetaDataImport2::GetVersionString 方法
ms.date: 03/30/2017
api_name:
- IMetaDataImport2.GetVersionString
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataImport2::GetVersionString
helpviewer_keywords:
- GetVersionString method, IMetaDataImport2 interface [.NET Framework metadata]
- IMetaDataImport2::GetVersionString method [.NET Framework metadata]
ms.assetid: 308183ee-fd44-4432-9d86-ef00d181b49b
topic_type:
- apiref
ms.openlocfilehash: 3e62b1177c0161883ad03086723cc43b71292df5
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95702556"
---
# <a name="imetadataimport2getversionstring-method"></a>IMetaDataImport2::GetVersionString 方法

获取用于生成程序集的运行时的版本号。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetVersionString (  
   [out] LPWSTR      pwzBuf,  
   [in]  DWORD       ccBufSize,  
   [out] DWORD       *pccBufSize  
);  
```  
  
## <a name="parameters"></a>参数  

 `pwzBuf`  
 弄用于存储指定版本的字符串的数组。  
  
 `ccBufSize`  
 中数组的大小（以宽字符为范围） `pwzBuf` 。  
  
 `pccBufSize`  
 弄数组中返回的宽字符数，包括 null 结束符 `pwzBuf` 。  
  
## <a name="remarks"></a>注解  

 `GetVersionString`方法获取当前元数据范围的生成版本。 如果从未保存过范围，则它将不会有内置版本，并将返回空字符串。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Cor  
  
 **库：** 用作 MsCorEE.dll 中的资源  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [IMetaDataImport2 接口](imetadataimport2-interface.md)
- [IMetaDataImport 接口](imetadataimport-interface.md)
