---
title: IMetaDataDispenser::DefineScope 方法
ms.date: 03/30/2017
api_name:
- IMetaDataDispenser.DefineScope
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataDispenser::DefineScope
helpviewer_keywords:
- DefineScope method [.NET Framework metadata]
- IMetaDataDispenser::DefineScope method [.NET Framework metadata]
ms.assetid: af28db02-29af-45ac-aec6-8d6c6123c2ff
topic_type:
- apiref
ms.openlocfilehash: 87a39350986cb7bb62f76b0d9a6a9aae8f82e2f9
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95726088"
---
# <a name="imetadatadispenserdefinescope-method"></a>IMetaDataDispenser::DefineScope 方法

在内存中创建一个新区域，您可以在其中创建新的元数据。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT DefineScope (  
    [in]  REFCLSID    rclsid,  
    [in]  DWORD       dwCreateFlags,  
    [in]  REFIID      riid,
    [out] IUnknown    **ppIUnk  
);  
```  
  
## <a name="parameters"></a>参数  

 `rclsid`  
 中要创建的元数据结构版本的 CLSID。 对于 .NET Framework 版本2.0，此值必须为 CLSID_CorMetaDataRuntime。  
  
 `dwCreateFlags`  
 中指定选项的标志。 对于 .NET Framework 2.0，此值必须为零。  
  
 `riid`  
 中要返回的所需元数据接口的 IID;调用方将使用接口创建新的元数据。  
  
 的值 `riid` 必须指定一个 "发出" 接口。 有效值为 IID_IMetaDataEmit、IID_IMetaDataAssemblyEmit 或 IID_IMetaDataEmit2。  
  
 `ppIUnk`  
 弄指向返回的接口的指针。  
  
## <a name="remarks"></a>注解  

 `DefineScope` 创建一组内存中的元数据表，为元数据生成一个唯一的 GUID (模块版本标识符或 MVID) ，并在模块表中为发出的编译单元创建一个条目。  
  
 根据需要，可以使用 [IMetaDataEmit：： SetModuleProps](imetadataemit-setmoduleprops-method.md) 或 [IMetaDataEmit：:D efinecustomattribute](imetadataemit-definecustomattribute-method.md) 方法，将属性作为一个整体附加到元数据范围。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Cor  
  
 **库：** 用作 MsCorEE.dll 中的资源  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [IMetaDataDispenser 接口](imetadatadispenser-interface.md)
- [IMetaDataDispenserEx 接口](imetadatadispenserex-interface.md)
- [IMetaDataAssemblyEmit 接口](imetadataassemblyemit-interface.md)
- [IMetaDataEmit 接口](imetadataemit-interface.md)
- [IMetaDataEmit2 接口](imetadataemit2-interface.md)
