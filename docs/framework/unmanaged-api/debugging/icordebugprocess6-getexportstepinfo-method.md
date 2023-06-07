---
title: ICorDebugProcess6::GetExportStepInfo 方法
ms.date: 03/30/2017
ms.assetid: a927e0ac-f110-426d-bbec-9377a29c8f17
ms.openlocfilehash: e2c04672e51ffb16043b14735cd5375073194c27
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95732614"
---
# <a name="icordebugprocess6getexportstepinfo-method"></a>ICorDebugProcess6::GetExportStepInfo 方法

提供运行时导出功能信息以帮助单步调试托管代码。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetExportStepInfo(  
    [in] LPCWSTR pszExportName,
    [out] CorDebugCodeInvokeKind* pInvokeKind,
    [out] CorDebugCodeInvokePurpose* pInvokePurpose);  
```  
  
## <a name="parameters"></a>参数  

 psz导出名  
 [输入] 如 PE 导出表中所写的运行时间导出函数名。  
  
 调用类型  
 弄一个指针，指向 [CorDebugCodeInvokeKind](cordebugcodeinvokekind-enumeration.md) 枚举的成员，该枚举描述导出函数将如何调用托管代码。  
  
 调用目的  
 弄指向 [CorDebugCodeInvokePurpose](cordebugcodeinvokepurpose-enumeration.md) 枚举的成员的指针，该成员描述导出函数将调用托管代码的原因。  
  
## <a name="return-value"></a>返回值  

 该方法可以返回下表所列的值。  
  
|返回值|说明|  
|------------------|-----------------|  
|`S_OK`|方法调用成功。|  
|`E_POINTER`|`pInvokeKind` 或 `pInvokePurpose` 为 **null**。|  
|其他失败 `HRESULT` 值。|根据需要。|  
  
## <a name="remarks"></a>注解  
  
> [!NOTE]
> 此方法仅适用于 .NET Native。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_46_native](../../../../includes/net-46-native-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [“ICor调试进程6”接口](icordebugprocess6-interface.md)
- [调试接口](debugging-interfaces.md)
