---
title: ICLRMetadataLocator::GetMetadata 方法
ms.date: 03/30/2017
api_name:
- ICLRMetadataLocator.GetMetadata
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICLRMetadataLocator::GetMetadata
helpviewer_keywords:
- GetMetadata method, ICLRMetadataLocator interface [.NET Framework debugging]
- ICLRMetadataLocator::GetMetadata method [.NET Framework debugging]
ms.assetid: 704a8893-ac56-43b4-90ea-715f38ccb40e
topic_type:
- apiref
ms.openlocfilehash: f0ba2342e9704ba06dd1d3612f699298c734a5eb
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95723514"
---
# <a name="iclrmetadatalocatorgetmetadata-method"></a>ICLRMetadataLocator::GetMetadata 方法

由公共语言运行时调用 (CLR) 数据访问服务检索图像的元数据。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetMetadata(  
    [in]  LPCWSTR         imagePath,  
    [in]  ULONG32         imageTimestamp,  
    [in]  ULONG32         imageSize,  
    [in]  GUID*           mvid,  
    [in]  ULONG32         mdRva,  
    [in]  ULONG32         flags,  
    [in]  ULONG32         bufferSize,  
    [out, size_is(bufferSize), length_is(*dataSize)]  
          BYTE*           buffer,  
    [out] ULONG32*        dataSize  
);  
```  
  
## <a name="parameters"></a>参数  

 `imagePath`  
 中一个字符串，指定图像文件的路径。  
  
 `imageTimestamp`  
 中图像文件的时间戳。  
  
 `imageSize`  
 中图像文件的大小。  
  
 `mvid`  
 中图像的全局唯一标识符。  
  
 `mdRva`  
 中元数据 (RVA) 的相对虚拟地址。 地址相对于映像基址。  
  
 `flags`  
 中保留供将来使用。  
  
 `bufferSize`  
 中要在其中放置元数据的缓冲区的大小。  
  
 `buffer`  
 弄要在其中放置元数据的缓冲区。  
  
 `dataSize`  
 弄返回的元数据的大小。  
  
## <a name="remarks"></a>注解  

 此方法由调试应用程序的编写器实现。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** ClrData，ClrData  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICLRMetadataLocator 接口](iclrmetadatalocator-interface.md)
