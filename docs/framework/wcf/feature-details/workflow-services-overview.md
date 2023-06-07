---
title: 工作流服务概述
ms.date: 03/30/2017
ms.assetid: e536dda3-e286-441e-99a7-49ddc004b646
ms.openlocfilehash: 7055ea6e6b6d6a5d7bef8d5ff465d2eb0c838bf6
ms.sourcegitcommit: 9c45035b781caebc63ec8ecf912dc83fb6723b1f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/25/2020
ms.locfileid: "88812180"
---
# <a name="workflow-services-overview"></a>工作流服务概述

工作流服务是使用工作流实现的基于 WCF 的服务。 工作流服务是使用消息传递活动发送和接收 Windows Communication Foundation (WCF) 消息的工作流。 .NET Framework 4.5 引入了多个消息传递活动，可用于从工作流中发送和接收消息。 有关消息传递活动以及如何使用消息传递活动来实现不同消息交换模式的详细信息，请参阅 [消息传递活动](messaging-activities.md)。

## <a name="benefits-of-using-workflow-services"></a>使用工作流服务的好处

随着应用程序的分布越来越广泛，各服务可负责调用其他服务来减轻工作负载。 作为异步操作实现这些调用会增加代码的复杂性。 错误处理需要处理异常和提供详细跟踪信息，从而进一步增加了这些形式的复杂性。 有些服务经常长时间运行，并且在等待输入时可能会占用宝贵的系统资源。 鉴于这些问题，分布式应用程序往往非常复杂，且难以编写和维护。 工作流很自然地表示异步工作的协调，特别是对外部服务的调用。 工作流也是表示长时间运行的业务流程的有效方式。 正是这些特性使得工作流成为了在分布式环境中生成服务的一笔宝贵财富。

## <a name="implementing-a-workflow-service"></a>实现工作流服务

实现 WCF 服务时，您定义了许多协定，用于描述服务以及它所发送和接收的数据。 这些数据表示为数据协定和消息协定。 WCF 和工作流服务均在服务说明中使用数据协定和消息协定定义。 服务自身以 WSDL 形式公开元数据，以便描述服务的操作。 在 WCF 中，服务协定和操作协定定义其支持的服务和操作。 但在工作流服务中，这些协定属于业务流程自身。 它们由称为协定推理的流程在元数据中公开。 使用 <xref:System.ServiceModel.Activities.WorkflowServiceHost> 承载工作流服务时，将检查工作流定义，并根据在工作流中找到的消息传递活动集生成协定。 具体而言，是使用下面的活动和属性来生成协定：

<xref:System.ServiceModel.Activities.Receive> 活动

- <xref:System.ServiceModel.Activities.Receive.ServiceContractName%2A>

- <xref:System.ServiceModel.Activities.Receive.OperationName%2A>

- <xref:System.ServiceModel.Activities.Receive.Action%2A>

<xref:System.ServiceModel.Activities.SendReply> 活动

- <xref:System.ServiceModel.Activities.SendReply.Action%2A>

<xref:System.ServiceModel.Activities.TransactedReceiveScope> 活动

协定推理的最终结果是使用与 WCF 服务和操作协定相同的数据结构的服务说明。 然后，将使用此信息对工作流服务公开 WSDL。

> [!NOTE]
> [!INCLUDE[netfx_current_short](../../../../includes/netfx-current-short-md.md)] 禁止使用不带某些附加工具支持的现有协定定义来编写工作流服务。 工作流服务协定由前面讨论的协定推理流程创建。 不过，消息协定和数据协定完全受到支持。

## <a name="workflow-services-and-msmq-based-bindings"></a>工作流服务和基于 MSMQ 的绑定

WCF 定义了两个基于 MSMQ 的绑定：<xref:System.ServiceModel.NetMsmqBinding> 和 <xref:System.ServiceModel.MsmqIntegration.MsmqIntegrationBinding>。  基于 MSMQ 的绑定具有长时间运行的性质，因此常常与工作流服务一起使用。 基于 MSMQ 的绑定具有一个 `ValidityDuration` 属性，该属性指定可以假定 MSMQ 消息有效的时间。 由于工作流服务具有长时间运行的性质，因此在工作流服务可以处理 MSMQ 消息之前，该消息可能已过期。 因此，将 MSMQ 绑定的有效期设置为一个适当值非常重要。 必须基于工作流及其对消息的处理方式选择此值。 例如，如果工作流具有一个 <xref:System.ServiceModel.Activities.Receive> 活动，后跟一个需要运行 10 分钟的自定义活动，再后跟另一个 <xref:System.ServiceModel.Activities.Receive> 活动，则 `ValidityDuration` 的正确值将大于 10 分钟。

## <a name="hosting-a-workflow-service"></a>承载工作流服务

与 WCF 服务类似，必须承载工作流服务。 WCF 服务使用 <xref:System.ServiceModel.ServiceHost> 类承载服务和工作流服务， <xref:System.ServiceModel.Activities.WorkflowServiceHost> 以便使用宿主服务。 与 WCF 服务类似，工作流服务可以通过多种方式来托管，例如：

- 在托管的 .NET Framework 应用程序中。

- 在 Internet Information Services (IIS) 中。

- 在 Windows 进程激活服务 (WAS) 中。

- 在托管 Windows 服务中。

托管 .NET Framework 应用程序或托管的 Windows 服务中承载的工作流服务创建类的实例 <xref:System.ServiceModel.Activities.WorkflowServiceHost> ，并向其传递 <xref:System.ServiceModel.Activities.WorkflowService> 包含属性内的工作流定义的实例 <xref:System.ServiceModel.Activities.WorkflowService.Body%2A> 。 包含消息传递活动的工作流定义公开为工作流服务。

若要在 IIS 或 WAS 中承载工作流服务，请将包含工作流服务定义的 .xamlx 文件放置到虚拟目录中。 将自动创建 (使用) 的默认终结点 <xref:System.ServiceModel.BasicHttpBinding> 。有关详细信息，请参阅 [简化配置](../simplified-configuration.md)。 也可以在虚拟目录中放置 Web.config 文件以指定你自己的终结点。 如果您的工作流定义位于程序集中，则可以在虚拟目录中放置一个 .svc 文件，并在 App_Code 目录中放置该工作流程序集。 该 .svc 文件必须指定服务主机工厂以及实现该工作流服务的类。 下面的示例演示如何指定服务主机工厂以及实现该工作流服务的类。

```aspx-csharp
<%@ServiceHost Factory="System.ServiceModel.Activities.Activation.WorkflowServiceHostFactory"
Service="EchoService"%>
```
