---
title: 生成适用于云的可复原服务。 在云中处理暂时性故障
description: 通过 Azure 云和 Windows 容器现代化现有 .NET 应用程序 | 生成适用于云的可复原服务。 在云中处理暂时性故障
ms.date: 04/30/2018
ms.openlocfilehash: 8e9f1eda71e4b98a56cbfc1c7a4ff34e67bee3f4
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91172152"
---
# <a name="build-resilient-services-ready-for-the-cloud-embrace-transient-failures-in-the-cloud"></a>生成适用于云的可复原服务：在云中处理暂时性故障

恢复能力是指从故障中恢复并继续工作的能力。 复原能力并不是指避免故障，而是接受会发生故障这一事实，然后以能够避免停机或数据丢失的方式对故障做出响应。 恢复的目标是使应用程序在发生故障后回到完全正常运行的状态。

当你的应用程序至少实现了基于软件的复原能力模型（而不是基于硬件的模型）时，你的应用程序即处于云就绪状态。 你的云应用程序必须接受将会出现的部分故障。 设计或部分重构你的应用程序，以实现预期部分故障的复原能力。 应将其设计为能够处理部分故障，如短暂的网络中断或者云中的节点或 VM 故障。 甚至将容器移动到业务流程协调程序群集内的另一个节点，也可能导致应用程序出现间歇性的功能短缺故障。

## <a name="handling-partial-failure"></a>处理部分失败错误

在基于云的应用程序中，存在部分故障的现有风险。 例如，单个网站实例或容器可能出现故障，或者它可能短时间不可用或没有响应。 或者，单个 VM 或服务器可能会出现故障。

由于客户端和服务是彼此独立的流程，因此服务可能无法及时响应客户端的请求。 服务可能过载并且对请求的响应速度慢，或者由于网络问题在短时间内无法访问。

例如，假设有一个在 Azure SQL 数据库中访问数据库的整体式 .NET 应用程序。 如果 Azure SQL 数据库或任何其他第三方服务短时间内没有响应（Azure SQL 数据库可能移动到另一个节点或服务器，在几秒钟内无响应），则当用户尝试执行任何操作时，应用程序可能会出现故障并同时显示异常。

使用 HTTP 服务的应用可能会出现类似的情况。 在出现短暂的暂时性故障时，网络或服务本身可能无法在云中使用。

如图 4-9 中所示的可复原应用程序应该实现“以指数回退方式重试”之类的技术，使应用程序有机会处理资源的暂时性故障。 你还应在应用程序中使用“断路器”。 当实际上是长期故障时，断路器会阻止该应用程序尝试访问该资源。 通过使用断路器，应用程序可避免造成自身的拒绝服务。

![通过以指数回退方式重试处理的部分故障的关系图。](./media/retry-partial-failures.png)

**图 4-9.** 。 通过以指数回退方式重试处理的部分故障

可以在 HTTP 资源和数据库资源中使用这些技术。 在图 4-9 中，应用程序基于 3 层体系结构，因此，你需要在服务级别 (HTTP) 和数据级别 (TCP) 上使用这些方法。 在除数据库之外仅使用单个应用层（无其他服务或微服务）的整体式应用程序中，处理数据库连接级别的暂时性故障可能就足够了。 在这种情况下，只需要数据库连接的特定配置。

实现访问数据库的可复原通信时，根据所使用的 .NET 版本，过程可能非常直接（例如[带有实体框架 6 或更高版本](/ef/ef6/fundamentals/connection-resiliency/retry-logic)。 只需配置数据库连接即可）。 或者，你可能需要使用其他库，如[暂时性故障处理应用程序块](/previous-versions/msp-n-p/hh680934(v=pandp.50))（适用于早期版本的 .NET），甚至实现你自己的库。

实现 HTTP 重试和断路器时，适用于 .NET 的建议使用 [Polly](https://github.com/App-vNext/Polly) 库，此库面向 .NET Framework 4.0、.NET Framework 4.5 和 .NET Standard 1.1，其中包括 .NET Core 支持。

若要了解如何实现用于处理云中部分故障的策略，请参阅以下参考。

### <a name="additional-resources"></a>其他资源

- **实现可复原通信以处理部分故障**

    [https://docs.microsoft.com/dotnet/architecture/microservices/implement-resilient-applications/partial-failure-strategies](../../microservices/implement-resilient-applications/partial-failure-strategies.md)

- **实体框架连接复原能力和重试逻辑（版本 6 及更高版本）**

    [https://docs.microsoft.com/ef/ef6/fundamentals/connection-resiliency/retry-logic](/ef/ef6/fundamentals/connection-resiliency/retry-logic)

- **暂时性故障处理应用程序块**

- <https://docs.microsoft.com/previous-versions/msp-n-p/hh680934(v=pandp.50)>

- **用于复原 HTTP 通信的 Polly 库**

    <https://github.com/App-vNext/Polly>

>[!div class="step-by-step"]
>[上一页](when-to-deploy-windows-containers-to-azure-container-service-kubernetes.md)
>[下一页](modernize-your-apps-with-monitoring-and-telemetry.md)
