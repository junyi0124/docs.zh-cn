---
title: IMetaDataTables::GetColumn 方法
ms.date: 02/25/2019
api_name:
- IMetaDataTables.GetColumn
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataTables::GetColumn
helpviewer_keywords:
- IMetaDataTables::GetColumn method [.NET Framework metadata]
- GetColumn method [.NET Framework metadata]
ms.assetid: 1032055b-cabb-45c5-a50e-7e853201b175
topic_type:
- apiref
ms.openlocfilehash: 270546f0270521e38cfdcae5e4d2137202c13cb1
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95711062"
---
# <a name="imetadatatablesgetcolumn-method"></a>IMetaDataTables::GetColumn 方法

获取一个指针，该指针指向给定表中指定列和行的单元中包含的值。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetColumn (
    [in]  ULONG   ixTbl,  
    [in]  ULONG   ixCol,  
    [in]  ULONG   rid,  
    [out] ULONG   *pVal  
);  
```  
  
## <a name="parameters"></a>参数

 `ixTbl`  
 中表的索引。  
  
 `ixCol`  
 中表中的列的索引。  
  
 `rid`  
 中表中的行的索引。  
  
 `pVal`  
 弄指向单元格中的值的指针。  

## <a name="remarks"></a>注解

返回的值的 interpretion `pVal` 取决于列的类型。 列类型可通过调用 [IMetaDataTables](imetadatatables-getcolumninfo-method.md)来确定。

- **GetColumn** 方法自动将 **Rid** 或 **CodedToken** 类型的列转换为完整的32位 `mdToken` 值。
- 它还会自动将8位或16位值转换为完整的32位值。
- 对于 *堆* 类型列，返回的 *pVal* 将是对应堆中的索引。

| 列类型              | pVal 包含 | 注释                          |
|--------------------------|---------------|-----------------------------------|
| `0`..`iRidMax`<br> (0. 63)   | mdToken     | *pVal* 将包含一个完整的令牌。 函数自动将 Rid 转换为完整的标记。 |
| `iCodedToken`..`iCodedTokenMax`<br> (64.. 95)  | mdToken | 返回后， *pVal* 将包含一个完整的令牌。 函数自动将 CodedToken 解压缩到完整的令牌中。 |
| `iSHORT` (96)             | Int16         | 自动将符号扩展为32位。  |
| `iUSHORT` (97)            | UInt16        | 自动将符号扩展为32位。  |
| `iLONG` (98)              | Int32         |                                        |
| `iULONG` (99)             | UInt32        |                                        |
| `iBYTE` (100)             | Byte          | 自动将符号扩展为32位。  |
| `iSTRING` (101)           | 字符串堆索引 | *pVal* 是字符串堆中的索引。 使用 [IMetadataTables：： GetString](imetadatatables-getstring-method.md) 获取实际的列字符串值。 |
| `iGUID` (102)             | Guid 堆索引 | *pVal* 是 Guid 堆中的索引。 使用 [IMetadataTables：： GetGuid](imetadatatables-getguid-method.md) 获取实际的列 Guid 值。 |
| `iBLOB` (103)             | Blob 堆索引 | *pVal* 是 Blob 堆中的索引。 使用 [IMetadataTables：： GetBlob](imetadatatables-getblob-method.md) 获取实际的列 Blob 值。 |
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Cor  
  
 **库：** 用作 MsCorEE.dll 中的资源  
  
 **.NET Framework 版本**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [IMetaDataTables 接口](imetadatatables-interface.md)
- [IMetaDataTables2 接口](imetadatatables2-interface.md)
