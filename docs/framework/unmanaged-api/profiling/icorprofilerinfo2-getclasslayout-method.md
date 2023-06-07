---
title: ICorProfilerInfo2::GetClassLayout 方法
ms.date: 03/30/2017
api_name:
- ICorProfilerInfo2.GetClassLayout
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerInfo2::GetClassLayout
helpviewer_keywords:
- ICorProfilerInfo2::GetClassLayout method [.NET Framework profiling]
- GetClassLayout method, ICorProfilerInfo2 interface [.NET Framework profiling]
ms.assetid: a3a36987-5666-4e2f-95b5-d0cb246502ec
topic_type:
- apiref
ms.openlocfilehash: a2bac05e7471a0df8d624bf5dfbe2aa58c25cf4c
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95727167"
---
# <a name="icorprofilerinfo2getclasslayout-method"></a>ICorProfilerInfo2::GetClassLayout 方法

获取内存中由指定的类定义的字段的布局信息。 也就是说，此方法获取类的字段的偏移量。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetClassLayout(  
    [in]  ClassID classID,  
    [in, out] COR_FIELD_OFFSET rFieldOffset[],  
    [in]  ULONG cFieldOffset,  
    [out] ULONG *pcFieldOffset,  
    [out] ULONG *pulClassSize);  
```  
  
## <a name="parameters"></a>参数  

 `classID`  
 [in] 将为其检索布局的类的 ID。  
  
 `rFieldOffset`  
 [in，out] [COR_FIELD_OFFSET](../metadata/cor-field-offset-structure.md) 结构的数组，其中每个结构都包含类的字段的标记和偏移量。  
  
 `cFieldOffset`  
 [in] `rFieldOffset` 数组的大小。  
  
 `pcFieldOffset`  
 [out] 指向可用元素总数的指针。 如果 `cFieldOffset` 为 0，则此值指示所需元素的数目。  
  
 `pulClassSize`  
 [out] 指向包含类的大小（以字节为单位）的位置的指针。  
  
## <a name="remarks"></a>注解  

 `GetClassLayout` 方法仅返回由类自身定义的字段。 如果类的父类也定义了字段，探查器必须对父类调用 `GetClassLayout` 以获取这些字段。  
  
 如果你通过字符串类使用 `GetClassLayout`，则该方法将失败，错误代码为 E_INVALIDARG。 使用 [ICorProfilerInfo2：： GetStringLayout](icorprofilerinfo2-getstringlayout-method.md) 获取有关字符串布局的信息。 当使用数组类来调用 `GetClassLayout` 时，它也将失败。  
  
 返回 `GetClassLayout` 后，必须验证 `rFieldOffset` 缓冲区是否具有用于包含所有可用 `COR_FIELD_OFFSET` 结构的足够空间。 若要执行此操作，请将 `pcFieldOffset` 指向的值与 `COR_FIELD_OFFSET` 结构的大小除以 `rFieldOffset` 大小所得的值进行比较。 如果 `rFieldOffset` 不够大，则分配更大的 `rFieldOffset` 缓冲区，用新的、更大的大小来更新 `cFieldOffset`并再次调用 `GetClassLayout`。  
  
 或者，可以先用长度为零的 `rFieldOffset` 缓冲区调用 `GetClassLayout` 以获取正确的缓冲区大小。 然后，可将缓冲区大小设置为 `pcFieldOffset` 中返回的值，并再次调用 `GetClassLayout`。  
  
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
