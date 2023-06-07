---
title: IMetaDataTables::GetNextGuid 方法
ms.date: 03/30/2017
api_name:
- IMetaDataTables.GetNextGuid
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataTables::GetNextGuid
helpviewer_keywords:
- GetNextGuid method [.NET Framework metadata]
- IMetaDataTables::GetNextGuid method [.NET Framework metadata]
ms.assetid: 68f6ea4d-9112-4d6b-93d9-e34f1e2f2496
topic_type:
- apiref
ms.openlocfilehash: 71dce539941f78feff3d5f89028d654cade25feb
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95727258"
---
# <a name="imetadatatablesgetnextguid-method"></a>IMetaDataTables::GetNextGuid 方法

获取当前表列中的下一个 GUID 值的索引。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetNextGuid (  
    [in]  ULONG   ixGuid,  
    [out] ULONG   *pNext  
);  
```  
  
## <a name="parameters"></a>参数  

 `ixGuid`  
 中来自 GUID 表列的索引值。  
  
 `pNext`  
 弄指向下一个 GUID 值的索引的指针。  
  
## <a name="remarks"></a>注解  

  不建议使用此方法，因为它不返回一致的结果。 有关 GUID 表的信息，请参阅公共语言基础结构 (CLI) 文档，尤其是 "第二部分：元数据定义和语义"。 文档在线提供;请参阅 [ECMA c # 和公共语言基础结构标准](../../../standard/components.md#applicable-standards) 和 [标准 ECMA-335-公共语言基础结构 (CLI) ](http://www.ecma-international.org/publications/standards/Ecma-335.htm)。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Cor  
  
 **库：** 用作 MsCorEE.dll 中的资源  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [IMetaDataTables 接口](imetadatatables-interface.md)
- [IMetaDataTables2 接口](imetadatatables2-interface.md)
