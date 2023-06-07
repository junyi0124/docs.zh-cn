---
title: ICLRDataTarget::Request 方法
ms.date: 03/30/2017
api_name:
- ICLRDataTarget.Request
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICLRDataTarget::Request
helpviewer_keywords:
- ICLRDataTarget::Request method [.NET Framework debugging]
- Request method [.NET Framework debugging]
ms.assetid: 4723bd1c-eddb-4ed2-897a-010024a47e01
topic_type:
- apiref
ms.openlocfilehash: 4d0cf22b4b0644d6b25d6b3ef884718cb9ca1e42
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95723774"
---
# <a name="iclrdatatargetrequest-method"></a>ICLRDataTarget::Request 方法

由公共语言运行时调用 (CLR) 数据访问服务请求操作，如实现所定义。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT Request (  
    [in] ULONG32            reqCode,  
    [in] ULONG32            inBufferSize,  
    [in, size_is(inBufferSize)]
        BYTE                *inBuffer,  
    [in] ULONG32            outBufferSize,  
    [out, size_is(outBufferSize)]
        BYTE                *outBuffer  
);  
```  
  
## <a name="parameters"></a>参数  

 `reqCode`  
 中用户定义的。  
  
 `inBufferSize`  
 中输入缓冲区的大小，该大小用于传入的请求。  
  
 `inBuffer`  
 中包含请求的缓冲区。  
  
 `outBufferSize`  
 中用于响应的输出缓冲区的大小。  
  
 `outBuffer`  
 弄包含响应的缓冲区。  
  
## <a name="remarks"></a>注解  

 `Request`方法便于添加未指定的自定义操作。 也就是说，此方法提供了扩展性，无需版本的接口定义。  
  
 此方法由调试应用程序的编写器实现。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** ClrData，ClrData  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICLRDataTarget 接口](iclrdatatarget-interface.md)
