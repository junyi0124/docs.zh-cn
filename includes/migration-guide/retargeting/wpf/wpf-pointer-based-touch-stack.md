---
ms.openlocfilehash: eb89cbc72d8b09fd1aa5bc775a6c539eb208fa70
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/01/2020
ms.locfileid: "85614403"
---
### <a name="wpf-pointer-based-touch-stack"></a>基于 WPF 指针的触控堆栈

#### <a name="details"></a>详细信息

此次更改后，可启用基于可选 WM_POINTER 的 WPF 触控/触笔堆栈。  没有显式启动此功能的开发人员应该看不到 WPF 触控/触笔行为的更改。基于 WPF 触控/触笔堆栈的可选 WM_POINTER 当前存在的已知问题：

- 不支持实时墨迹书写。
- 尽管墨迹书写和触笔插件仍可运行，但它们是在 UI 线程上进行处理，这可能会导致性能变得糟糕。
- 从触控/触笔事件提升到鼠标事件方面的更改导致行为发生变化
- 控制行为可能不同
- 拖/放行为无法正确显示触控输入反馈
- 这不会影响触笔输入
- 无法再通过触控/触笔事件启动拖/放行为
- 这可能会在检测到鼠标输入前导致应用程序挂起。
- 相反，开发者应通过鼠标事件启动拖放行为。

#### <a name="suggestion"></a>建议

要启用此堆栈的开发者可以在应用程序的 App.config 文件中添加/合并下面的代码：

<pre><code class="lang-xml">&lt;configuration&gt;&#13;&#10;&lt;runtime&gt;&#13;&#10;&lt;AppContextSwitchOverrides value=&quot;Switch.System.Windows.Input.Stylus.EnablePointerSupport=true&quot;/&gt;&#13;&#10;&lt;/runtime&gt;&#13;&#10;&lt;/configuration&gt;&#13;&#10;</code></pre>

删除它或将该值设为 false 将关闭此可选堆栈。请注意，此堆栈仅在 Windows 10 创意者更新以及更高版本上可用。

| “属性”    | “值”       |
|:--------|:------------|
| 范围   | 边缘        |
| Version | 4.7         |
| 类型    | 重定目标 |
