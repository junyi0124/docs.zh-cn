---
title: IMetaDataEmit::DefineImportMember 方法
ms.date: 03/30/2017
api_name:
- IMetaDataEmit.DefineImportMember
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataEmit::DefineImportMember
helpviewer_keywords:
- DefineImportMember method [.NET Framework metadata]
- IMetaDataEmit::DefineImportMember method [.NET Framework metadata]
ms.assetid: c7dd94c6-335b-46ff-9dfe-505056db5673
topic_type:
- apiref
ms.openlocfilehash: 60210bc8f93294c3c3380c36096f3e80e5b26643
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95723254"
---
# <a name="imetadataemitdefineimportmember-method"></a>IMetaDataEmit::DefineImportMember 方法

创建对在当前范围外定义的类型或模块的指定成员的引用，并定义该引用的标记。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT DefineImportMember (
    [in]  IMetaDataAssemblyImport  *pAssemImport,
    [in]  const void               *pbHashValue,
    [in]  ULONG                    cbHashValue,  
    [in]  IMetaDataImport          *pImport,
    [in]  mdToken                  mbMember,
    [in]  IMetaDataAssemblyEmit    *pAssemEmit,
    [in]  mdToken                  tkParent,
    [out] mdMemberRef              *pmr
);  
```  
  
## <a name="parameters"></a>参数  

 `pAssemImport`  
 中一个 [IMetaDataAssemblyImport](imetadataassemblyimport-interface.md) 接口，它表示从中导入目标成员的程序集。  
  
 `pbHashValue`  
 中一个数组，其中包含由指定的程序集的哈希 `pAssemImport` 。  
  
 `cbHashValue`  
 [in] `pbHashValue` 数组中的字节数。  
  
 `pImport`  
 中一个 [IMetaDataImport](imetadataimport-interface.md) 接口，表示从其导入目标成员的元数据范围。  
  
 `mbMember`  
 中指定目标成员的元数据标记。 标记可以是 `mdMethodDef` 成员方法的 () ， `mdProperty` (成员属性) ，或 (`mdFieldDef` 标记成员字段的) 。  
  
 `pAssemEmit`  
 中一个 [IMetaDataAssemblyEmit](imetadataassemblyemit-interface.md) 接口，该接口表示向其中导入目标成员的程序集。  
  
 `tkParent`  
 中 `mdTypeRef` `mdModuleRef` 类型或模块的或标记，分别拥有目标成员。  
  
 `pmr`  
 弄 `mdMemberRef` 在成员引用的当前范围内定义的标记。  
  
## <a name="remarks"></a>注解  

 `DefineImportMember`方法查找由指定的成员，该成员 `mbMember` 在指定的另一个范围内定义， `pImport` 并检索其属性。 它使用此信息调用当前范围中的 [IMetaDataEmit：:D efinememberref](imetadataemit-definememberref-method.md) 方法来创建成员引用。  
  
 通常，在使用方法之前， `DefineImportMember` 必须在当前作用域内为目标成员的父类、接口或模块创建类型引用或模块引用。 然后，在参数中传递此引用的元数据标记 `tkParent` 。 如果目标成员的父成员稍后将由编译器或链接器解析，则不需要创建对它的引用。 总结：  
  
- 如果目标成员是字段或方法，则使用 [IMetaDataEmit：:D efinetyperefbyname](imetadataemit-definetyperefbyname-method.md) 或 [IMetaDataEmit：:D efineimporttype](imetadataemit-defineimporttype-method.md) 方法为成员的父类或父接口创建类型引用（在当前范围内）。  
  
- 如果目标成员是全局变量或全局函数 (也就是说，不是类或接口的成员) ，请使用 [IMetaDataEmit：:D efinemoduleref](imetadataemit-definemoduleref-method.md) 方法为成员的父模块创建模块引用（在当前范围内）。  
  
- 如果目标成员的父项稍后将由编译器或链接器解析，则传入 `mdTokenNil` `tkParent` 。 这种情况的唯一应用场景是从一个 .obj 文件中导入全局函数或全局变量，该文件最终将链接到当前模块和合并的元数据中。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Cor  
  
 **库：** 用作 MSCorEE.dll 中的资源  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [IMetaDataEmit 接口](imetadataemit-interface.md)
- [IMetaDataEmit2 接口](imetadataemit2-interface.md)
