---
title: StrongNameGetBlobFromImage 函数
ms.date: 03/30/2017
api_name:
- StrongNameGetBlobFromImage
api_location:
- mscoree.dll
api_type:
- DLLExport
f1_keywords:
- StrongNameGetBlobFromImage
helpviewer_keywords:
- StrongNameGetBlobFromImage function [.NET Framework strong naming]
ms.assetid: 1de658e6-da32-4d01-9097-6f43c92222e1
topic_type:
- apiref
ms.openlocfilehash: 3a84221f94bad76d69f0dc67fe695ada3f3862f4
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95732237"
---
# <a name="strongnamegetblobfromimage-function"></a>StrongNameGetBlobFromImage 函数

获取指定内存地址处程序集映像的二进制表示形式。  
  
 此函数已弃用。 改为使用 [ICLRStrongName：： StrongNameGetBlobFromImage](../hosting/iclrstrongname-strongnamegetblobfromimage-method.md) 方法。  
  
## <a name="syntax"></a>语法  
  
```cpp  
BOOLEAN StrongNameGetBlobFromImage (  
    [in]  BYTE        *pbBase,  
    [in]  DWORD       dwLength,  
    [in]  BYTE        *pbBlob,  
    [in, out] DWORD   *pcbBlob  
);  
```  
  
## <a name="parameters"></a>参数  

 `pbBase`  
 中映射的程序集清单的内存地址。  
  
 `dwLength`  
 中中图像的大小（以字节为单位） `pbBase` 。  
  
 `pbBlob`  
 中包含图像的二进制表示形式的缓冲区。  
  
 `pcbBlob`  
 [in，out]请求的最大大小（以字节为单位） `pbBlob` 。 返回时，的实际大小（以字节为单位） `pbBlob` 。  
  
## <a name="return-value"></a>返回值  

 `true` 成功完成时;否则为 `false` 。  
  
## <a name="remarks"></a>注解  

 如果 `StrongNameGetBlobFromImage` 函数未成功完成，请调用 [StrongNameErrorInfo](strongnameerrorinfo-function.md) 函数来检索上次生成的错误。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Stackexchange.redis.strongname  
  
 **库：** 作为中的资源包含 MsCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [StrongNameGetBlobFromImage 方法](../hosting/iclrstrongname-strongnamegetblobfromimage-method.md)
- [StrongNameGetBlob 方法](../hosting/iclrstrongname-strongnamegetblob-method.md)
- [ICLRStrongName 接口](../hosting/iclrstrongname-interface.md)
