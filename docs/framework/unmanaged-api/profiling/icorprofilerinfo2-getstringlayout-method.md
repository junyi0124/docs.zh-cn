---
title: ICorProfilerInfo2::GetStringLayout 方法
ms.date: 03/30/2017
api_name:
- ICorProfilerInfo2.GetStringLayout
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerInfo2::GetStringLayout
helpviewer_keywords:
- GetStringLayout method [.NET Framework profiling]
- ICorProfilerInfo2::GetStringLayout method [.NET Framework profiling]
ms.assetid: 43189651-a535-4803-a1d1-f1c427ace2ca
topic_type:
- apiref
ms.openlocfilehash: d141a78a953d4e0ab922535ad2363c79f2e18ecd
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95727037"
---
# <a name="icorprofilerinfo2getstringlayout-method"></a>ICorProfilerInfo2::GetStringLayout 方法

获取有关字符串对象布局的信息。 此方法在 .NET Framework 4 中已弃用，并且被 [ICorProfilerInfo3：： GetStringLayout2](icorprofilerinfo3-getstringlayout2-method.md) 方法取代。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetStringLayout(  
    [out] ULONG *pBufferLengthOffset,  
    [out] ULONG *pStringLengthOffset,  
    [out] ULONG *pBufferOffset);  
```  
  
## <a name="parameters"></a>参数  

 `pBufferLengthOffset`  
 弄一个指针， `ObjectID` 它指向存储字符串长度的相对于指针的位置偏移量。 长度存储为 `DWORD` 。  
  
> [!NOTE]
> 此参数返回字符串本身的长度，而不是缓冲区的长度。 缓冲区的长度不再可用。  
  
 `PStringLengthOffset`  
 弄指向位置偏移量的指针，该位置相对于 `ObjectID` 指针存储字符串本身的长度。 长度存储为 `DWORD` 。  
  
 `pBufferOffset`  
 弄一个指针， `ObjectID` 它指向存储宽字符字符串的缓冲区相对于指针的偏移量。  
  
## <a name="remarks"></a>注解  

 `GetStringLayout`方法获取 `ObjectID` 在其中存储下列位置的相对于指针的偏移量：  
  
- 字符串缓冲区的长度。  
  
- 字符串本身的长度。  
  
- 包含宽字符实际字符串的缓冲区。  
  
 字符串可以以 null 结尾。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **头文件：** CorProf.idl、CorProf.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICorProfilerInfo 接口](icorprofilerinfo-interface.md)
- [ICorProfilerInfo2 接口](icorprofilerinfo2-interface.md)
