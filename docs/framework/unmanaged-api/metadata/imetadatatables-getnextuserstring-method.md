---
title: IMetaDataTables::GetNextUserString 方法
ms.date: 03/30/2017
api_name:
- IMetaDataTables.GetNextUserString
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataTables::GetNextUserString
helpviewer_keywords:
- GetNextUserString method [.NET Framework metadata]
- IMetaDataTables::GetNextUserString method [.NET Framework metadata]
ms.assetid: b7cb40ee-67b7-4f4e-8dcc-ee7ac8bc986b
topic_type:
- apiref
ms.openlocfilehash: bb53d980a7b2121854748d5117bc539fed125163
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95680360"
---
# <a name="imetadatatablesgetnextuserstring-method"></a>IMetaDataTables::GetNextUserString 方法

获取包含当前表列中的下一个硬编码字符串的行的索引。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetNextUserString (  
    [in]  ULONG   ixUserString,  
    [out] ULONG   *pNext  
);  
```  
  
## <a name="parameters"></a>参数  

 `ixUserString`  
 中当前字符串列中的索引值。  
  
 `pNext`  
 弄指向列中下一个字符串的行索引的指针。  
  
## <a name="remarks"></a>注解  

 不建议使用此方法，因为它不返回一致的结果。 有关 GUID 表的信息，请参阅公共语言基础结构 (CLI) 文档，尤其是 "第二部分：元数据定义和语义"。 文档在线提供;请参阅 [ECMA c # 和公共语言基础结构标准](../../../standard/components.md#applicable-standards) 和 [标准 ECMA-335-公共语言基础结构 (CLI) ](http://www.ecma-international.org/publications/standards/Ecma-335.htm)。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Cor  
  
 **库：** 用作 MsCorEE.dll 中的资源  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [IMetaDataTables 接口](imetadatatables-interface.md)
- [IMetaDataTables2 接口](imetadatatables2-interface.md)
