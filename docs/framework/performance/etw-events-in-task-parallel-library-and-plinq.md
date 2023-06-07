---
title: 任务并行库和 PLINQ 中的 ETW 事件
description: 阅读任务并行库和 PLINQ 中的 ETW 事件。 使用这些事件对应用程序进行分析和故障排除。
ms.date: 03/30/2017
helpviewer_keywords:
- tasks, ETW events
ms.assetid: 87a9cff5-d86f-4e44-a06e-d12764d0dce2
ms.openlocfilehash: 4cb4967ea704064ae08d09311ff33720e3871e19
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96263641"
---
# <a name="etw-events-in-task-parallel-library-and-plinq"></a>任务并行库和 PLINQ 中的 ETW 事件

任务并行库和 PLINQ 均可生成 Windows 事件跟踪 (ETW) 事件，通过 Windows 性能分析器等工具使用这些事件可分析并排除应用程序故障。 但在大多数情况下，分析并行应用程序代码的最佳方式是在 Visual Studio 中使用 [并发可视化工具](/visualstudio/profiling/concurrency-visualizer) 。

## <a name="task-parallel-library-etw-events"></a>任务并行库 ETW 事件

在 EVENT_HEADER 结构中，<xref:System.Threading.Tasks.Parallel.For%2A?displayProperty=nameWithType><xref:System.Threading.Tasks.Parallel.ForEach%2A?displayProperty=nameWithType> 和 <xref:System.Threading.Tasks.Parallel.Invoke%2A?displayProperty=nameWithType> 生成的事件的 ProviderId GUID 为：

`0x2e5dba47, 0xa3d2, 0x4d16, 0x8e, 0xe0, 0x66, 0x71, 0xff, 0xdc, 0xd7, 0xb5`

### <a name="parallel-loop-begin"></a>并行循环开始

EVENT_DESCRIPTOR.Task = 1

EVENT_DESCRIPTOR.Id = 1

#### <a name="user-data"></a>用户数据

|**名称**|**类型**|**说明**|
|--------------|--------------|---------------------|
|OriginatingTaskSchedulerID|<xref:System.Int32?displayProperty=nameWithType>|启动循环的 TaskScheduler 的 ID。|
|OriginatingTaskID|<xref:System.Int32?displayProperty=nameWithType>|启动循环的任务的 ID。|
|ForkJoinContextID|<xref:System.Int32?displayProperty=nameWithType>|用于为具有分叉/联接语义的事件指示嵌套和对的唯一标识符。|
|OperationType|<xref:System.Int32?displayProperty=nameWithType>|指示循环的类型：<br /><br /> 1 = ParallelInvoke<br /><br /> 2 = ParallelFor<br /><br /> 3 = ParallelForEach|
|InclusiveFrom|<xref:System.Int64?displayProperty=nameWithType>|循环计数器的起始值|
|ExclusiveTo|<xref:System.Int64?displayProperty=nameWithType>|循环计数器的结束值|

### <a name="parallel-loop-end"></a>并行循环结束

 EVENT_DESCRIPTOR.Task = 2

 EVENT_DESCRIPTOR.Id = 2

#### <a name="user-data"></a>用户数据

|**名称**|**类型**|**说明**|
|--------------|--------------|---------------------|
|OriginatingTaskSchedulerID|<xref:System.Int32?displayProperty=nameWithType>|启动循环的 TaskScheduler 的 ID。|
|OriginatingTaskID|<xref:System.Int32?displayProperty=nameWithType>|启动循环的任务的 ID。|
|ForkJoinContextID|<xref:System.Int32?displayProperty=nameWithType>|用于为具有分叉/联接语义的事件指示嵌套和对的唯一标识符。|
|totalIterations|<xref:System.Int64?displayProperty=nameWithType>|迭代总次数|

### <a name="parallel-invoke-begin"></a>并行调用开始

 EVENT_DESCRIPTOR.Task = 3

 EVENT_DESCRIPTOR.Id = 3

#### <a name="user-data"></a>用户数据

|**名称**|**类型**|**说明**|
|--------------|--------------|---------------------|
|OriginatingTaskSchedulerID|<xref:System.Int32?displayProperty=nameWithType>|启动循环的 TaskScheduler 的 ID。|
|OriginatingTaskID|<xref:System.Int32?displayProperty=nameWithType>|启动循环的任务的 ID。|
|ForkJoinContextID|<xref:System.Int32?displayProperty=nameWithType>|用于为具有分叉/联接语义的事件指示嵌套和对的唯一标识符。|
|totalIterations|<xref:System.Int64?displayProperty=nameWithType>|迭代总次数|
|operationType|<xref:System.Int32?displayProperty=nameWithType>|指示循环的类型：<br /><br /> 1 = ParallelInvoke<br /><br /> 2 = ParallelFor<br /><br /> 3 = ParallelForEach|
|ActionCount|<xref:System.Int32?displayProperty=nameWithType>|将在并行调用中执行的操作数。|

### <a name="parallel-invoke-end"></a>并行调用结束

 EVENT_DESCRIPTOR.Task = 4

 EVENT_DESCRIPTOR.Id = 4

#### <a name="user-data"></a>用户数据

|**名称**|**类型**|**说明**|
|--------------|--------------|---------------------|
|OriginatingTaskSchedulerID|<xref:System.Int32?displayProperty=nameWithType>|启动循环的 TaskScheduler 的 ID。|
|OriginatingTaskID|<xref:System.Int32?displayProperty=nameWithType>|启动循环的任务的 ID。|
|ForkJoinContextID|<xref:System.Int32?displayProperty=nameWithType>|用于为具有分叉/联接语义的事件指示嵌套和对的唯一标识符。|

## <a name="plinq-etw-events"></a>PINQ ETW 事件

 PLINQ 的 EVENT_HEADER.ProviderId GUID 为：

`0x159eeeec, 0x4a14, 0x4418, 0xa8, 0xfe, 0xfa, 0xab, 0xcd, 0x98, 0x78, 0x87`

### <a name="parallel-query-begin"></a>并行查询开始

 EVENT_DESCRIPTOR.Task = 1

 EVENT_DESCRIPTOR.Id = 1

#### <a name="user-data"></a>用户数据

|**名称**|**类型**|**说明**|
|--------------|--------------|---------------------|
|OriginatingTaskSchedulerID|<xref:System.Int32?displayProperty=nameWithType>|启动循环的 TaskScheduler 的 ID。|
|OriginatingTaskID|<xref:System.Int32?displayProperty=nameWithType>|启动循环的任务的 ID。|
|QueryID|<xref:System.Int32?displayProperty=nameWithType>|唯一的查询标识符。|

### <a name="parallel-query-end"></a>并行查询结束

 EVENT_DESCRIPTOR.Task = 2

 EVENT_DESCRIPTOR.Id = 2

#### <a name="user-data"></a>用户数据

|**名称**|**类型**|**说明**|
|--------------|--------------|---------------------|
|OriginatingTaskSchedulerID|<xref:System.Int32?displayProperty=nameWithType>|启动循环的 TaskScheduler 的 ID。|
|OriginatingTaskID|<xref:System.Int32?displayProperty=nameWithType>|启动循环的任务的 ID。|
|QueryID|<xref:System.Int32?displayProperty=nameWithType>|唯一的查询标识符。|

## <a name="see-also"></a>另请参阅

- [ETW Events in the .NET Framework](etw-events.md)
- [任务并行库 (TPL)](../../standard/parallel-programming/task-parallel-library-tpl.md)
- [并行 LINQ (PLINQ)](../../standard/parallel-programming/introduction-to-plinq.md)
