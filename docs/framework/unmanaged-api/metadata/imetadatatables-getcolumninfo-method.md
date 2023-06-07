---
title: IMetaDataTables::GetColumnInfo 方法
ms.date: 10/10/2019
api_name:
- IMetaDataTables.GetColumnInfo
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataTables::GetColumnInfo
helpviewer_keywords:
- IMetaDataTables::GetColumnInfo method [.NET Framework metadata]
- GetColumnInfo method [.NET Framework metadata]
ms.assetid: 68c160ea-ae7d-4750-985d-a038b2c8e7d9
topic_type:
- apiref
ms.openlocfilehash: 227d9ab67ab3091508232be3018ca520a6b5dcc6
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95711047"
---
# <a name="imetadatatablesgetcolumninfo-method"></a>IMetaDataTables::GetColumnInfo 方法

获取有关指定表中指定列的数据。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetColumnInfo (
    [in]  ULONG        ixTbl,  
    [in]  ULONG        ixCol,  
    [out] ULONG        *poCol,  
    [out] ULONG        *pcbCol,  
    [out] ULONG        *pType,  
    [out] const char   **ppName  
);  
```  
  
## <a name="parameters"></a>参数

=======

 `ixTbl`  
 中所需表的索引。  
  
 `ixCol`  
 中所需列的索引。  
  
 `poCol`  
 弄指向行中列的偏移量的指针。  
  
 `pcbCol`  
 弄一个指针，指向列的大小（以字节为单位）。  
  
 `pType`  
 弄一个指针，指向列中值的类型。  
  
 `ppName`  
 弄指向列名的指针的指针。  

## <a name="remarks"></a>注解

返回的列类型位于值的范围内：

| pType                    | 说明   | Helper 函数                   |
|--------------------------|---------------|-----------------------------------|
| `0`..`iRidMax`<br> (0. 63)    | 去掉           | **IsRidType**<br>**IsRidOrToken** |
| `iCodedToken`..`iCodedTokenMax`<br> (64.. 95)  | 编码标记 | **IsCodedTokenType** <br>**IsRidOrToken** |
| `iSHORT` (96)             | Int16         | **IsFixedType**                   |
| `iUSHORT` (97)            | UInt16        | **IsFixedType**                   |
| `iLONG` (98)              | Int32         | **IsFixedType**                   |
| `iULONG` (99)             | UInt32        | **IsFixedType**                   |
| `iBYTE` (100)             | Byte          | **IsFixedType**                   |
| `iSTRING` (101)           | String        | **IsHeapType**                    |
| `iGUID` (102)             | GUID          | **IsHeapType**                    |
| `iBLOB` (103)             | Blob          | **IsHeapType**                    |

存储在 *堆* 中的值 (即， `IsHeapType == true` 可以使用读取) ：

- `iSTRING`： **IMetadataTables. GetString**
- `iGUID`： **IMetadataTables. GetGUID**
- `iBLOB`： **IMetadataTables. GetBlob**

> [!IMPORTANT]
> 若要使用上表中定义的常量，请包含 `#define _DEFINE_META_DATA_META_CONSTANTS` *cor* 头文件提供的指令。

## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Cor  
  
 **库：** 用作 MsCorEE.dll 中的资源  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [IMetaDataTables 接口](imetadatatables-interface.md)
- [IMetaDataTables2 接口](imetadatatables2-interface.md)
