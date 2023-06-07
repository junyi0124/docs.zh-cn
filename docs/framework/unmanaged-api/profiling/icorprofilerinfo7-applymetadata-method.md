---
title: ICorProfilerInfo7：： ApplyMetaData 方法
ms.date: 02/15/2019
dev_langs:
- cpp
api_name:
- ICorProfilerInfo7.ApplyMetaData
api_location:
- mscorwks.dll
api_type:
- COM
ms.assetid: a1bfb649-4584-4d35-b3e6-8fe59b53992a
ms.openlocfilehash: 2c71db25422740880d8b29576eff247d5eba5f1d
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95686106"
---
# <a name="icorprofilerinfo7applymetadata-method"></a>ICorProfilerInfo7：： ApplyMetaData 方法

[仅在 .NET Framework 4.6.1 及更高版本中受支持]  
  
 将方法的新定义的元数据应用于 `IMetadataEmit::Define*` 指定的模块。  
  
## <a name="syntax"></a>语法  
  
```cpp
HRESULT ApplyMetaData(  
        [in] ModuleID moduleID  
);  
```  
  
## <a name="parameters"></a>参数  

 `moduleID`  
 中已更改其元数据的模块的标识符。  
  
## <a name="remarks"></a>注解  

 如果在 [ModuleLoadFinished](icorprofilercallback-moduleloadfinished-method.md) 回调之后进行元数据更改，则必须在使用新元数据之前调用此方法。  
  
 `ApplyMetaData` 仅支持添加以下类型的元数据：  
  
- `AssemblyRef` 通过调用 [IMetaDataAssemblyEmit：:D efineassemblyref](../metadata/imetadataassemblyemit-defineassemblyref-method.md)创建的记录。 方法。  
  
- `TypeRef` 通过调用 [IMetaDataEmit：:D efinetyperefbyname](../metadata/imetadataemit-definetyperefbyname-method.md) 方法创建的记录。  
  
- `TypeSpec` 通过调用 [IMetaDataEmit：： GetTokenFromTypeSpec](../metadata/imetadataemit-gettokenfromtypespec-method.md) 方法创建的记录。  
  
- `MemberRef` 通过调用 [IMetaDataEmit：:D efinememberref](../metadata/imetadataemit-definememberref-method.md) 方法创建的记录。  
  
- `MemberSpec` 通过调用 [IMetaDataEmit2：:D efinemethodspec](../metadata/imetadataemit2-definemethodspec-method.md) 方法创建的记录。  
  
- `UserString` 通过调用 [IMetaDataEmit：:D efineuserstring](../metadata/imetadataemit-defineuserstring-method.md) 方法创建的记录。  

从 .NET Core 3.0 开始， `ApplyMetaData` 还支持以下类型：

- `TypeDef` 通过调用 [IMetaDataEmit：:D efinetypedef](../metadata/imetadataemit-definetypedef-method.md) 方法创建的记录。

- `MethodDef` 通过调用 [IMetaDataEmit：:D efinemethod](../metadata/imetadataemit-definemethod-method.md) 方法创建的记录。 但是，不支持向现有类型添加虚拟方法。 必须在 [ModuleLoadFinished](icorprofilercallback-moduleloadfinished-method.md) 回调之前添加虚拟方法。

## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **头文件：** CorProf.idl、CorProf.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v461plus](../../../../includes/net-current-v461plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICorProfilerInfo7 接口](icorprofilerinfo7-interface.md)
