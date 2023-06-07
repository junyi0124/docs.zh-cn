---
title: ICorDebugBlockingObjectEnum::Next 方法
ms.date: 03/30/2017
api_name:
- ICorDebugBlockingObjectEnum.Next Method
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugBlockingObjectEnum::Next
helpviewer_keywords:
- Next method, ICorDebugBlockingObjectEnum interface [.NET Framework debugging]
- ICorDebugBlockingObjectEnum::Next method [.NET Framework debugging]
ms.assetid: 0121753f-ebea-48d0-aeb2-ed7fda76dc60
topic_type:
- apiref
ms.openlocfilehash: 232068a5fee8f7bd3dfbddf4d9452e80d6fd6170
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95719185"
---
# <a name="icordebugblockingobjectenumnext-method"></a>ICorDebugBlockingObjectEnum::Next 方法

从当前位置开始，获取枚举中指定的 [CorDebugBlockingObject](cordebugblockingobject-structure.md) 对象数。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT Next([in] ULONG  celt,  
             [out, size_is(celt), length_is(*pceltFetched)]  
                           CorDebugBlockingObject values[],  
             [out] ULONG *pceltFetched;  
```  
  
## <a name="parameters"></a>参数  

 `celt`  
 中要检索的对象的数目。  
  
 `values`  
 弄指向 [CorDebugBlockingObject](cordebugblockingobject-structure.md) 对象的指针的数组。  
  
 `pceltFetched`  
 弄一个指针，指向已检索到的对象的数目。  
  
## <a name="return-value"></a>返回值  

 此方法会返回以下特定的 HRESULT。  
  
|HRESULT|说明|  
|-------------|-----------------|  
|S_OK|该方法已成功完成。|  
|S_FALSE|`pceltFetched` 不等于 `celt`。|  
  
## <a name="remarks"></a>注解  

 此方法的功能类似于典型的 COM 枚举器。  
  
 输入数组值必须至少为大小 `celt` 。 数组将用枚举中的下一个值填充， `celt` 如果小于保留，则用所有剩余值填充 `celt` 。 此方法返回时， `pceltFetched` 将用检索到的值的数目进行填充。 如果 `values` 包含无效指针或指向小于的缓冲区， `celt` 或者如果 `pceltFetched` 是无效指针，则结果是不确定的。  
  
> [!NOTE]
> 尽管不需要释放 [CorDebugBlockingObject](cordebugblockingobject-structure.md) 结构，但它内的 "ICorDebugValue" 接口需要释放。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICorDebugDataTarget 接口](icordebugdatatarget-interface.md)
- [调试接口](debugging-interfaces.md)
- [调试](index.md)
