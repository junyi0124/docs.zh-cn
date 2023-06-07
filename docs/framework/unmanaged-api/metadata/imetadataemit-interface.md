---
title: IMetaDataEmit 接口
ms.date: 03/30/2017
api_name:
- IMetaDataEmit
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataEmit
helpviewer_keywords:
- IMetaDataEmit interface [.NET Framework metadata]
ms.assetid: 3b48fd47-7397-4e2c-8bec-8157aa08978c
topic_type:
- apiref
ms.openlocfilehash: f5495c170abf3e991a6e28016687f8ae77f0b423
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95722019"
---
# <a name="imetadataemit-interface"></a>IMetaDataEmit 接口

提供用于创建、修改和保存有关当前定义的范围中的程序集的元数据的方法。 元数据可存储在内存中或保存到磁盘。  
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|------------|-----------------|  
|[ApplyEditAndContinue 方法](imetadataemit-applyeditandcontinue-method.md)|用指定中的更改更新当前程序集范围 `pImport` 。|  
|[DefineCustomAttribute 方法](imetadataemit-definecustomattribute-method.md)|使用指定的元数据签名创建要附加到指定对象的自定义属性的定义，并获取该自定义属性定义的标记。|  
|[DefineEvent 方法](imetadataemit-defineevent-method.md)|使用指定的元数据签名创建事件的定义，并获取该事件定义的标记。|  
|[DefineField 方法](imetadataemit-definefield-method.md)|使用指定的元数据签名创建字段的定义，并获取该字段定义的标记。|  
|[DefineImportMember 方法](imetadataemit-defineimportmember-method.md)|为当前范围外的模块中定义的类型的成员创建定义，并获取该引用定义的标记。|  
|[DefineImportType 方法](imetadataemit-defineimporttype-method.md)|创建对在当前范围外的模块中定义的类型的引用的定义，并获取该引用定义的标记。|  
|[DefineMemberRef 方法](imetadataemit-definememberref-method.md)|创建对当前范围之外的模块成员的引用的定义，并获取该引用定义的标记。|  
|[DefineMethod 方法](imetadataemit-definemethod-method.md)|使用指定的签名为方法创建定义，并将标记返回到该方法定义。|  
|[DefineMethodImpl 方法](imetadataemit-definemethodimpl-method.md)|创建一个定义，用于实现从接口继承的方法，并返回该方法实现定义的标记。|  
|[DefineModuleRef 方法](imetadataemit-definemoduleref-method.md)|创建具有指定名称的模块的元数据签名。|  
|[DefineNestedType 方法](imetadataemit-definenestedtype-method.md)|创建类型定义的元数据签名并返回 `mdTypeDef` 该类型的标记，此外，还指定该定义类型是所引用类型的成员 `tdEncloser` 。|  
|[DefineParam 方法](imetadataemit-defineparam-method.md)|创建具有指定标记所引用的方法的指定签名的参数定义，并获取该参数定义的标记。|  
|[DefinePermissionSet 方法](imetadataemit-definepermissionset-method.md)|使用指定的元数据签名创建权限集的定义，并获取该权限集定义的标记。|  
|[DefinePinvokeMap 方法](imetadataemit-definepinvokemap-method.md)|设置指定的标记所引用的方法的 PInvoke 签名功能。|  
|[DefineProperty 方法](imetadataemit-defineproperty-method.md)|使用指定的和方法访问器创建指定类型的属性定义， `get` `set` 并获取该属性定义的标记。|  
|[DefineSecurityAttributeSet 方法](imetadataemit-definesecurityattributeset-method.md)|创建一组安全权限，以附加到指定标记所引用的对象。|  
|[DefineTypeDef 方法](imetadataemit-definetypedef-method.md)|创建公共语言运行时类型的类型定义，并获取该类型定义的元数据标记。|  
|[DefineTypeRefByName 方法](imetadataemit-definetyperefbyname-method.md)|获取在当前范围之外的另一个模块中定义的类型的元数据标记。|  
|[DefineUserString 方法](imetadataemit-defineuserstring-method.md)|获取指定文本字符串的元数据标记。|  
|[DeleteClassLayout 方法](imetadataemit-deleteclasslayout-method.md)|销毁指定标记所引用的类型的类布局元数据签名。|  
|[DeleteFieldMarshal 方法](imetadataemit-deletefieldmarshal-method.md)|销毁指定标记所引用的对象的 PInvoke 封送元数据签名。|  
|[DeletePinvokeMap 方法](imetadataemit-deletepinvokemap-method.md)|销毁指定标记所引用的对象的 PInvoke 映射元数据。|  
|[DeleteToken 方法](imetadataemit-deletetoken-method.md)|从当前的元数据范围中删除指定的标记。|  
|[GetSaveSize 方法](imetadataemit-getsavesize-method.md)|获取当前范围内的程序集的预计二进制大小。|  
|[GetTokenFromSig 方法](imetadataemit-gettokenfromsig-method.md)|获取指定元数据签名的令牌。|  
|[GetTokenFromTypeSpec 方法](imetadataemit-gettokenfromtypespec-method.md)|获取具有指定元数据签名的类型的元数据标记。|  
|[Merge 方法](imetadataemit-merge-method.md)|将指定的导入范围添加到要合并的范围列表。|  
|[MergeEnd 方法](imetadataemit-mergeend-method.md)|合并到当前范围中由一个或多个之前的调用指定的所有元数据范围 `IMetaDataEmit::Merge` 。|  
|[Save 方法](imetadataemit-save-method.md)|将当前范围中的所有元数据保存到指定地址处的文件。|  
|[SaveToMemory 方法](imetadataemit-savetomemory-method.md)|将当前范围中的所有元数据保存到指定的内存区域。|  
|[SaveToStream 方法](imetadataemit-savetostream-method.md)|将当前范围中的所有元数据保存到指定的 `IStream` 。|  
|[SetClassLayout 方法](imetadataemit-setclasslayout-method.md)|设置或更新通过之前对的调用定义的类型的类布局签名 `IMetaDataEmit::DefineTypeDef` 。|  
|[SetCustomAttributeValue 方法](imetadataemit-setcustomattributevalue-method.md)|设置或更新通过之前对的调用定义的自定义特性的值 `IMetaDataEmit::DefineCustomAttribute` 。|  
|[SetEventProps 方法](imetadataemit-seteventprops-method.md)|设置或更新通过之前对的调用定义的事件的指定功能 `IMetaDataEmit::DefineEvent` 。|  
|[SetFieldMarshal 方法](imetadataemit-setfieldmarshal-method.md)|为指定的标记引用的字段、方法返回或方法参数设置 PInvoke 封送处理信息。|  
|[SetFieldProps 方法](imetadataemit-setfieldprops-method.md)|设置或更新指定字段标记所引用的字段的默认值。|  
|[SetFieldRVA 方法](imetadataemit-setfieldrva-method.md)|为指定的标记所引用的字段的相对虚拟地址设置全局变量值。|  
|[SetHandler 方法](imetadataemit-sethandler-method.md)|将指定指针所引用的方法设置 `IUnknown` 为标记重新映射的通知回调。|  
|[SetMethodImplFlags 方法](imetadataemit-setmethodimplflags-method.md)|设置或更新指定的标记所引用的继承方法实现的元数据签名。|  
|[SetMethodProps 方法](imetadataemit-setmethodprops-method.md)|设置或更新通过之前对的调用所定义的方法的特性（存储在指定的相对虚拟地址上） `IMetaDataEmit::DefineMethod` 。|  
|[SetModuleProps 方法](imetadataemit-setmoduleprops-method.md)|更新对先前调用所定义的模块的引用 `IMetaDataEmit::DefineModuleRef` 。|  
|[SetParamProps 方法](imetadataemit-setparamprops-method.md)|设置或更改通过先前对的调用所定义的方法参数的功能 `IMetaDataEmit::DefineParam` 。|  
|[SetParent 方法](imetadataemit-setparent-method.md)|确定由先前对的调用所定义的指定成员是否为 `IMetaDataEmit::DefineMemberRef` 指定类型的成员，该成员由之前对的调用定义 `IMetaDataEmit::DefineTypeDef` 。|  
|[SetPermissionSetProps 方法](imetadataemit-setpermissionsetprops-method.md)|设置或更新通过先前对的调用所定义的权限集的元数据签名的功能 `IMetaDataEmit::DefinePermissionSet` 。|  
|[SetPinvokeMap 方法](imetadataemit-setpinvokemap-method.md)|设置或更改方法的 PInvoke 签名的功能，由先前对的调用定义 `IMetaDataEmit::DefinePinvokeMap` 。|  
|[SetPropertyProps 方法](imetadataemit-setpropertyprops-method.md)|设置由先前对的调用所定义的属性的元数据中存储的功能 `IMetaDataEmit::DefineProperty` 。|  
|[SetRVA 方法](imetadataemit-setrva-method.md)|设置指定方法的相对虚拟地址。|  
|[SetTypeDefProps 方法](imetadataemit-settypedefprops-method.md)|设置由先前对的调用所定义的类型的功能 `IMetaDataEmit::DefineTypeDef` 。|  
|[TranslateSigWithScope 方法](imetadataemit-translatesigwithscope-method.md)|将程序集导入到当前作用域中，并为合并的作用域获取新的元数据签名。|  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Cor  
  
 **库：** 用作 MsCorEE.dll 中的资源  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [元数据接口](metadata-interfaces.md)
- [IMetaDataEmit2 接口](imetadataemit2-interface.md)
