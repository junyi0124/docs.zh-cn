---
title: 活动 ID 传播
ms.date: 03/30/2017
ms.assetid: cd1c1ae5-cc8a-4f51-90c9-f7b476bcfe70
ms.openlocfilehash: 0f0478b16bf2ca0975ae0290a8855756ecfc383e
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96236099"
---
# <a name="activity-id-propagation"></a>活动 ID 传播

当 ServiceModel 活动跟踪启用（ServiceModel 传播）或禁用（用户对用户活动跟踪）时，都会发生传播。  
  
## <a name="servicemodel-activity-tracing-is-enabled"></a>ServiceModel 活动跟踪启用  

 如果启用 ServiceModel ActivityTracing，传播将在 ProcessAction 活动之间发生。  
  
### <a name="server"></a>服务器  

 当 `propagateActivity` `true` 客户端和服务器上的属性设置为时， `ProcessAction` 服务器中的活动 id 与传播的消息头中的 id 相同 `ActivityId` 。  
  
 如果 `ActivityId` 消息中不存在任何标头 (即， `propagateActivity` = `false` 在客户端) 上或 `propagateActivity` = `false` 在服务器上时，服务器将生成新的活动 ID。  
  
### <a name="client"></a>客户端  

 如果客户端是同步单线程，则客户端忽略客户端或服务器上 `propagateActivity` 的任何设置。 响应则改为在请求活动中处理。 如果客户端是异步或同步多线程，则 `propagateActivity` = `true` 在客户端中，服务器发送的消息中有活动 id 标头，客户端将从消息中检索活动 id，并传输到包含传播的活动 ID 的 "进程操作" 活动。 否则，客户端从“进程管理”活动转移到新的“进程操作”活动。 完成到新“进程操作”活动的这一额外转移是为了实现一致性。 在此活动内，如果为响应消息处理分配了线程，则客户端从本地线程上下文检索 BeginCall 活动的活动 ID。 然后，客户端转移到初始“进程操作”活动。  
  
 如果客户端为双工，则客户端在接收消息时用作服务器。  
  
### <a name="propagation-in-fault-messages"></a>错误消息中的传播  

 有效消息和错误消息的处理没有任何区别。 如果为 `propagateActivity` = `true` ，则添加到 SOAP 错误消息标头的活动 ID 等同于环境活动。  
  
## <a name="servicemodel-activity-tracing-is-disabled"></a>ServiceModel 活动跟踪禁用  

 如果 ServiceModel ActivityTracing 禁用，传播将在用户代码活动之间直接发生，而不经过 ServiceModel 活动。 用户定义的活动 ID 也通过消息活动 ID 标头传播。  
  
 如果对 `propagateActivity` = `true` `ActivityTracing` = `off` 工作的跟踪 (侦听器，无论是否在 system.servicemodel) 上启用跟踪，无论是否启用了跟踪，都将发生以下情况：  
  
- 在操作请求或发送响应时，TLS 中的活动 ID 从用户代码传播出去，直至构成消息。 活动 ID 标头也在消息发送之前插入消息中。  
  
- 接收到请求或响应时，只要创建了接收的消息对象，即从消息头中检索活动 ID。 消息从作用域中一消失，TLS 中的活动 ID 就会进行传播，直至达到用户代码。  
  
 这些操作保证了在启用传播的情况下，用户跟踪将出现在相同的活动中。 但是，这不保证 ServiceModel 跟踪。 仅当 ServiceModel 跟踪处理是在设置用户代码活动的线程中执行时，这些跟踪才会出现在用户代码活动中。  
  
 一般而言，在以下位置可以观察到 ServiceModel 跟踪：  
  
- 如果禁用 ServiceModel 跟踪，则发出的所有跟踪都将出现在用户活动中。 如果服务器和客户端上都启用传播，则这些跟踪将在同一个活动中。  
  
- 如果启用了 ServiceModel 跟踪，但禁用了 ActivityTracing，并且服务器和客户端都启用传播，则用户跟踪将出现在同一个活动中。 ServiceModel 跟踪出现在默认的 0000 活动中，除非这些跟踪发生在与最初设置该活动的用户代码处理相同的线程中。  
  
- 如果启用了 ServiceModel 跟踪和 ActivityTracing，则用户跟踪将出现在用户定义的活动中，而 ServiceModel 跟踪则出现在 ServiceModel 定义的活动中。 传播在 ServiceModel 级别发生。
