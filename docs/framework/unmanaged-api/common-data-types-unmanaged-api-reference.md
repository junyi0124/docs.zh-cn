---
title: 常见数据类型（非托管 API 参考）
ms.date: 03/30/2017
ms.assetid: e4ab2c4c-9433-4eba-9e9a-096de406cafb
ms.openlocfilehash: 5c00ff6d0947b5d847a9622dce02bd310491818c
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95673977"
---
# <a name="common-data-types-unmanaged-api-reference"></a>常见数据类型（非托管 API 参考）

本主题列出了由 C/C++ `typedef` 语句定义的 .NET Framework 的非托管 API 所使用的简单数据类型。 这些数据类型通常是 C/C++ 基元数据类型的别名。 通常，这些数据类型的值是不透明的；即它们由特定的函数或方法返回，以便可以将它们传递给其他函数或方法，而无需修改。  
  
|数据类型|定义|定义位置|说明|  
|---------------|----------------|----------------|-----------------|  
|AppDomainID|`typedef UINT_PTR AppDomainID;`|corprof.h|应用程序域的标识符。|  
|AssemblyID|`typedef UINT_PTR AssemblyID;`|corprof.h|程序集的标识符。|  
|ClassID|`typedef UINT_PTR ClassID;`|corprof.h|托管类的标识符。|  
|CLRDATA_ADDRESS|`typedef ULONG64 CLRDATA_ADDRESS;`|clrdata|64位的内存地址。|
|CLRDATA_ENUM|`typedef ULONG64 CLRDATA_ADDRESS;`|不可用|64位的内存地址。|
|CONNID|`typedef DWORD CONNID;`|cordebug.h、mscoree.h|已连接到 Microsoft SQL Server 实例的线程的连接标识符。|  
|ContextID|`typedef UINT_PTR ContextID;`|corprof.h|与特定托管线程关联的上下文的标识符。|  
|COR_PRF_ELT_INFO|`typedef UINT_PTR COR_PRF_ELT_INFO;`|corprof.h|表示有关特定堆栈帧的信息的不透明的句柄。|  
|COR_PRF_FRAME_INFO|`typedef UINT_PTR COR_PRF_FRAME_INFO;`|corprof.h|指向堆栈帧的不透明的句柄。 此句柄仅在其传递到的回调过程中有效。|  
|CORDB_ADDRESS|`typedef ULONG64 CORDB_ADDRESS;`|cordebug.h|内存中的地址。|  
|CORDB_CONTINUE_STATUS|`typedef DWORD CORDB_CONTINUE_STATUS;`|cordebug.h|继续状态。|  
|CORDB_REGISTER|`typedef ULONG64 CORDB_REGISTER;`|cordebug.h|CPU 寄存器的值。|
|FunctionID|`typedef UINT_PTR FunctionID;`|corprof.h|函数或方法的标识符。|  
|GCHandleID|`typedef UINT_PTR GCHandleID;`|corprof.h|垃圾回收句柄。|  
|mdMethodDef|`typedef mdToken mdMethodDef;`|cordebug.h|方法定义标记。|
|mdToken|`typedef UINT32 mdToken;`|corprof.h|元数据标记 (元数据表) 中的行。|  
|ModuleID|`typedef UINT_PTR ModuleID;`|corprof.h|程序集模块的标识符。|  
|ObjectID|`typedef UINT_PTR ObjectID;`|corprof.h|对象的标识符。|  
|PCCOR_SIGNATURE|`typedef SIZE_T PCCOR_SIGNATURE;`|cordebug.h|指向成员或元数据签名的指针。|
|ProcessID|`typedef UINT_PTR ProcessID;`|corprof.h|托管进程的标识符。|  
|ReJITID|`typedef UINT_PTR ReJITID;`|corprof.h|实时编译的函数的标识符。|  
|SIZE_T|`typedef ULONG_PTR SIZE_T;`|corsym|指向64位的内存地址的指针。|
|TASKID|`typedef UINT64 TASKID;`|cordebug.h、mscoree.h|[ICLRTask](./hosting/iclrtask-interface.md)实例的标识符。|  
|ThreadID|`typedef UINT_PTR ThreadID;`|corprof.h|托管线程的标识符。|  
  
## <a name="see-also"></a>另请参阅

- [非托管 API 参考](index.md)
