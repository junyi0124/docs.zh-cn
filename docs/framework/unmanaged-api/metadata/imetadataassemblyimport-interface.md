---
title: IMetaDataAssemblyImport 接口
ms.date: 03/30/2017
api_name:
- IMetaDataAssemblyImport
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataAssemblyImport
helpviewer_keywords:
- IMetaDataAssemblyImport interface [.NET Framework metadata]
ms.assetid: 29c6fba5-4cea-417d-b484-7ed22ebff1c9
topic_type:
- apiref
ms.openlocfilehash: c556fe247754b363ece0c5dc60750068276ddcc4
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95714752"
---
# <a name="imetadataassemblyimport-interface"></a>IMetaDataAssemblyImport 接口

提供访问和检查程序集清单内容的方法。  
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|------------|-----------------|  
|[CloseEnum 方法](imetadataassemblyimport-closeenum-method.md)|释放到指定枚举器的句柄。|  
|[EnumAssemblyRefs 方法](imetadataassemblyimport-enumassemblyrefs-method.md)|获取一个接口指针，该指针指向包含 `mdAssemblyRef` 当前元数据范围内的程序集所引用的程序集的标记。|  
|[EnumExportedTypes 方法](imetadataassemblyimport-enumexportedtypes-method.md)|获取一个指向枚举数的接口指针，该枚举数包含 `mdExportedType` 当前元数据范围内的程序集引用的 COM 类型的标记。|  
|[EnumFiles 方法](imetadataassemblyimport-enumfiles-method.md)|获取一个接口指针，该指针指向一个枚举数，该枚举数包含 `mdFile` 当前元数据范围内的程序集所引用的文件的标记。|  
|[EnumManifestResources 方法](imetadataassemblyimport-enummanifestresources-method.md)|获取一个接口指针，该指针指向包含 `mdManifestResource` 当前元数据范围内的程序集所引用资源的标记的枚举数。|  
|[FindAssembliesByName 方法](imetadataassemblyimport-findassembliesbyname-method.md)|获取 `mdAssemblyRef` 具有指定名称的程序集的标记数组。|  
|[FindExportedTypeByName 方法](imetadataassemblyimport-findexportedtypebyname-method.md)|获取 `mdExportedType` 具有指定名称的 COM 类型的标记。|  
|[FindManifestResourceByName 方法](imetadataassemblyimport-findmanifestresourcebyname-method.md)|获取 `mdManifestResource` 具有指定名称的资源的标记。|  
|[GetAssemblyFromScope 方法](imetadataassemblyimport-getassemblyfromscope-method.md)|获取当前元数据范围内的程序集的标记。|  
|[GetAssemblyProps 方法](imetadataassemblyimport-getassemblyprops-method.md)|获取指定程序集的属性设置。|  
|[GetAssemblyRefProps 方法](imetadataassemblyimport-getassemblyrefprops-method.md)|获取指定标记的属性设置 `mdAssemblyRef` 。|  
|[GetExportedTypeProps 方法](imetadataassemblyimport-getexportedtypeprops-method.md)|获取指定 COM 类型的属性设置。|  
|[GetFileProps 方法](imetadataassemblyimport-getfileprops-method.md)|获取指定文件的属性设置。|  
|[GetManifestResourceProps 方法](imetadataassemblyimport-getmanifestresourceprops-method.md)|获取指定的清单资源的属性设置。|  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Cor  
  
 **库：** 用作 MsCorEE.dll 中的资源  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [元数据接口](metadata-interfaces.md)
- [IMetaDataAssemblyEmit 接口](imetadataassemblyemit-interface.md)
