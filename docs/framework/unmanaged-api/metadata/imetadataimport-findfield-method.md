---
title: IMetaDataImport::FindField 方法
ms.date: 03/30/2017
api_name:
- IMetaDataImport.FindField
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataImport::FindField
helpviewer_keywords:
- IMetaDataImport::FindField method [.NET Framework metadata]
- FindField method [.NET Framework metadata]
ms.assetid: 38cd4e16-fbb2-471c-aa73-ac51a1931ad2
topic_type:
- apiref
ms.openlocfilehash: 9b42f0f7c8e2878ee3ec140344f51517a24247c4
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95729858"
---
# <a name="imetadataimportfindfield-method"></a>IMetaDataImport::FindField 方法

获取一个指针，该指针指向由指定的括起来 <xref:System.Type> 且具有指定名称和元数据签名的字段的 FieldDef 标记。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT FindField (  
   [in]  mdTypeDef         td,  
   [in]  LPCWSTR           szName,  
   [in]  PCCOR_SIGNATURE   pvSigBlob,  
   [in]  ULONG             cbSigBlob,  
   [out] mdFieldDef        *pmb  
);  
```  
  
## <a name="parameters"></a>参数  

 `td`  
 中包含要搜索的字段的类或接口的 TypeDef 标记。 如果此值为 `mdTokenNil` ，则对全局变量执行查找。  
  
 `szName`  
 中要搜索的字段的名称。  
  
 `pvSigBlob`  
 中指向字段的二进制元数据签名的指针。  
  
 `cbSigBlob`  
 中的大小（以字节为单位） `pvSigBlob` 。  
  
 `pmb`  
 弄指向匹配的 FieldDef 标记的指针。  
  
## <a name="remarks"></a>注解  

 您可以使用其封闭类或接口指定字段 (`td`) ，其名称 (`szName`) ，还可以指定其签名 (`pvSigBlob`) 。  
  
 传递给的签名 `FindField` 必须已在当前范围内生成，因为签名将绑定到特定范围。 签名可以嵌入标识封闭类或值类型的标记。  (令牌是) 本地 TypeDef 表中的索引。 不能在当前范围的上下文之外生成运行时签名，并将该签名用作的输入 `FindField` 。  
  
 `FindField` 仅查找直接在类或接口中定义的字段;它不会找到继承的字段。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Cor  
  
 **库：** 作为中的资源包含 MsCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [IMetaDataImport 接口](imetadataimport-interface.md)
- [IMetaDataImport2 接口](imetadataimport2-interface.md)
