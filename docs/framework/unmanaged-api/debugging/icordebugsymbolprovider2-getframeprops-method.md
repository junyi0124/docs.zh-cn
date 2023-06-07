---
title: ICorDebugSymbolProvider2::GetFrameProps 方法
ms.date: 03/30/2017
ms.assetid: f07b73f3-188d-43a9-8f7d-44dce2f1ddb7
ms.openlocfilehash: ba1fd104c35b6e6dfdfd771f71eb19f8d532a1d6
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95672006"
---
# <a name="icordebugsymbolprovider2getframeprops-method"></a>ICorDebugSymbolProvider2::GetFrameProps 方法

返回启动方法相对虚拟地址的方法和具有代码相对虚拟地址的父框架。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetFrameProps(  
   [in] ULONG32 codeRva,  
   [out] ULONG32 *pCodeStartRva,  
   [out] ULONG32 *pParentFrameStartRva  
);  
```  
  
## <a name="parameters"></a>参数  

 `codeRva`  
 [in] 代码相对虚拟地址。  
  
 `pCodeStartRva`  
 [out] 指向方法的启动相对虚拟地址的指针。  
  
 `pParentFrameStartRva`  
 [out] 指向框架的启动相对虚拟地址的指针。  
  
## <a name="remarks"></a>注解  
  
> [!NOTE]
> 此方法仅适用于 .NET Native。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_46_native](../../../../includes/net-46-native-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICorDebugSymbolProvider2 接口](icordebugsymbolprovider2-interface.md)
- [调试接口](debugging-interfaces.md)
