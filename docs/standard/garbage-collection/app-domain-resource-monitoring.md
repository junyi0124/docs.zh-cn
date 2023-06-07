---
title: 应用程序域资源监控
ms.date: 03/30/2017
helpviewer_keywords:
- monitoring managed memory use by application domain
- application domains, memory use
- memory use, monitoring
- application domains, resource monitoring
ms.assetid: 318bedf8-7f35-4f00-b34a-2b7b8e3fa315
ms.openlocfilehash: c4644defa5a67d9d39c4faa89943a6aa5f763eae
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94827823"
---
# <a name="application-domain-resource-monitoring"></a>应用程序域资源监控

借助应用域资源监视 (ARM)，主机可以通过应用域监视 CPU 和内存使用情况。 对于在长时间运行的进程中使用多个应用域的 ASP.NET 等主机，这就很有用。 主机可以卸载对整个进程的性能有不利影响的应用的应用域，但仅当能够发现有问题的应用时，才可以这样做。 ARM 提供的信息就有助于作出此类决定。

例如，托管服务可能有多个应用在 ASP.NET 服务器上运行。 如果进程中的一个应用开始占用太多内存或过长的处理器时间，托管服务就可以使用 ARM 发现导致问题发生的应用域。

ARM 是轻型服务，足可用于实际应用。 若要访问信息，可以使用 Windows 事件跟踪 (ETW)，或直接使用托管或本机 API。

## <a name="enabling-resource-monitoring"></a>启用资源监视

可以通过下列四种方法启用 ARM：在公共语言运行时 (CLR) 启动时提供配置文件、使用非托管宿主 API、使用托管代码或侦听 ARM ETW 事件。

一旦启用，ARM 就会开始收集进程中所有应用域的相关数据。如果在启用 ARM 前就创建了应用域，累积数据会在 ARM 启用时启动，而不是在应用域创建时启动。一旦启用，ARM 便无法再禁用。

- 可以在 CLR 启动时启用 ARM，具体操作是向配置文件添加 [\<appDomainResourceMonitoring>](../../framework/configure-apps/file-schema/runtime/appdomainresourcemonitoring-element.md) 元素，并将 `enabled` 属性设置为 `true`。 值 `false`（默认值）只表示不在启动时启用 ARM；稍后可以使用其他激活机制之一来激活它。

- 主机可以请求获取 [ICLRAppDomainResourceMonitor](../../framework/unmanaged-api/hosting/iclrappdomainresourcemonitor-interface.md) 托管接口来启用 ARM。 成功获取此接口后，就会启用 ARM。

- 托管代码可以将静态 <xref:System.AppDomain.MonitoringIsEnabled%2A?displayProperty=nameWithType> 属性（Visual Basic 中的 `Shared`）设置为 `true`，从而启用 ARM。 设置此属性后，就会启用 ARM。

- 启动后，可以通过侦听 ETW 事件来启用 ARM。 使用 `AppDomainResourceManagementKeyword` 关键字启用公共提供程序 `Microsoft-Windows-DotNETRuntime` 后，ARM 便会启用，并开始抛出所有应用域的事件。 若要将数据与应用域及线程相关联，还必须使用 `ThreadingKeyword` 关键字启用 `Microsoft-Windows-DotNETRuntimeRundown` 提供程序。

## <a name="using-arm"></a>使用 ARM

ARM 提供应用域使用的总处理器时间，以及关于内存使用情况的三种信息。

- **应用域使用的总处理器时间（以秒为单位）** ：此时间的计算方式为，将操作系统报告的线程时间相加得出，包括在生存期内在应用域中执行的所有线程。 受阻止或处于睡眠状态的线程不使用处理器时间。 如果线程调用本机代码，线程在本机代码中花费的时间计入执行调用的应用域的总处理器时间。

  - 托管 API：<xref:System.AppDomain.MonitoringTotalProcessorTime%2A?displayProperty=nameWithType> 属性。

  - 宿主 API：[ICLRAppDomainResourceMonitor::GetCurrentCpuTime](../../framework/unmanaged-api/hosting/iclrappdomainresourcemonitor-getcurrentcputime-method.md) 方法。

  - ETW 事件：`ThreadCreated`、`ThreadAppDomainEnter` 和 `ThreadTerminated` 事件。 若要了解提供程序和关键字，请参阅 [CLR ETW 事件](../../framework/performance/clr-etw-events.md) 中的“应用域资源监视事件”。

