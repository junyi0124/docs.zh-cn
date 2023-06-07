---
title: 处理部分失败的策略
description: 了解妥善处理部分失败错误的几种策略。
ms.date: 10/16/2018
ms.openlocfilehash: abf87df5afed02b4d794a1307a0ed943cafb4db3
ms.sourcegitcommit: e3cbf26d67f7e9286c7108a2752804050762d02d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/09/2020
ms.locfileid: "80988799"
---
# <a name="strategies-to-handle-partial-failure"></a>处理部分失败错误的策略

处理部分失败的策略如下。

**在内部微服务中使用异步通信（例如，基于消息的通信）** 。 强烈建议不要跨内部微服务创建长链同步 HTTP 调用，因为此错误设计会最终成为中断的主要原因。 与此相反，建议跨内部微服务情况下，仅在经过初始请求/响应周期后，使用异步（基于消息）通信，但客户端应用程序和第一级微服务或细化 API 网关之间的前端通信除外。 最终一致性和事件驱动的体系结构有助于尽可能减少连锁反应。 这些方法强制执行较高级别的微服务自治，从而防止此处提到的问题发生。

**采用使用指数回退算法的重试**。 这种技术有助于通过执行特定次数的调用重试避免短时间的间歇性故障，以防仅短时间内服务不可用。 此问题可能是因为间歇性网络问题或微服务/容器移动到群集中的不同节点造成的。 但是，如果这些重试没有针对断路器恰当设计，可能加剧连锁反应，最终导致[拒绝服务 (DoS)](https://en.wikipedia.org/wiki/Denial-of-service_attack)。

**暂时避开网络超时**。 通常情况下，客户端应设计为不会无限期阻止，且应在等待响应时使用超时。 使用超时可确保永远不会无限期地占用资源。

**使用断路器模式**。 在此方法中，客户端进程跟踪失败请求数。 如果错误率超出配置的限制，“断路器”跳变，使进一步尝试立即失败。 （如果大量请求失败，则表明服务不可用，发送请求无意义。）超时期限过后，客户端应重试，如果新的请求成功，关闭断路器。

**提供回退**。 在此方法中，客户端进程在请求失败时执行回退逻辑，如返回缓存数据或默认值。 这是适用于查询的一种方法，对于更新或命令较复杂。

**限制排队请求数**。 客户端还应对客户端微服务可发送到特定服务的未完成请求的数量设置上限。 如果已达到限制，发出其他请求可能无意义，这些尝试应立即失败。 对于实现，Polly [Bulkhead 隔离](https://github.com/App-vNext/Polly/wiki/Bulkhead)策略可以用于满足此要求。 这种方法实质上是将 <xref:System.Threading.SemaphoreSlim> 作为实现的平行化限制。 它也允许 bulkhead 外的“队列”。 可在执行前主动舍弃额外的负载（例如，由于容量被视为已满）。 这样一来，其对某些失败情景的响应速度比断路器要快，因为断路器会等待失败。 [Polly](http://www.thepollyproject.org/) 中的 BulkheadPolicy 对象公开 bulkhead 和队列的已满程度，且提供有关溢出的事件，因此也可用于驱动自动水平缩放。

## <a name="additional-resources"></a>其他资源

- **复原模式**\
  [https://docs.microsoft.com/azure/architecture/patterns/category/resiliency](/azure/architecture/patterns/category/resiliency)

- **添加复原能力和优化性能**\
  <https://docs.microsoft.com/previous-versions/msp-n-p/jj591574(v=pandp.10)>

- **Bulkhead。** GitHub 存储库。 使用 Polly 策略的实现。\
  <https://github.com/App-vNext/Polly/wiki/Bulkhead>

- **为 Azure 设计复原应用程序**\
  [https://docs.microsoft.com/azure/architecture/resiliency/](/azure/architecture/resiliency/)

- **瞬时故障处理**\
  [https://docs.microsoft.com/azure/architecture/best-practices/transient-faults](/azure/architecture/best-practices/transient-faults)

>[!div class="step-by-step"]
>[上一页](handle-partial-failure.md)
>[下一页](implement-retries-exponential-backoff.md)
