---
title: CoreClrDebugProcInfo 结构
ms.date: 03/30/2017
api_name:
- CoreClrDebugProcInfo
api_location:
- mscordbi_macx86.dll
api_type:
- COM
f1_keywords:
- CoreClrDebugProcInfo
helpviewer_keywords:
- remote debugging API [Silverlight]
- CoreClrDebugProcInfo structure
- Silverlight, remote debugging
ms.assetid: 4ddc37da-5c94-4beb-b61c-b54071c0e749
topic_type:
- apiref
ms.openlocfilehash: 5996393fd1a0504f9c3d3f9f07aa0e3d886a0787
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95719692"
---
# <a name="coreclrdebugprocinfo-structure"></a>CoreClrDebugProcInfo 结构

表示在远程计算机上运行的进程。  
  
## <a name="syntax"></a>语法  
  
```cpp  
struct  CoreClrDebugProcInfo {  
    DWORD m_dwPID;  
    DWORD m_dwInternalID;  
    WCHAR m_wszName[256];  
};  
```  
  
## <a name="members"></a>成员  
  
|成员|说明|  
|------------|-----------------|  
|`m_dwPID`|操作系统分配的进程标识符。|  
|`m_dwInternalID`|由运行在目标计算机上的远程调试代理分配的进程标识符。 此标识符的回收通常少于 OS 标识符。|  
|`m_wszName`|进程的命令行。 此成员可能会被截断。|  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** CoreClrRemoteDebuggingInterfaces  
  
 **库：** mscordbi_macx86.dll  
  
 **.NET Framework 版本：** 3.5 SP1
