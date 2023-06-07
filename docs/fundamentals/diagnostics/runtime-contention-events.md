---
title: 监视锁争用运行时事件
description: 请参阅收集特定于监视锁争用的信息的 ETW 事件。
ms.date: 11/13/2020
ms.topic: reference
helpviewer_keywords:
- monitor lock contention events (CoreCLR)
- ETW, EventPipe, LTTng contention events (CoreCLR)
ms.openlocfilehash: 38e5f493d4b223b4839dbd6f814cf656722189b2
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "96591060"
---
# <a name="net-runtime-contention-events"></a>.NET 运行时争用事件

这些运行时事件捕获有关监视锁争用的信息，例如 with `Monitor.Enter` 或 c # lock 关键字。 有关如何将这些事件用于诊断的详细信息，请参阅 [日志记录和跟踪 .net 应用程序](../../core/diagnostics/logging-tracing.md)

## <a name="contentionstart_v1-event"></a>ContentionStart_V1 事件

在监视器锁争用开始时发出此事件。

|引发事件的关键字|Level|
|-----------------------------------|-----------|
|`ContentionKeyword` (0x4000)|信息性 (4)|

 下表显示了事件信息。

|事件|事件 ID|在发生以下情况时引发|
|-----------|--------------|-----------------|
|`ContentionStart_V1`|81|监视器锁争用开始。|

|字段名称|数据类型|说明|
|----------------|---------------|-----------------|
|`Flags`|`win:UInt8`|`0` 对于托管; `1` 适用于本机。|
|`ClrInstanceID`|`win:UInt16`|CoreCLR 实例的唯一 ID。|

## <a name="contentionstop_v1-event"></a>ContentionStop_V1 事件

此事件在监视器锁争用结束时发出。

|引发事件的关键字|Level|
|-----------------------------------|-----------|
|`ContentionKeyword` (0x4000)|信息性 (4)|

 下表显示了事件信息。

|事件|事件 ID|在发生以下情况时引发|
|-----------|--------------|-----------------|
|`ContentionStop_V1`|91|监视器锁争用结束。|

|字段名称|数据类型|说明|
|----------------|---------------|-----------------|
|`Flags`|`win:UInt8`|`0` 对于托管; `1` 适用于本机。|
|`ClrInstanceID`|`win:UInt16`|CoreCLR 实例的唯一 ID。|
|`DurationNs`|`win:Double`|争用的持续时间，以纳秒为单位。|
