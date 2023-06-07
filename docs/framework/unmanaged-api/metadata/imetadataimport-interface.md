---
title: IMetaDataImport 接口
ms.date: 03/30/2017
api_name:
- IMetaDataImport
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataImport
helpviewer_keywords:
- IMetaDataImport interface [.NET Framework metadata]
ms.assetid: 0adbbd35-5e8d-4fec-8268-dc70a07c5975
topic_type:
- apiref
ms.openlocfilehash: 0049db66d7a753488388c85e87e1f907db56c7cf
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95679086"
---
# <a name="imetadataimport-interface"></a>IMetaDataImport 接口

提供从可迁移可执行 (PE) 文件或其他源（如类型库或独立的运行时元数据二进制文件）导入和操作现有元数据的方法。  
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|------------|-----------------|  
|[CloseEnum 方法](imetadataimport-closeenum-method.md)|关闭具有指定句柄的枚举器。|  
|[CountEnum 方法](imetadataimport-countenum-method.md)|获取具有指定句柄的枚举器中的元素数。|  
|[EnumCustomAttributes 方法](imetadataimport-enumcustomattributes-method.md)|枚举与指定类型或成员关联的自定义特性定义标记的列表。|  
|[EnumEvents 方法](imetadataimport-enumevents-method.md)|枚举指定的 TypeDef 标记的事件定义标记。|  
|[EnumFields 方法](imetadataimport-enumfields-method.md)|枚举指定的 TypeDef 标记所引用的类型的 FieldDef 标记。|  
|[EnumFieldsWithName 方法](imetadataimport-enumfieldswithname-method.md)|枚举具有指定名称的指定类型的 FieldDef 标记。|  
|[EnumInterfaceImpls 方法](imetadataimport-enuminterfaceimpls-method.md)|枚举表示接口实现的 MethodDef 标记。|  
|[EnumMemberRefs 方法](imetadataimport-enummemberrefs-method.md)|枚举表示指定类型成员的 MemberRef 标记。|  
|[EnumMembers 方法](imetadataimport-enummembers-method.md)|枚举表示指定类型的成员的 MemberDef 标记。|  
|[EnumMembersWithName 方法](imetadataimport-enummemberswithname-method.md)|枚举表示具有指定名称的指定类型的成员的 MemberDef 标记。|  
|[EnumMethodImpls 方法](imetadataimport-enummethodimpls-method.md)|枚举表示指定类型的方法的 MethodBody 和 MethodDeclaration 标记。|  
|[EnumMethods 方法](imetadataimport-enummethods-method.md)|枚举表示指定类型的方法的 MethodDef 标记。|  
|[EnumMethodSemantics 方法](imetadataimport-enummethodsemantics-method.md)|枚举与指定方法相关的属性和属性更改事件。|  
|[EnumMethodsWithName 方法](imetadataimport-enummethodswithname-method.md)|枚举具有指定名称并且由指定的 TypeDef 标记所引用的类型定义的方法。|  
|[EnumModuleRefs 方法](imetadataimport-enummodulerefs-method.md)|枚举表示导入的模块的 ModuleRef 标记。|  
|[EnumParams 方法](imetadataimport-enumparams-method.md)|枚举 ParamDef 标记，这些标记表示指定的 MethodDef 标记所引用的方法的参数。|  
|[EnumPermissionSets 方法](imetadataimport-enumpermissionsets-method.md)|枚举指定的元数据范围内的对象的权限。|  
|[EnumProperties 方法](imetadataimport-enumproperties-method.md)|枚举 PropertyDef 标记，这些标记表示指定的 TypeDef 标记所引用的类型的属性。|  
|[EnumSignatures 方法](imetadataimport-enumsignatures-method.md)|枚举当前范围内表示独立签名的 Signature 标记。|  
|[EnumTypeDefs 方法](imetadataimport-enumtypedefs-method.md)|枚举表示当前范围内的所有类型的 TypeDef 标记。|  
|[EnumTypeRefs 方法](imetadataimport-enumtyperefs-method.md)|枚举当前元数据范围内定义的 TypeRef 标记。|  
|[EnumTypeSpecs 方法](imetadataimport-enumtypespecs-method.md)|枚举当前元数据范围内定义的 TypeSpec 标记。|  
|[EnumUnresolvedMethods 方法](imetadataimport-enumunresolvedmethods-method.md)|枚举表示当前元数据范围内未解析的方法的 MemberDef 标记。|  
|[EnumUserStrings 方法](imetadataimport-enumuserstrings-method.md)|枚举表示当前元数据范围内的硬编码字符串的 String 标记。|  
|[FindField 方法](imetadataimport-findfield-method.md)|获取属于指定类型的成员并具有指定的名称和元数据签名的字段的 FieldDef 标记。|  
|[FindMember 方法](imetadataimport-findmember-method.md)|获取一个指向成员的 MemberDef 标记的指针，该成员由指定的类型定义，并且具有指定的名称和元数据签名。|  
|[FindMemberRef 方法](imetadataimport-findmemberref-method.md)|获取一个指向成员的 MemberRef 标记的指针，该成员由指定的类型定义，并且具有指定的名称和元数据签名。|  
|[FindMethod 方法](imetadataimport-findmethod-method.md)|获取一个指向方法的 MethodDef 标记的指针，该方法由指定的类型定义，并且具有指定的名称和元数据签名。|  
|[FindTypeDefByName 方法](imetadataimport-findtypedefbyname-method.md)|获取一个指向具有指定名称的类型的 TypeDef 元数据标记的指针。|  
|[FindTypeRef 方法](imetadataimport-findtyperef-method.md)|获取一个指向 TypeRef 元数据标记的指针，该标记通过指定的名称在指定搜索范围内引用类型。|  
|[GetClassLayout 方法](imetadataimport-getclasslayout-method.md)|获取指定的 TypeDef 标记所引用类的布局信息。|  
|[GetCustomAttributeByName 方法](imetadataimport-getcustomattributebyname-method.md)|给定自定义特性的名称时，获取该特性的值。|  
|[GetCustomAttributeProps 方法](imetadataimport-getcustomattributeprops-method.md)|获取给定元数据标记的自定义属性的值。|  
|[GetEventProps 方法](imetadataimport-geteventprops-method.md)|对于指定的事件标记表示的事件，获取元数据信息，包括声明类型、委托的添加和删除方法、任何标志及其他关联数据。|  
|[GetFieldMarshal 方法](imetadataimport-getfieldmarshal-method.md)|获取一个指针，该指针指向由指定的字段元数据标记表示的字段的本机非托管类型。|  
|[GetFieldProps 方法](imetadataimport-getfieldprops-method.md)|获取与指定 FieldDef 标记引用的字段关联的元数据。|  
|[GetInterfaceImplProps 方法](imetadataimport-getinterfaceimplprops-method.md)|对于实现指定方法的类型和声明该方法的接口，获取一个指向其元数据标记的指针。|  
|[GetMemberProps 方法](imetadataimport-getmemberprops-method.md)|获取指定的元数据标记所引用的类型成员的元数据信息，包括名称、二进制签名和相对虚拟地址。|  
|[GetMemberRefProps 方法](imetadataimport-getmemberrefprops-method.md)|获取与指定标记引用的成员关联的元数据。|  
|[GetMethodProps 方法](imetadataimport-getmethodprops-method.md)|获取与指定的 MethodDef 标记引用的方法关联的元数据。|  
|[GetMethodSemantics 方法](imetadataimport-getmethodsemantics-method.md)|获取一个指针，该指针指向方法（由指定的 MethodDef 标记引用）与成对属性和事件（由指定的 EventProp 标记引用）之间的关系。|  
|[GetModuleFromScope 方法](imetadataimport-getmodulefromscope-method.md)|获取指向当前元数据范围内所引用模块的元数据标记的指针。|  
|[GetModuleRefProps 方法](imetadataimport-getmodulerefprops-method.md)|获取指定元数据标记引用的模块的名称。|  
|[GetNameFromToken 方法](imetadataimport-getnamefromtoken-method.md)|获取指定的元数据标记所引用的对象的 UTF-8 名称。|  
|[GetNativeCallConvFromSig 方法](imetadataimport-getnativecallconvfromsig-method.md)|获取指定的签名指针所表示的方法的本机调用约定。|  
|[GetNestedClassProps 方法](imetadataimport-getnestedclassprops-method.md)|获取指定嵌套类型的封闭父类型的 TypeDef 标记。|  
|[GetParamForMethodIndex 方法](imetadataimport-getparamformethodindex-method.md)|获取一个指向标记的指针，此标记表示在指定的 MethodDef 标记表示的方法的方法参数序列中位于指定序号位置的参数。|  
|[GetParamProps 方法](imetadataimport-getparamprops-method.md)|获取指定的 ParamDef 标记所引用的参数的元数据值。|  
|[GetPermissionSetProps 方法](imetadataimport-getpermissionsetprops-method.md)|获取与指定的 Permission 标记所表示的 System.Security.PermissionSet 关联的元数据。|  
|[GetPinvokeMap](imetadataimport-getpinvokemap-method.md)|获取用于表示 PInvoke 调用的目标程序集的 ModuleRef 标记。|  
|[GetPropertyProps 方法](imetadataimport-getpropertyprops-method.md)|获取与指定的标记表示的属性关联的元数据。|  
|[GetRVA 方法](imetadataimport-getrva-method.md)|获取由指定标记表示的代码对象的相对虚拟地址的偏移量。|  
|[GetScopeProps 方法](imetadataimport-getscopeprops-method.md)|获取当前元数据范围内的程序集或模块的名称和版本标识符（可选）。|  
|[GetSigFromToken 方法](imetadataimport-getsigfromtoken-method.md)|获取与指定标记关联的二进制元数据签名。|  
|[GetTypeDefProps 方法](imetadataimport-gettypedefprops-method.md)|返回指定 TypeDef 标记所表示类型的元数据信息。|  
|[GetTypeRefProps 方法](imetadataimport-gettyperefprops-method.md)|获取与指定的 TypeRef 标记所引用的类型关联的元数据。|  
|[GetTypeSpecFromToken 方法](imetadataimport-gettypespecfromtoken-method.md)|获取指定标记所表示的类型规范的二进制元数据签名。|  
|[GetUserString 方法](imetadataimport-getuserstring-method.md)|获取指定元数据标记所表示的文字字符串。|  
|[IsGlobal 方法](imetadataimport-isglobal-method.md)|获取一个值，该值指示由指定的元数据标记表示的字段、方法或类型是否具有全局范围。|  
|[IsValidToken 方法](imetadataimport-isvalidtoken-method.md)|获取指示指定的标记是否包含对代码对象的有效引用的值。|  
|[ResetEnum 方法](imetadataimport-resetenum-method.md)|将指定的枚举器重置到指定位置。|  
|[ResolveTypeRef 方法](imetadataimport-resolvetyperef-method.md)|获取指定的 TypeRef 标记所引用的类型的类型信息。|  
  
## <a name="remarks"></a>注解  

 `IMetaDataImport` 接口的设计主要供将要导入类型信息（例如，开发工具）或管理已部署组件（例如，解析/激活服务）的工具和服务使用。 `IMetaDataImport` 中的方法属于下列任务类别：  
  
- 枚举元数据范围内的项集合。  
  
- 查找具有特定特征集的项。  
  
- 获取指定项的属性。  
  
- Get 方法专门用来返回元数据项的单值属性。 当该属性是对另一个项的引用时，将返回该项的标记。 任何指针输入类型都可为 NULL，以指示未请求特定值。 若要获取基本上由集合对象（例如，某个类实现的接口集合）构成的属性，请使用枚举方法。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Cor  
  
 **库：** 用作 MsCorEE.dll 中的资源  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [元数据接口](metadata-interfaces.md)
- [IMetaDataImport2 接口](imetadataimport2-interface.md)
