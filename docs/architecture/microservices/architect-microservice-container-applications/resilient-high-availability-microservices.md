---
title: 微服务中的复原和高可用性
description: 微服务必须能够承受暂时的网络和依赖项故障，必须可复原以实现高可用性。
ms.date: 09/20/2018
ms.openlocfilehash: 601255c1e6941b2de9fdb34098dea7edf6d8b987
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91172445"
---
# <a name="resiliency-and-high-availability-in-microservices"></a>微服务中的复原和高可用性

意外故障处理是最难解决的问题之一，尤其是在分布系统之中。 开发人员编写的大多数代码都包括处理异常，这也是测试中花费时间最多的地方。 这个问题比编写代码处理故障更为复杂。 计算机中微服务运行失败时会出现哪些情况？ 不仅需要检测此次微服务失败（这本身就是一个难题），还需要采取某些措施以重新启动微服务。

微服务需要具有应对故障的复原能力，并且能够经常在另一台计算机上重新启动以获得可用性。 这种复原还归结于代表微服务保存的状态（微服务可从中恢复此状态）和微服务是否可以成功重新启动。 换言之，这需要计算机功能中的复原能力（进程随时可重新启动），还需要状态或数据的复原能力（不会丢失数据并且数据保持一致）。

在其他情况下，复原问题会更加复杂，例如在应用程序升级时发生此故障。 与部署系统一起工作的微服务需要确定它应该继续更新到新版本还是应该回滚到之前的版本，才能保持一致的状态。 需要考虑的问题如：是否有足够的计算机可继续更新以及如何恢复微服务的以前版本。 这要求微服务发出运行状况信息，使整个应用程序和业务流程协调程序可以做出以上决定。

