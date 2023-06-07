---
title: ICorDebugModule2::ApplyChanges 方法
ms.date: 03/30/2017
api_name:
- ICorDebugModule2.ApplyChanges
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugModule2::ApplyChanges
helpviewer_keywords:
- ApplyChanges method [.NET Framework debugging]
- ICorDebugModule2::ApplyChanges method [.NET Framework debugging]
ms.assetid: 96fa3406-6a6f-41a1-88c6-d9bc5d1a16d1
topic_type:
- apiref
ms.openlocfilehash: a6b1a7c9be821890a3f15d8c3297273607f5bedd
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95709695"
---
# <a name="icordebugmodule2applychanges-method"></a>ICorDebugModule2::ApplyChanges 方法

将元数据中的更改和 Microsoft 中间语言 (MSIL) 代码中的更改应用到正在运行的进程。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT ApplyChanges (  
    [in] ULONG                       cbMetadata,  
    [in, size_is(cbMetadata)] BYTE   pbMetadata[],  
    [in] ULONG                       cbIL,  
    [in, size_is(cbIL)] BYTE         pbIL[]  
);  
```  
  
## <a name="parameters"></a>参数  

 `cbMetadata`  
 中增量元数据的大小（以字节为单位）。  
  
 `pbMetadata`  
 中包含增量元数据的缓冲区。 缓冲区的地址从 [IMetaDataEmit2：： SaveDeltaToMemory](../metadata/imetadataemit2-savedeltatomemory-method.md) 方法返回。  
  
 元数据中 (Rva) 的相对虚拟地址应相对于 MSIL 代码的开头。  
  
 `cbIL`  
 中增量 MSIL 代码的大小（以字节为单位）。  
  
 `pbIL`  
 中包含更新的 MSIL 代码的缓冲区。  
  
## <a name="remarks"></a>注解  

 `pbMetadata`参数采用特殊的增量元数据格式， (为[IMetaDataEmit2：： SaveDeltaToMemory](../metadata/imetadataemit2-savedeltatomemory-method.md)) 输出。 `pbMetadata` 采用以前的元数据作为基础，并描述要应用于该基的各个更改。  
  
 与此相反， `pbIL[` ] 参数包含用于已更新方法的新 msil，旨在完全替换该方法的以前的 msil  
  
 当在调试器内存中创建了增量 MSIL 和元数据时，调试器会调用以将 `ApplyChanges` 更改发送到公共语言运行时 (CLR) 。 运行时将更新其元数据表，将新的 MSIL 置于进程中，并为新的 MSIL 设置实时 (JIT) 编译。 应用这些更改后，调试器应调用 [IMetaDataEmit2：： ResetENCLog](../metadata/imetadataemit2-resetenclog-method.md) 来准备下一个编辑会话。 然后，调试器可以继续执行该过程。  
  
 每当调试器 `ApplyChanges` 在具有增量元数据的模块上调用时，它还应在该模块的元数据的所有副本上调用具有相同增量元数据的 [IMetaDataEmit：： ApplyEditAndContinue](../metadata/imetadataemit-applyeditandcontinue-method.md) （用于发出更改的副本除外）。 如果元数据的副本在某种程度上与实际的元数据不同步，则调试器始终可以丢弃该复制并获取新副本。  
  
 如果该 `ApplyChanges` 方法失败，调试会话将处于无效状态，必须重新启动。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]
