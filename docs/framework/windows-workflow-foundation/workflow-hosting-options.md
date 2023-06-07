---
title: 工作流托管选项
ms.date: 03/30/2017
ms.assetid: 37bcd668-9c5c-4e7c-81da-a1f1b3a16514
ms.openlocfilehash: 8ddb83f068eab8480bacc8b80bc5d44b7755fa59
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96293775"
---
# <a name="workflow-hosting-options"></a>工作流托管选项

大多数 Windows Workflow Foundation (WF) 示例使用托管在控制台应用程序中的工作流，但这不是实际工作流的现实方案。 实际业务应用程序中的工作流将托管在持久的进程中，无论是开发人员编写的 Windows 服务，还是 IIS 7.0 或 AppFabric 等服务器应用程序。 这些方法之间的区别如下。

## <a name="hosting-workflows-in-iis-with-windows-appfabric"></a>用配有 Windows AppFabric 的 IIS 承载工作流

用配有 AppFabric 的 IIS 是承载工作流的首选方法。 用 AppFabric 来承载工作流的应用程序是 Windows Activation Service，它解除了仅通过 IIS 对 HTTP 的依赖。

## <a name="hosting-workflows-in-iis-alone"></a>仅用 IIS 来承载工作流

建议不要使用 IIS 7.0，因为 AppFabric 提供了一些管理和监视工具，可帮助维护正在运行的应用程序。 如果在迁移到 AppFabric 时出现基础结构问题，则仅应在 IIS 7.0 中承载工作流。

> [!WARNING]
> IIS 7.0 会出于各种原因定期回收应用程序池。 当应用程序池被回收时，IIS 即停止接受发给旧应用程序池的消息，并实例化一个新的应用程序池以接受新请求。 如果工作流在发送响应后继续工作，则 IIS 7.0 将不知道正在执行的工作，并可能回收宿主应用程序池。 如果发生这种情况，则工作流将中止，并且跟踪服务将记录 [1004 一个 WorkflowInstanceAborted](1004-workflowinstanceaborted.md) 消息，其中包含空的原因字段。
>
> 如果使用了持久化机制，则主机必须显式地从上一个持久化点重启被中止的实例。
>
> 如果使用了 AppFabric，则工作流管理服务最终将从上次成功持久化的点恢复工作流（如果使用了持久化机制）。 如果未使用持久化机制，则该工作流执行非“请求/响应”模式的操作，当工作流中止时，数据也将丢失。

## <a name="hosting-a-workflow-in-a-custom-windows-service"></a>在自定义 Windows 服务中承载工作流

若要创建自定义工作流服务来承载工作流，要求开发人员复制 AppFabric 自带的许多功能，但允许通过自定义功能实现更大的灵活性。 仅当 AppFabric 经证不合适时，才应考虑这一选项。
