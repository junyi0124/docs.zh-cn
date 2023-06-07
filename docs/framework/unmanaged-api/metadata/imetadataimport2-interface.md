---
title: IMetaDataImport2 接口
ms.date: 03/30/2017
api_name:
- IMetaDataImport2
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataImport2
helpviewer_keywords:
- IMetaDataImport2 interface [.NET Framework metadata]
ms.assetid: d39b2b87-ba53-4771-ae53-952a68452511
topic_type:
- apiref
ms.openlocfilehash: a845ecfde6583d625d2a8f165443344ff9e40d05
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95702541"
---
# <a name="imetadataimport2-interface"></a>IMetaDataImport2 接口

扩展 [IMetaDataImport](imetadataimport-interface.md) 接口以提供使用泛型类型的功能。  
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|------------|-----------------|  
|[EnumGenericParamConstraints 方法](imetadataimport2-enumgenericparamconstraints-method.md)|获取与指定标记表示的泛型参数关联的泛型参数约束数组的枚举器。|  
|[EnumGenericParams 方法](imetadataimport2-enumgenericparams-method.md)|获取与指定的 TypeDef 或 MethodDef 标记相关联的泛型参数标记数组的枚举器。|  
|[EnumMethodSpecs 方法](imetadataimport2-enummethodspecs-method.md)|获取与指定的 MethodDef 或 MemberRef 标记相关联的 MethodSpec 标记数组的枚举器。|  
|[GetGenericParamConstraintProps 方法](imetadataimport2-getgenericparamconstraintprops-method.md)|获取与指定约束标记所表示的泛型参数约束关联的元数据。|  
|[GetGenericParamProps 方法](imetadataimport2-getgenericparamprops-method.md)|获取与指定标记表示的泛型参数关联的元数据。|  
|[GetMethodSpecProps 方法](imetadataimport2-getmethodspecprops-method.md)|获取指定的 MethodSpec 标记所引用的方法的元数据签名。|  
|[GetPEKind 方法](imetadataimport2-getpekind-method.md)|获取一个值，该值标识可移植可执行文件中代码的性质 (PE) 文件，通常是在当前元数据范围内定义的 DLL 或 EXE 文件|  
|[GetVersionString 方法](imetadataimport2-getversionstring-method.md)|获取用于生成程序集的运行时的版本号。|  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Cor  
  
 **库：** 用作 MsCorEE.dll 中的资源  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- <xref:System.Reflection.PortableExecutableKinds>
- [元数据接口](metadata-interfaces.md)
- [IMetaDataImport 接口](imetadataimport-interface.md)