- **应用程序域在其生命周期内进行的托管分配总量（以字节为单位）** ：总分配并不总是反映应用程序域的内存使用情况，因为所分配的对象可能是短期的。 不过，如果应用分配并释放大量对象，分配成本可能会非常高。

  - 托管 API：<xref:System.AppDomain.MonitoringTotalAllocatedMemorySize%2A?displayProperty=nameWithType> 属性。

  - 宿主 API：[ICLRAppDomainResourceMonitor::GetCurrentAllocated](../../framework/unmanaged-api/hosting/iclrappdomainresourcemonitor-getcurrentallocated-method.md) 方法。

  - ETW 事件：`AppDomainMemAllocated` 事件、`Allocated` 字段。

- **应用域引用且在最新执行的完全阻止式回收后保留的托管内存（以字节为单位）** ：此数字只有在执行完全阻止式回收后才准确。 （这与并发回收相反，后者发生在后台，不会阻止应用。）例如，<xref:System.GC.Collect?displayProperty=nameWithType> 方法重载导致执行完全阻止式回收。

  - 托管 API：<xref:System.AppDomain.MonitoringSurvivedMemorySize%2A?displayProperty=nameWithType> 属性。

  - 宿主 API：[ICLRAppDomainResourceMonitor::GetCurrentSurvived](../../framework/unmanaged-api/hosting/iclrappdomainresourcemonitor-getcurrentsurvived-method.md) 方法、`pAppDomainBytesSurvived` 参数。

  - ETW 事件：`AppDomainMemSurvived` 事件、`Survived` 字段。

- **进程引用且在最新执行的完全阻止式回收后保留的托管内存总量（以字节为单位）** ：可将为单个应用程序域保留的内存与此数字进行比较。

  - 托管 API：<xref:System.AppDomain.MonitoringSurvivedProcessMemorySize%2A?displayProperty=nameWithType> 属性。

  - 宿主 API：[ICLRAppDomainResourceMonitor::GetCurrentSurvived](../../framework/unmanaged-api/hosting/iclrappdomainresourcemonitor-getcurrentsurvived-method.md) 方法、`pTotalBytesSurvived` 参数。

  - ETW 事件：`AppDomainMemSurvived` 事件、`ProcessSurvived` 字段。

### <a name="determining-when-a-full-blocking-collection-occurs"></a>确定何时发生完全阻止式回收

若要确定何时保留的内存计数是准确的，只需知道何时发生了完全阻止式回收即可。 执行此操作的方法取决于用来检查 ARM 统计信息的 API。

#### <a name="managed-api"></a>托管 API

如果使用 <xref:System.AppDomain> 类的属性，可以使用 <xref:System.GC.RegisterForFullGCNotification%2A?displayProperty=nameWithType> 方法来注册获取完全回收的通知。 使用的阈值并不重要，因为正在等待回收完成，而不是回收的方法完成。 然后，可以调用 <xref:System.GC.WaitForFullGCComplete%2A?displayProperty=nameWithType> 方法，一直阻止到完全回收完成。 可以创建线程，用于在循环中调用此方法，并在此方法返回结果时执行任何所需的分析。

也可以定期调用 <xref:System.GC.CollectionCount%2A?displayProperty=nameWithType> 方法，以确定第 2 代回收计数是否已增加。 此方法可能无法准确指明完全回收的发生，具体视轮询频率而定。

#### <a name="hosting-api"></a>宿主 API

如果使用非托管宿主 API，主机必须向 CLR 传递 [IHostGCManager](../../framework/unmanaged-api/hosting/ihostgcmanager-interface.md) 接口实现。 如果 CLR 恢复执行在回收发生时被暂停的线程，便会调用 [IHostGCManager::SuspensionEnding](../../framework/unmanaged-api/hosting/ihostgcmanager-suspensionending-method.md) 方法。 CLR 将生成的已完成回收作为方法参数进行传递，以便主机能够确定回收是完全回收还是部分回收。 实现 [IHostGCManager::SuspensionEnding](../../framework/unmanaged-api/hosting/ihostgcmanager-suspensionending-method.md) 方法可以查询保留的内存，以确保在内存更新时立即检索计数。

## <a name="see-also"></a>请参阅

- <xref:System.AppDomain.MonitoringIsEnabled%2A?displayProperty=nameWithType>
- [ICLRAppDomainResourceMonitor 接口](../../framework/unmanaged-api/hosting/iclrappdomainresourcemonitor-interface.md)
- [\<appDomainResourceMonitoring>](../../framework/configure-apps/file-schema/runtime/appdomainresourcemonitoring-element.md)
- [CLR ETW 事件](../../framework/performance/clr-etw-events.md)
