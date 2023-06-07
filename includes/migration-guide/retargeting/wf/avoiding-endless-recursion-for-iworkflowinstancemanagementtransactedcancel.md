---
ms.openlocfilehash: ab9431780422dfece5dcf8007d13e6d584f5118f
ms.sourcegitcommit: 721c3e4bdbb1ea0bb420818ec944c538fe5c513a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/01/2020
ms.locfileid: "96478014"
---
### <a name="avoiding-endless-recursion-for-iworkflowinstancemanagementtransactedcancel-and-iworkflowinstancemanagementtransactedterminate"></a>避免 IWorkflowInstanceManagement.TransactedCancel 和 IWorkflowInstanceManagement.TransactedTerminate 无限递归

#### <a name="details"></a>详细信息

在某些情况下，使用 <xref:System.ServiceModel.Activities.IWorkflowInstanceManagement.TransactedCancel%2A?displayProperty=nameWithType> 或 <xref:System.ServiceModel.Activities.IWorkflowInstanceManagement.TransactedTerminate%2A?displayProperty=nameWithType> API 取消或终止工作流服务实例时，由于 `Workflow` 运行时尝试将服务实例永久保存为处理请求的一部分时出现无限递归，工作流实例可能会遇到堆栈溢出情况。 如果工作流实例处于等待另一项服务的其他未完成 WCF 请求完成的状态，则会出现此问题。 `TransactedCancel` 和 `TransactedTerminate` 操作会创建排队等待工作流服务实例的工作项。 这些工作项目不会作为 `TransactedCancel/TransactedTerminate` 请求处理的一部分执行。 由于工作流服务实例正忙于等待其他未完成 WCF 请求的完成，所以创建的工作项继续排队。 `TransactedCancel/TransactedTerminate` 操作完成且控件返回到客户端。 与 `TransactedCancel/TransactedTerminate` 操作相关的事务尝试提交时，它需保存工作流服务实例状态。 但是，由于该实例有未完成的 `WCF` 请求，所以工作流运行时无法保存工作流服务实例，并且无限递归循环会导致堆栈溢出。因为 `TransactedCancel` 和 `TransactedTerminate` 仅在内存中创建工作项，所以不影响事务存在这一事实。 事务回滚不会放弃工作项。为了解决此问题，自 .NET Framework 4.7.2 起，我们引入了 `AppSetting`，可将其添加到工作流服务的 `web.config/app.config`，使其忽略 `TransactedCancel` 和 `TransactedTerminate` 的事务。 这样，不必等待工作流实例保留即可提交事务。 此功能的 AppSetting 名为 `microsoft:WorkflowServices:IgnoreTransactionsForTransactedCancelAndTransactedTerminate`。 `true` 值表示应忽略事务，从而避免堆栈溢出。 此 AppSetting 的默认值是 `false`，因此不影响现有工作流服务实例。

#### <a name="suggestion"></a>建议

如果正在使用 AppFabric 或另一个 <xref:System.ServiceModel.Activities.IWorkflowInstanceManagement> 客户端，并且在尝试取消或终止工作流实例时遇到工作流服务实例中堆栈溢出问题，那么可以将以下内容添加到工作流服务 web.config/app.config 文件的 `<appSettings>` 部分：

<pre><code class="lang-xml">&lt;add key=&quot;microsoft:WorkflowServices:IgnoreTransactionsForTransactedCancelAndTransactedTerminate&quot; value=&quot;true&quot;/&gt;&#13;&#10;</code></pre>

如果没有遇到问题，则不需要执行此操作。

| “属性”    | “值”       |
|:--------|:------------|
| 范围   | 边缘        |
| Version | 4.7.2       |
| 类型    | 重定目标 |