此外，复原还与基于云的系统的必要行为方式有关。 如前所述，基于云的系统必须包含故障，必须尝试从故障中自动恢复。 例如，如果发生网络或容器故障，客户端应用或客户端服务必须有重试发送信息或重试请求的策略，因为在多数情况下，云中的故障仅是部分故障。 本指南中的[实现具有恢复能力的应用程序](../implement-resilient-applications/index.md)一节介绍了部分故障的处理方式。 该部分通过使用库（如[Polly](https://github.com/App-vNext/Polly)，其中提供了多种策略处理此类问题）介绍了诸如指数退避重试或 .NET Core 中的断路器模式的技术。

## <a name="health-management-and-diagnostics-in-microservices"></a>微服务中的运行状况管理和诊断

微服务必须报告其运行状况和诊断，这可能看起来显而易见，但时常被忽视。 否则，从操作角度来说，几乎没有见解。 关联一组独立服务中的诊断事件和处理计算机时钟扭曲以了解事件顺序是很有难度的。 和通过商定的协议和数据格式与微服务交互的方式一样，这需要一个标准方式来记录运行状况和诊断事件，最终记录到事件存储中供查询和查看。 对于微服务方法，其关键在于不同团队达成单个日志记录格式。 还需要一种一致的方法在应用程序中查看诊断事件。

### <a name="health-checks"></a>运行状况检查

运行状况不同于诊断。 运行状况是微服务报告其当前状态以采取适当的措施。 一个很好的例子就是使用升级和部署机制来保证可用性。 即使可能由于进程崩溃或计算机重启导致当前服务不正常，仍可对服务器进行操作。 最不需要做的就是执行升级，那样会让情况变得更糟。 最佳方法是先调查或给微服务恢复的时间。 微服务的运行状况事件能帮助做出明智的决定，实际上是帮助创建自愈服务。

本指南的[在 ASP.NET Core 服务中实现运行状况检查](../implement-resilient-applications/monitor-app-health.md#implement-health-checks-in-aspnet-core-services)一节介绍了如何在微服务中使用新 ASP.NET 运行状况检查库，使微服务可向监视器服务报告它们的状态，以执行适当的操作。

还可以使用出色的开源库 Beat Pulse，该库可通过 [GitHub](https://github.com/Xabaril/BeatPulse) 作为 [NuGet 包](https://www.nuget.org/packages/BeatPulse/) 获取。 此库还执行经过改进的运行状况检查，它可以处理两种类型的检查：

- **运行情况**：检查微服务是否处于活动状态，即微服务是否接受请求并作出响应。
- **就绪情况**：检查微服务的依赖项（数据库、查询服务等）是否已就绪，以便微服务可以执行预期操作。

### <a name="using-diagnostics-and-logs-event-streams"></a>使用诊断和日志事件流

日志提供应用程序或服务的运行方式的相关信息，其中包括异常、警告和简单的信息。 通常，每篇日志都是文本格式，每个事件一行，但异常经常显示跨越多行的堆栈跟踪。

在基于服务器的整体式应用程序中，可简单地将日志写入磁盘上的文件（日志文件），然后使用任意工具对其进行分析。 因为应用程序限制在固定的服务器或 VM 上执行，所以它通常不会太过复杂而导致无法分析事件流。 但在分布式应用程序中，多个服务在业务流程协调程序群集中的多数节点上执行，因此关联分布式事件是一项挑战。

基于微服务的应用程序不应尝试自行存储事件或日志文件的输出流，甚至不应尝试管理事件到中心位置的路由。 它应该是透明的，即每个进程只需要将其事件流写入一个标准输出，该标准输出将由运行的执行环境基础结构收集。 事件流路由器的一个示例为 [Microsoft.Diagnostic.EventFlow](https://github.com/Azure/diagnostics-eventflow)，该路由器从多个源收集事件流，并将其发布到输出系统。 这些可以包括用于开发环境或云系统（如 [Azure Monitor](https://azure.microsoft.com/services/monitor//) 和 [Azure 诊断](/azure/azure-monitor/platform/diagnostics-extension-overview)）的简单标准输出。 也有一些好的第三方日志分析平台和工具（如 [Splunk](https://www.splunk.com/goto/Splunk_Log_Management?ac=ga_usa_log_analysis_phrase_Mar17&_kk=logs%20analysis&gclid=CNzkzIrex9MCFYGHfgodW5YOtA)），它们甚至可以实时搜索、提示、报告和监视日志。

### <a name="orchestrators-managing-health-and-diagnostics-information"></a>管理运行状况和诊断信息的业务流程协调程序

创建基于微服务的应用程序时，需要处理其复杂性。 当然，单个微服务的处理很简单，但处理微服务的数十个或数百个类型以及数千个实例则十分复杂。 这不仅是构建微服务体系结构，如果想要一个稳定紧密的系统，还需要具有高可用性、可寻址能力、复原能力、运行状况和诊断功能。

![为微服务提供支持平台的群集的示意图。](./media/resilient-high-availability-microservices/microservice-platform.png)

**图 4-22**。 微服务平台是应用程序运行状况管理的基础

图 4-22 所示的复杂问题很难自行解决。 开发团队应侧重于解决商业问题和构建使用基于微服务方法的自定义应用程序。 他们不应侧重于解决复杂的基础结构问题，因为如果他们这样做，任何基于微服务的应用程序都将耗费巨大成本。 因此，出现了面向微服务的平台（称为业务流程协调程序或微服务群集），这些平台尝试解决构建和运行服务的困难问题，并尝试有效地使用基础结构资源。 这降低了构建使用微服务方法的应用程序的复杂程度。

不同的业务流程协调程序可能听起来相似，但它们各自提供的诊断和运行状况检查都在功能和成熟状态上不同，有时这取决于 OS 平台，如下节所述。

## <a name="additional-resources"></a>其他资源

- **The Twelve-Factor App.XI.日志：将日志视为事件流** \
  <https://12factor.net/logs>

- Microsoft Diagnostic EventFlow 库 GitHub 存储库  。 \
  <https://github.com/Azure/diagnostics-eventflow>

- 什么是 Azure 诊断   \
  <https://docs.microsoft.com/azure/azure-diagnostics>

- **将 Windows 计算机连接到 Azure Monitor 服务** \
  <https://docs.microsoft.com/azure/azure-monitor/platform/agent-windows>

- **记录你的想法：使用语义日志记录应用程序块** \
  <https://docs.microsoft.com/previous-versions/msp-n-p/dn440729(v=pandp.60)>

- Splunk 官方网站  。 \
  <https://www.splunk.com/>

- Windows 事件跟踪 (ETW) 的 EventSource 类 API  \
  [https://docs.microsoft.com/dotnet/api/system.diagnostics.tracing.eventsource](xref:System.Diagnostics.Tracing.EventSource)

>[!div class="step-by-step"]
>[上一页](microservice-based-composite-ui-shape-layout.md)
>[下一页](scalable-available-multi-container-microservice-applications.md)
