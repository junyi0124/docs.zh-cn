---
title: StrongNameGetBlob 函数
ms.date: 03/30/2017
api_name:
- StrongNameGetBlob
api_location:
- mscoree.dll
api_type:
- DLLExport
f1_keywords:
- StrongNameGetBlob
helpviewer_keywords:
- StrongNameGetBlob function [.NET Framework strong naming]
ms.assetid: 15d09166-be00-4696-913f-2c1fbc7ac2e1
topic_type:
- apiref
ms.openlocfilehash: 8f5cb89294004dfb1f020627ceb1edb58735f72c
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95732276"
---
# <a name="strongnamegetblob-function"></a>StrongNameGetBlob 函数

使用指定地址处可执行文件的二进制表示形式填充指定的缓冲区。  
  
 此函数已弃用。 改为使用 [ICLRStrongName：： StrongNameGetBLob](../hosting/iclrstrongname-strongnamegetblob-method.md) 方法。  
  
## <a name="syntax"></a>语法  
  
```cpp  
BOOLEAN StrongNameGetBlob (  
    [in]  LPCWSTR    wszFilePath,  
    [in]  BYTE       *pbBlob,  
    [in, out] DWORD  *pcbBlob  
);  
```  
  
## <a name="parameters"></a>参数  

 `wszFilePath`  
 中要加载的可执行文件的有效路径。  
  
 `pbBlob`  
 中要在其中加载可执行文件的缓冲区。  
  
 `pcbBlob`  
 [in，out]请求的最大大小（以字节为单位） `pbBlob` 。 返回时，的实际大小（以字节为单位） `pbBlob` 。  
  
## <a name="return-value"></a>返回值  

 `true` 成功完成时;否则为 `false` 。  
  
## <a name="remarks"></a>注解  

 如果 `StrongNameGetBlob` 函数未成功完成，请调用 [StrongNameErrorInfo](strongnameerrorinfo-function.md) 函数来检索上次生成的错误。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Stackexchange.redis.strongname  
  
 **库：** 作为中的资源包含 MsCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [StrongNameGetBlob 方法](../hosting/iclrstrongname-strongnamegetblob-method.md)
- [StrongNameGetBlobFromImage 方法](../hosting/iclrstrongname-strongnamegetblobfromimage-method.md)
- [ICLRStrongName 接口](../hosting/iclrstrongname-interface.md)
