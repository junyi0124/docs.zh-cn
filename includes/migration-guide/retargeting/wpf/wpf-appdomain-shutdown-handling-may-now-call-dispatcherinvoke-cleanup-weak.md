---
ms.openlocfilehash: 8b804d23247b95381f3ff83bc12c32215a420fb6
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/01/2020
ms.locfileid: "85614413"
---
### <a name="wpf-appdomain-shutdown-handling-may-now-call-dispatcherinvoke-in-cleanup-of-weak-events"></a>WPF AppDomain 关闭处理现在可以调用 Dispatcher.Invoke 以清理弱事件

#### <a name="details"></a>详细信息

在 .NET Framework 4.7.1 及更早的版本中，在 AppDomain 关闭期间，WPF 可能会在 .NET 终结器线程上创建 <xref:System.Windows.Threading.Dispatcher?displayProperty=nameWithType>。  此问题已在 .NET Framework 4.7.2 和更高版本中得到修复，方法是使弱事件的清理能够感知线程。  因此，WPF 可能会调用 <xref:System.Windows.Threading.Dispatcher.Invoke%2A?displayProperty=nameWithType> 来完成清理过程。在某些应用程序中，终结器计时的这一更改可能在 AppDomain 或进程关闭期间导致异常。  这种情况常见于未在进程或 AppDomain 关闭之前正确关闭在工作线程上运行的调度程序的应用程序中。  此类应用程序应负责正确管理调度程序的生存期。

#### <a name="suggestion"></a>建议

在 .NET Framework 4.7.2 及更高版本中，开发人员可以禁用此修补程序以帮助缓解（但不能消除）由于清理更改而导致的计时问题。若要禁用清理中的更改，请使用以下 AppContext 标志。<pre><code class="lang-xml">&lt;configuration&gt;&#13;&#10;&lt;runtime&gt;&#13;&#10;&lt;AppContextSwitchOverrides value=&quot;Switch.MS.Internal.DoNotInvokeInWeakEventTableShutdownListener=true&quot;/&gt;&#13;&#10;&lt;/runtime&gt;&#13;&#10;&lt;/configuration&gt;&#13;&#10;</code></pre>

| “属性”    | “值”       |
|:--------|:------------|
| 范围   | 边缘        |
| Version | 4.7.2       |
| 类型    | 重定目标 |
