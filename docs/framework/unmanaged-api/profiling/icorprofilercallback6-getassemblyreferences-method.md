---
title: ICorProfilerCallback6::GetAssemblyReferences 方法
ms.date: 03/30/2017
dev_langs:
- cpp
api_name:
- ICorProfilerCallback6.GetAssemblyReferences
api_location:
- mscorwks.dll
- corprof.idl
api_type:
- COM
ms.assetid: 8b391afb-d79f-41bd-94ce-43ce62c6b5fc
topic_type:
- apiref
ms.openlocfilehash: c9e973009f46ef7e554ee2df63493464f4956342
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95725477"
---
# <a name="icorprofilercallback6getassemblyreferences-method"></a>ICorProfilerCallback6::GetAssemblyReferences 方法

[仅在 .NET Framework 4.5.2 及更高版本中受支持]  
  
 当公共语言运行时执行程序集引用闭包审核时，通知探查器程序集正处于非常早期的加载阶段。  
  
## <a name="syntax"></a>语法  
  
```cpp
HRESULT GetAssemblyReferences(        [in, string] const WCHAR* wszAssemblyPath,  
        [in] ICorProfilerAssemblyReferenceProvider* pAsmRefProvider  
);  
```  
  
## <a name="parameters"></a>参数  

 `wszAssemblyPath`  
 [in] 元数据将发生修改的程序集的路径和名称。  
  
 `pAsmRefProvider`  
 中一个指针，指向用于指定要添加的程序集引用的 [ICorProfilerAssemblyReferenceProvider](icorprofilerassemblyreferenceprovider-interface.md) 接口的地址。  
  
## <a name="return-value"></a>返回值  

 将忽略此回调的返回值。  
  
## <a name="remarks"></a>注解  

 调用[ICorProfilerCallback5：： SetEventMask2](icorprofilerinfo5-seteventmask2-method.md)方法时，可通过设置[COR_PRF_HIGH_ADD_ASSEMBLY_REFERENCES](cor-prf-high-monitor-enumeration.md)事件掩码标志来控制此回调。 如果探查器注册了 [ICorProfilerCallback6：： GetAssemblyReferences](icorprofilercallback6-getassemblyreferences-method.md) 回调方法，则运行时将传递要加载的程序集的路径和名称以及指向该方法的指向 [ICorProfilerAssemblyReferenceProvider](icorprofilerassemblyreferenceprovider-interface.md) 接口对象的指针。 然后，探查器可以[ICorProfilerAssemblyReferenceProvider::AddAssemblyReference](icorprofilerassemblyreferenceprovider-addassemblyreference-method.md) `COR_PRF_ASSEMBLY_REFERENCE_INFO` 为它计划从回调中指定的程序集引用的每个目标程序集调用 ICorProfilerAssemblyReferenceProvider：： AddAssemblyReference 方法 `GetAssemblyReferences` 。  
  
 仅当探查器必须修改程序集的元数据以添加程序集引用时，才使用 `GetAssemblyReferences` 回调。  (但要注意的是，程序集元数据的实际修改是在 [ICorProfilerCallback：： ModuleLoadFinished](icorprofilercallback-moduleloadfinished-method.md)回调方法中完成的。 ) 探查器应该实现 `GetAssemblyReferences` 回调方法，以通知公共语言运行时 (CLR) 在加载模块时将添加程序集引用。  这有助于确保即使探查器打算以后修改元数据程序集引用，CLR 在此早期阶段作出的程序集共享决策也会保持有效。  这可以避免一些由于探查器元数据修改而导致 `SECURITY_E_INCOMPATIBLE_SHARE` 错误的实例。  
  
 探查器使用此方法提供的 [ICorProfilerAssemblyReferenceProvider](icorprofilerassemblyreferenceprovider-interface.md) 对象将程序集引用添加到 CLR 程序集引用闭包查看器。  应仅在此回调内使用 [ICorProfilerAssemblyReferenceProvider](icorprofilerassemblyreferenceprovider-interface.md) 对象。 从此回调对 [ICorProfilerAssemblyReferenceProvider：： AddAssemblyReference](icorprofilerassemblyreferenceprovider-addassemblyreference-method.md) 方法的调用不会导致修改的元数据，而只会导致修改后的程序集引用闭包审核。 探查器仍必须使用 [IMetaDataAssemblyEmit](../metadata/imetadataassemblyemit-interface.md) 对象从引用程序集的 [ICorProfilerCallback：： ModuleLoadFinished](icorprofilercallback-moduleloadfinished-method.md) 回调中显式添加程序集引用，即使它实现回调也是如此 `GetAssemblyReferences` 。  
  
 探查器应准备为同一程序集接收对此回调的重复调用，并且应对每个这样的重复调用做出相同的响应 (方法是将同一组 [ICorProfilerAssemblyReferenceProvider：： AddAssemblyReference](icorprofilerassemblyreferenceprovider-addassemblyreference-method.md) 调用) 。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **头文件：** CorProf.idl、CorProf.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v452plus](../../../../includes/net-current-v452plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICorProfilerCallback6 接口](icorprofilercallback6-interface.md)
- [ModuleLoadFinished 方法](icorprofilercallback-moduleloadfinished-method.md)
- [COR_PRF_ASSEMBLY_REFERENCE_INFO 结构](cor-prf-assembly-reference-info-structure.md)
- [ICorProfilerAssemblyReferenceProvider 方法](icorprofilerassemblyreferenceprovider-interface.md)
