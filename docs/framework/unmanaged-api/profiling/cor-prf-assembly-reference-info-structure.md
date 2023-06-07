---
title: COR_PRF_ASSEMBLY_REFERENCE_INFO 结构
ms.date: 03/30/2017
dev_langs:
- cpp
ms.assetid: c8c1d916-8d1a-4f82-8128-9fd3732383fc
ms.openlocfilehash: 7c7d447afcb5a8617aa92212f3325719d5f43bf5
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95718613"
---
# <a name="cor_prf_assembly_reference_info-structure"></a>COR_PRF_ASSEMBLY_REFERENCE_INFO 结构

[仅在 .NET Framework 4.5.2 及更高版本中受支持]  
  
 向公共语言运行时提供有关在执行程序集引用闭包审核时应考虑的程序集引用的信息。  
  
## <a name="syntax"></a>语法  
  
```cpp  
typedef struct _COR_PRF_ASSEMBLY_REFERENCE_INFO {  
    void* pbPublicKeyOrToken;  
    ULONG cbPublicKeyOrToken;  
    LPCWSTR szName;  
    ASSEMBLYMETADATA* pMetaData;  
    void* pbHashValue;  
    ULONG cbHashValue;  
    DWORD dwAssemblyRefFlags;  
} COR_PRF_EX_CLAUSE_INFO;  
```  
  
## <a name="members"></a>成员  
  
|成员|说明|  
|------------|-----------------|  
|`pbPublicKeyOrToken`|指向程序集的公钥或标记的指针。|  
|`cbPublicKeyOrToken`|公钥或标记中的字节数。|  
|`szName`|所引用的程序集的名称。|  
|`pMetaData`|指向程序集的元数据的指针。|  
|`pbHashValue`|指向哈希二进制大对象 (BLOB) 的指针。|  
|`cbHashValue`|哈希 BLOB 中的字节数。|  
|`dwAssemblyRefFlags`|程序集的标志。|  
  
## <a name="remarks"></a>注解  

 当 `COR_PRF_EX_CLAUSE_INFO` 结构声明在执行程序集引用闭包审核时公共语言运行时应考虑的附加程序集引用时，该结构将由探查器进行填充。  
  
 如果探查器注册了 [ICorProfilerCallback6：： GetAssemblyReferences](icorprofilercallback6-getassemblyreferences-method.md) 回调方法，则运行时将传递要加载的程序集的路径和名称以及指向该方法的指向 [ICorProfilerAssemblyReferenceProvider](icorprofilerassemblyreferenceprovider-interface.md) 接口对象的指针。 然后，探查器可以[ICorProfilerAssemblyReferenceProvider::AddAssemblyReference](icorprofilerassemblyreferenceprovider-addassemblyreference-method.md) `COR_PRF_ASSEMBLY_REFERENCE_INFO` 为它计划从[ICorProfilerCallback6：： GetAssemblyReferences](icorprofilercallback6-getassemblyreferences-method.md)回调中指定的程序集引用的每个目标程序集调用 ICorProfilerAssemblyReferenceProvider：： AddAssemblyReference 方法和一个对象。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **头文件：** CorProf.idl、CorProf.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v452plus](../../../../includes/net-current-v452plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [分析结构](profiling-structures.md)
- [GetAssemblyReferences 方法](icorprofilercallback6-getassemblyreferences-method.md)
- [AddAssemblyReference 方法](icorprofilerassemblyreferenceprovider-addassemblyreference-method.md)
