---
ms.openlocfilehash: f50022d9a7bacd7be40fe3050ced26e7c25cf7aa
ms.sourcegitcommit: cbacb5d2cebbf044547f6af6e74a9de866800985
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/05/2020
ms.locfileid: "89497222"
---
### <a name="crash-in-selector-when-removing-an-item-from-a-custom-incc-collection"></a>从自定义 INCC 集合删除项时选择器出现故障

#### <a name="details"></a>详细信息

<code>T:System.InvalidOperationException</code> 会在以下方案中发生：<ul><li><code>T:System.Windows.Controls.Primitives.Selector</code> 的 ItemsSource 是 <code>T:System.Collections.Specialized.INotifyCollectionChanged</code> 自定义实现的集合。</li><li>从集合中删除所选的项。</li><li><code>T:System.Collections.Specialized.NotifyCollectionChangedEventArgs</code> 中 <code>P:System.Collections.Specialized.NotifyCollectionChangedEventArgs.OldStartingIndex</code> = -1（表示未知位置）。</li></ul>异常的调用堆栈开始于 System.Windows.Threading.Dispatcher.VerifyAccess()、System.Windows.DependencyObject.GetValue(DependencyProperty dp)、System.Windows.Controls.Primitives.Selector.GetIsSelected(DependencyObject element)。如果应用程序的 Dispatcher 线程不止一个，则 .NET Framework 4.5 中可能发生此异常。 在 .NET Framework 4.7 中，也可能在具有单个 Dispatcher 线程的应用程序中发生该异常。 此问题已在 .NET Framework 4.7.1 中解决。

#### <a name="suggestion"></a>建议

升级到 .NET Framework 4.7.1。

| 名称    | 值       |
|:--------|:------------|
| 范围   |次要|
|Version|4.7|
|类型|运行时|

#### <a name="affected-apis"></a>受影响的 API

无法通过 API 分析检测到。

<!--

#### Affected APIs

Not detectable via API analysis.

-->
