---
ms.openlocfilehash: f907cb1939e111c586d68243e6b8a8e291974e36
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/01/2020
ms.locfileid: "85615614"
---
### <a name="wpf-layout-rounding-of-margins-has-changed"></a>边距的 WPF 布局舍入已更改

#### <a name="details"></a>详细信息

边距舍入的方式以及边框和边框内的背景已更改。 此更改的结果是：

- 元素的宽度或高度最多可以扩大或收缩一个像素。
- 对象的位置最多可以移动一个像素。
- 居中的元素最多可以垂直或水平地偏离中心一个像素。
默认情况下，仅对面向 .NET Framework 4.6 的应用启用此新布局。

#### <a name="suggestion"></a>建议

由于这种修改往往会消除高 DPI 处 WPF 控件的右侧或底部剪辑，因此面向早期版本的 .NET framework 但在.NET Framework 4.6 上运行的应用可以通过将下面的行添加到 app.config 文件的 `<runtime>` 部分来选择加入此新行为：

<pre><code class="lang-xml">&lt;AppContextSwitchOverrides value=&quot;Switch.MS.Internal.DoNotApplyLayoutRoundingToMarginsAndBorderThickness=false&quot; /&gt;&#39;&#13;&#10;</code></pre>

面向 .NET Framework 4.6 但希望 WPF 控件使用之前的布局算法来呈现的应用可以通过将下面的行添加到 app.config 文件的 `<runtime>` 部分来执行此操作：

<pre><code class="lang-xml">&lt;AppContextSwitchOverrides value=&quot;Switch.MS.Internal.DoNotApplyLayoutRoundingToMarginsAndBorderThickness=true&quot; /&gt;&#39;.&#13;&#10;</code></pre>

| “属性”    | “值”       |
|:--------|:------------|
| 范围   | 次要       |
| Version | 4.6         |
| 类型    | 重定目标 |
