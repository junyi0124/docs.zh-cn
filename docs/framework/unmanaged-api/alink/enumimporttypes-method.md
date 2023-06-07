---
title: EnumImportTypes 方法
ms.date: 03/30/2017
api_name:
- EnumImportTypes
- IALink.EnumImportTypes
api_location:
- alink.dll
api_type:
- COM
f1_keywords:
- EnumImportTypes
helpviewer_keywords:
- EnumImportTypes method
ms.assetid: 83a0e4e7-ec06-40cb-9b63-700b9695bb04
topic_type:
- apiref
ms.openlocfilehash: ca7c7570aff63aa328dddc0626648fa74397addc
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/23/2019
ms.locfileid: "74448733"
---
# <a name="enumimporttypes-method"></a>EnumImportTypes 方法

枚举每个范围中的每个类型。

## <a name="syntax"></a>语法

```cpp
HRESULT EnumImportTypes(
    HALINKENUM   hEnum,
    DWORD        dwMax,
    mdTypeDef    aTypeDefs[],
    DWORD*       pdwCount
) PURE;
```

## <a name="parameters"></a>参数

`hEnum`\
枚举器的句柄。

`dwMax`\
要检索的类型的最大数目。

`aTypeDefs`\
接收类型标记，而不是超过 `dwMax`。

`pdwCount`\
接收 `aTypeDefs`中类型的实际数量。

## <a name="return-value"></a>返回值

如果方法成功，则返回 S_OK。

## <a name="requirements"></a>要求

需要 alink

## <a name="see-also"></a>另请参阅

- [IALink 接口](ialink-interface.md)
- [IALink2 接口](ialink2-interface.md)
- [ALink API](index.md)
