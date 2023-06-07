---
title: ICorDebugILCode::GetEHClauses 方法
ms.date: 03/30/2017
dev_langs:
- cpp
api_name:
- ICorDebugILCode.GetEHClauses
api_location:
- mscordbi.dll
api_type:
- COM
ms.assetid: cf7a0e00-06ae-47a5-8037-598b26196802
topic_type:
- apiref
ms.openlocfilehash: 38936a57944e9a0920c374f473c4cbe8e8d70abb
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95728662"
---
# <a name="icordebugilcodegetehclauses-method"></a>ICorDebugILCode::GetEHClauses 方法

[仅在 .NET Framework 4.5.2 及更高版本中受支持]  
  
 返回指向为此中间语言 (IL) 定义的异常处理 (EH) 子句列表的指针。  
  
## <a name="syntax"></a>语法  
  
```cpp
HRESULT GetEHClauses(  
   [in] ULONG32 cClauses,  
   [out] ULONG32 * pcClauses,  
   [out, size_is(cClauses), length_is(*pcClauses)] CorDebugEHClause clauses[]);  
```  
  
## <a name="parameters"></a>参数  

 `cClauses`  
 [in] `clauses` 数组的存储容量。 有关详细信息，请参阅备注部分。  
  
 `pcClauses`  
 [out] 有关哪些信息写入了 `clauses` 数组的子句数。  
  
 子句  
 弄一个 [CorDebugEHClause](cordebugehclause-structure.md) 对象数组，其中包含有关为此 IL 定义的异常处理子句的信息。  
  
## <a name="remarks"></a>注解  

 如果 `cClauses` 为0且 `pcClauses` 为非 **null**， `pcClauses` 则将设置为可用的异常处理子句数。 如果  为非零，则它表示  数组的存储容量。 当该方法返回时，`clauses` 将包含最大的 `cClauses` 项，并且 `pcClauses` 将设置为实际写入 `clauses` 数组的子句数。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v452plus](../../../../includes/net-current-v452plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICorDebugILCode 接口](icordebugilcode-interface.md)
- [CorDebugEHClause 结构](cordebugehclause-structure.md)
- [调试接口](debugging-interfaces.md)
