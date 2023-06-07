---
title: PFN_CLRDataCreateInstance 函数指针
ms.date: 03/30/2017
api_name:
- PFN_CLRDataCreateInstance
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- PFN_CLRDataCreateInstance
helpviewer_keywords:
- PFN_CLRDataCreateInstance function pointer [.NET Framework debugging]
ms.assetid: 5c66ac57-d751-4de5-af9f-26ceb949af8b
topic_type:
- apiref
ms.openlocfilehash: 68a5b8bb1568f10699653479357b02b2e847cc02
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95671960"
---
# <a name="pfn_clrdatacreateinstance-function-pointer"></a>PFN_CLRDataCreateInstance 函数指针

指向一个函数，该函数为指定的目标项创建接口对象。  
  
## <a name="syntax"></a>语法  
  
```cpp  
typedef HRESULT (STDAPICALLTYPE* PFN_CLRDataCreateInstance) (  
    [in]  REFIID           iid,  
    [in]  ICLRDataTarget  *target,  
    [out] void           **iface  
);  
```  
  
## <a name="parameters"></a>参数  

 `iid`  
 中要实例化的接口的标识符。  
  
 `target`  
 中一个指向用户实现的 [ICLRDataTarget](iclrdatatarget-interface.md) 对象的指针，该对象表示要为其创建 interface 对象的目标项。  
  
 `iface`  
 弄指向返回的接口对象地址的指针。  
  
## <a name="remarks"></a>注解  

 `ICLRDataTarget`对象由调试应用程序的编写器实现。 实现取决于所表示的目标项的类型。 目标项可以是进程、内存转储、远程计算机，等等。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** ClrData .idl  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [调试全局静态函数](debugging-global-static-functions.md)
