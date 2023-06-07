---
title: ICLRDataTarget::SetTLSValue 方法
ms.date: 03/30/2017
api_name:
- ICLRDataTarget.SetTLSValue
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICLRDataTarget::SetTLSValue
helpviewer_keywords:
- ICLRDataTarget::SetTLSValue method [.NET Framework debugging]
- SetTLSValue method [.NET Framework debugging]
ms.assetid: 4a2d6a24-749a-47ad-9f01-4517203d3f35
topic_type:
- apiref
ms.openlocfilehash: d2eaab1f42eb04d8e9727220a08842ca75a2eadf
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95723683"
---
# <a name="iclrdatatargetsettlsvalue-method"></a>ICLRDataTarget::SetTLSValue 方法

在目标进程中指定线程 (TLS) 中设置一个值。 此方法由公共语言运行时 (CLR) 数据访问服务调用。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT SetTLSValue (  
    [in] ULONG32            threadID,  
    [in] ULONG32            index,  
    [in] CLRDATA_ADDRESS    value  
);  
```  
  
## <a name="parameters"></a>参数  

 `threadID`  
 中目标进程中的线程的操作系统标识符。  
  
 `index`  
 中位置的索引。 此值必须是指定线程的本地存储区中的有效索引。  
  
 `value`  
 中一个 `CLRDATA_ADDRESS` 值，该值指定要置于给定 TLS 位置的值。  
  
## <a name="remarks"></a>注解  

 此方法由调试应用程序的编写器实现。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** ClrData，ClrData  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICLRDataTarget 接口](iclrdatatarget-interface.md)
