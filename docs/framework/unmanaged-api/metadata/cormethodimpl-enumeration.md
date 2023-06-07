---
title: CorMethodImpl 枚举
ms.date: 03/30/2017
api_name:
- CorMethodImpl
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- CorMethodImpl
helpviewer_keywords:
- CorMethodImpl enumeration [.NET Framework metadata]
ms.assetid: ffbb3caf-20da-4a4b-8983-77376e72b990
topic_type:
- apiref
ms.openlocfilehash: 40e82997e58292a10f5e960cc9d9785d9ea8946a
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95676967"
---
# <a name="cormethodimpl-enumeration"></a>CorMethodImpl 枚举

包含一些值，用于描述方法实现功能。  
  
## <a name="syntax"></a>语法  
  
```cpp  
typedef enum CorMethodImpl {  
  
    miCodeTypeMask      =   0x0003,  
    miIL                =   0x0000,  
    miNative            =   0x0001,  
    miOPTIL             =   0x0002,  
    miRuntime           =   0x0003,  
  
    miManagedMask       =   0x0004,  
    miUnmanaged         =   0x0004,  
    miManaged           =   0x0000,  
  
    miForwardRef        =   0x0010,  
    miPreserveSig       =   0x0080,  
  
    miInternalCall      =   0x1000,  
    miSynchronized      =   0x0020,  
    miNoInlining        =   0x0008,  
    miAggressiveInlining =  0x0100,  
    miNoOptimization     =  0x0040,  
    miMaxMethodImplVal  =   0xffff  
  
} CorMethodImpl;  
```  
  
## <a name="members"></a>成员  
  
|成员|说明|  
|------------|-----------------|  
|`miCodeTypeMask`|描述代码类型的标志。|  
|`miIL`|指定方法实现为 Microsoft 中间语言 (MSIL) 。|  
|`miNative`|指定方法实现为本机。|  
|`miOPTIL`|指定方法实现为 OPTIL。|  
|`miRuntime`|指定方法实现由公共语言运行时提供。|  
|`miManagedMask`|指示代码是托管代码还是非托管代码的标志。|  
|`miUnmanaged`|指定方法实现是不受管理的。|  
|`miManaged`|指定方法实现是托管的。|  
|`miForwardRef`|指定定义方法。 此标志主要用于合并方案。|  
|`miPreserveSig`|指定方法签名不能用于 HRESULT 转换。|  
|`miInternalCall`|保留供公共语言运行时内部使用。|  
|`miSynchronized`|指定方法是通过其主体单线程的。|  
|`miNoInlining`|指定方法不能内联。|  
|`miAggressiveInlining`|指定方法应尽可能内联。|  
|`miNoOptimization`|指定不应优化方法。|  
|`miMaxMethodImplVal`|的最大有效值 `CorMethodImpl` 。|  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Corhdr。h  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [元数据枚举](metadata-enumerations.md)
