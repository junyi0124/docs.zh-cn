---
ms.openlocfilehash: 7a7ecf052276fe8e62463498b83230d466630c79
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/01/2020
ms.locfileid: "85617787"
---
### <a name="workflow-xoml-definition-and-sqltrackingservice-cache-keys-changed-from-md5-to-sha256"></a>工作流 XOML 定义和 SqlTrackingService 缓存密钥已从 MD5 更改为 SHA256

#### <a name="details"></a>详细信息

工作流运行时保存 XOML 中定义的工作流定义的缓存。 SqlTrackingService 还保留按字符串键入的缓存。 由包含校验和哈希值的值键入这些缓存。 在 .NET Framework 4.7.2 和早期版本中，该校验和哈希使用 MD5 算法，这会在启用 FIPS 的系统上导致问题。 从 .NET Framework 4.8 开始，使用的算法为 SHA256。由于每次工作流运行时和 SqlTrackingService 启动时都会重新计算值，因此进行此更改后不会出现兼容性问题。 但是，我们提供了可以让客户恢复使用旧版哈希算法的例外做法（如有必要）。

#### <a name="suggestion"></a>建议

如果此更改在执行工作流时出现问题，请尝试设置一个或两个 `AppContext` 开关：

- 将 &quot;Switch.System.Workflow.Runtime.UseLegacyHashForWorkflowDefinitionDispenserCacheKey&quot; 设为 true。
- 将 &quot;Switch.System.Workflow.Runtime.UseLegacyHashForSqlTrackingCacheKey&quot; 设为 true。
在代码中：

<pre><code class="lang-csharp">System.AppContext.SetSwitch(&quot;Switch.System.Workflow.Runtime.UseLegacyHashForWorkflowDefinitionDispenserCacheKey&quot;, true);&#13;&#10;System.AppContext.SetSwitch(&quot;Switch.System.Workflow.Runtime.UseLegacyHashForSqlTrackingCacheKey&quot;, true);&#13;&#10;</code></pre>

或者在配置文件中（这需要在创建 <xref:System.Workflow.Runtime.WorkflowRuntime> 对象的应用程序的配置文件中）：

<pre><code class="lang-xml">&lt;configuration&gt;&#13;&#10;&lt;runtime&gt;&#13;&#10;&lt;AppContextSwitchOverrides value=&quot;Switch.System.Workflow.Runtime.UseLegacyHashForWorkflowDefinitionDispenserCacheKey=true&quot; /&gt;&#13;&#10;&lt;AppContextSwitchOverrides value=&quot;Switch.System.Workflow.Runtime.UseLegacyHashForSqlTrackingCacheKeytrue&quot; /&gt;&#13;&#10;&lt;/runtime&gt;&#13;&#10;&lt;/configuration&gt;&#13;&#10;</code></pre>

| “属性”    | “值”       |
|:--------|:------------|
| 范围   | 次要       |
| Version | 4.8         |
| 类型    | 重定目标 |
