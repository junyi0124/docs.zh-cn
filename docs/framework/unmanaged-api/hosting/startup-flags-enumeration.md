---
title: STARTUP_FLAGS 枚举
ms.date: 03/30/2017
api_name:
- STARTUP_FLAGS
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- STARTUP_FLAGS
helpviewer_keywords:
- STARTUP_FLAGS enumeration [.NET Framework hosting]
ms.assetid: 4f043594-0c45-4bc6-988e-a6793f0d8d06
topic_type:
- apiref
ms.openlocfilehash: 3c3f4d644bd7073655d2d77fe7f65a3a46cfea24
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95729897"
---
# <a name="startup_flags-enumeration"></a>STARTUP_FLAGS 枚举

包含指示公共语言运行时 (CLR) 的启动行为的值。 默认情况下，垃圾回收不是并发的，并且仅将基类库加载到非特定于域的区域。  
  
## <a name="syntax"></a>语法  
  
```cpp  
typedef enum {  
    STARTUP_CONCURRENT_GC                         = 0x1,  
    STARTUP_LOADER_OPTIMIZATION_MASK              = 0x3<<1,  
    STARTUP_LOADER_OPTIMIZATION_SINGLE_DOMAIN     = 0x1<<1,  
    STARTUP_LOADER_OPTIMIZATION_MULTI_DOMAIN      = 0x2<<1,  
    STARTUP_LOADER_OPTIMIZATION_MULTI_DOMAIN_HOST = 0x3<<1,  
  
    STARTUP_LOADER_SAFEMODE                       = 0x10,  
    STARTUP_LOADER_SETPREFERENCE                  = 0x100,  
  
    STARTUP_SERVER_GC                             = 0x1000,  
    STARTUP_HOARD_GC_VM                           = 0x2000,  
  
    STARTUP_SINGLE_VERSION_HOSTING_INTERFACE      = 0x4000,  
    STARTUP_LEGACY_IMPERSONATION                  = 0x10000,  
    STARTUP_DISABLE_COMMITTHREADSTACK             = 0x20000,  
    STARTUP_ALWAYSFLOW_IMPERSONATION              = 0x40000,  
    STARTUP_TRIM_GC_COMMIT                        = 0x80000,  
  
    STARTUP_ETW                                   = 0x100000,  
    STARTUP_ARM                                   = 0x400000  
} STARTUP_FLAGS;  
```  
  
## <a name="members"></a>成员  
  
|成员|说明|  
|------------|-----------------|  
|`STARTUP_CONCURRENT_GC`|指定应使用并发垃圾回收。 如果调用方请求单处理器计算机上的服务器生成和并发垃圾回收，则会改为运行工作站构建和非并发垃圾回收。 **注意：**  在实施了64位系统上的 WOW64 x86 模拟器的应用程序中不支持并发垃圾回收，该程序实现了以前称为 IA-64) 的 Intel Itanium 架构 (。 有关在64位 Windows 系统上使用 WOW64 的详细信息，请参阅 [运行32位应用程序](/windows/desktop/WinProg64/running-32-bit-applications)。|  
|`STARTUP_LOADER_OPTIMIZATION_MASK`|指定应执行加载程序优化。|  
|`STARTUP_LOADER_OPTIMIZATION_SINGLE_DOMAIN`|指定不以非特定于域的形式加载程序集。|  
|`STARTUP_LOADER_OPTIMIZATION_MULTI_DOMAIN`|指定以非特定于域的形式加载所有程序集。|  
|`STARTUP_LOADER_OPTIMIZATION_MULTI_DOMAIN_HOST`|指定以非特定于域的形式加载所有强名称程序集。|  
|`STARTUP_LOADER_SAFEMODE`|指定将不将 CLR 版本策略应用于传入的版本。 将加载指定的 CLR 的确切版本。 填充码不会评估策略来确定最新的兼容版本。|  
|`STARTUP_LOADER_SETPREFERENCE`|指定将设置首选运行时，但实际不会启动。|  
|`STARTUP_SERVER_GC`|指定将使用服务器垃圾回收。|  
|`STARTUP_HOARD_GC_VM`|指定垃圾回收将保留使用的虚拟地址。|  
|`STARTUP_SINGLE_VERSION_HOSTING_INTERFACE`|指定将不允许混合宿主接口。|  
|`STARTUP_LEGACY_IMPERSONATION`|指定模拟应默认情况下不流经异步点。|  
|`STARTUP_DISABLE_COMMITTHREADSTACK`|指定当线程开始运行时，不应提交完整线程堆栈。|  
|`STARTUP_ALWAYSFLOW_IMPERSONATION`|指定通过平台调用实现的托管模拟和模拟将流经异步点。 默认情况下，仅托管模拟将流经异步点。|  
|`STARTUP_TRIM_GC_COMMIT`|指定在系统内存不足时垃圾回收将使用较少的已提交空间。 请 `gcTrimCommitOnLowMemory` 参阅 [针对共享 Web 托管的优化](../../../standard/garbage-collection/optimization-for-shared-web-hosting.md)。|  
|`STARTUP_ETW`|指定为 Windows (ETW) 启用了公共语言运行时事件的事件跟踪。 从 Windows Vista 开始，始终启用事件跟踪，因此该标志不起作用。 请参阅 [控制 .NET Framework 日志记录](../../performance/controlling-logging.md)。|  
|`STARTUP_ARM`|指定启用应用程序域资源监视。 请参见 <xref:System.AppDomain.MonitoringIsEnabled%2A?displayProperty=nameWithType> 属性和[ \<appDomainResourceMonitoring> 元素](../../configure-apps/file-schema/runtime/appdomainresourcemonitoring-element.md)。|  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Mscoree.dll  
  
 **库：** MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [承载枚举](hosting-enumerations.md)
