---
title: ICorProfilerInfo2::GetClassIDInfo2 方法
ms.date: 03/30/2017
api_name:
- ICorProfilerInfo2.GetClassIDInfo2
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerInfo2::GetClassIDInfo2
helpviewer_keywords:
- GetClassIDInfo2 method [.NET Framework profiling]
- ICorProfilerInfo2::GetClassIDInfo2 method [.NET Framework profiling]
ms.assetid: 0141d582-d066-4d49-8d1f-ae82129a1960
topic_type:
- apiref
ms.openlocfilehash: 4b018a329396e0be684c999a33d4ef7c3518cb1c
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95703898"
---
# <a name="icorprofilerinfo2getclassidinfo2-method"></a>ICorProfilerInfo2::GetClassIDInfo2 方法

获取指定类的开放式泛型定义的父模块和元数据标记、 `ClassID` 其父类的，以及 `ClassID` 每个类型参数（如果存在）的。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetClassIDInfo2(  
    [in]  ClassID classId,  
    [out] ModuleID *pModuleId,  
    [out] mdTypeDef *pTypeDefToken,  
    [out] ClassID *pParentClassId,  
    [in]  ULONG32 cNumTypeArgs,  
    [out] ULONG32 *pcNumTypeArgs,  
    [out] ClassID typeArgs[]);  
```  
  
## <a name="parameters"></a>参数  

 `classId`  
 [in] 将为其检索信息的类的 ID。  
  
 `pModuleId`  
 弄指向指定类的开放式泛型定义的父模块 ID 的指针。  
  
 `pTypeDefToken`  
 弄指向指定类的开放式泛型定义的元数据标记的指针。  
  
 `pParentClassId`  
 [out] 指向父类 ID 的指针。  
  
 `cNumTypeArgs`  
 [in] `typeArgs` 数组的大小。  
  
 `pcNumTypeArgs`  
 [out] 指向可用元素总数的指针。  
  
 `typeArgs`  
 [out] `ClassID` 值的数组，其中每个值表示类的类型参数 ID。 方法返回时，`typeArgs` 将包含部分或全部可用 `ClassID` 值。  
  
## <a name="remarks"></a>注解  

 `GetClassIDInfo2`方法类似于[ICorProfilerInfo：： GetClassIDInfo](icorprofilerinfo-getclassidinfo-method.md)方法，但 `GetClassIDInfo2` 获取有关泛型类型的其他信息。  
  
 探查器代码可调用 [ICorProfilerInfo：： GetModuleMetaData](icorprofilerinfo-getmodulemetadata-method.md) 以获取给定模块的 [元数据](../metadata/index.md) 接口。 返回至 `pTypeDefToken` 所引用的位置的元数据标记可用于访问类的元数据。  
  
 `GetClassIDInfo2` 返回后，必须验证 `typeArgs` 缓冲区是否足够大，可包含所有 `ClassID` 值。 为此，请比较 `pcNumTypeArgs` 指向的值和 `cNumTypeArgs` 参数的值。 如果 `pcNumTypeArgs` 指向的值大于 `cNumTypeArgs`，请分配更大的 `typeArgs` 缓冲区，并用新的、更大的大小更新 `cNumTypeArgs`，然后再次调用 `GetClassIDInfo2`。  
  
 或者，可以先用长度为零的 `typeArgs` 缓冲区调用 `GetClassIDInfo2` 以获取正确的缓冲区大小。 然后，可将 `typeArgs` 缓冲区大小设置为 `pcNumTypeArgs` 中返回的值，并再次调用 `GetClassIDInfo2`。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **头文件：** CorProf.idl、CorProf.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICorProfilerInfo 接口](icorprofilerinfo-interface.md)
- [ICorProfilerInfo2 接口](icorprofilerinfo2-interface.md)
- [分析接口](profiling-interfaces.md)
- [分析](index.md)
