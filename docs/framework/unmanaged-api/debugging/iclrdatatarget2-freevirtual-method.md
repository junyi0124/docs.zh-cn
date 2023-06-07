---
title: ICLRDataTarget2::FreeVirtual 方法
ms.date: 03/30/2017
api_name:
- ICLRDataTarget2.FreeVirtual
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICLRDataTarget2::FreeVirtual
helpviewer_keywords:
- ICLRDataTarget::FreeVirtual method [.NET Framework debugging]
- FreeVirtual method [.NET Framework debugging]
ms.assetid: 26fb69f8-1467-4711-bd24-cb117c63938f
topic_type:
- apiref
ms.openlocfilehash: 1fb701a40abe2dc6e51443837c07ee5ba05ddfbe
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95723644"
---
# <a name="iclrdatatarget2freevirtual-method"></a>ICLRDataTarget2::FreeVirtual 方法

由公共语言运行时 (CLR) 数据访问服务调用，以释放以前在目标进程的地址空间中分配的内存。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT FreeVirtual(  
    [in] CLRDATA_ADDRESS addr,  
    [in] ULONG32 size,  
    [in] ULONG32 typeFlags  
);  
```  
  
## <a name="parameters"></a>参数  

 `addr`  
 中一个 `CLRDATA_ADDRESS` 值，该值指定要释放的内存的起始地址。  
  
 `size`  
 中要释放的内存的大小（以字节为单位）。  
  
 `typeFlags`  
 中控制释放内存的标志。 请参阅 Win32 `VirtualFree` 函数。  
  
## <a name="remarks"></a>注解  

 `FreeVirtual`方法用作 Win32 函数的逻辑包装 `VirtualFree` 。  
  
 此方法由调试应用程序的编写器实现。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** ClrData，ClrData  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICLRDataTarget2 接口](iclrdatatarget2-interface.md)
- [AllocVirtual 方法](iclrdatatarget2-allocvirtual-method.md)
