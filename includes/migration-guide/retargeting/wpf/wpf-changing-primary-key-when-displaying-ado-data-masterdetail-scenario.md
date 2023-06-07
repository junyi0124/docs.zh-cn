---
ms.openlocfilehash: 35fc4782ebbba33f5fc6668712af9d89d00cafe9
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/01/2020
ms.locfileid: "85614417"
---
### <a name="wpf-changing-a-primary-key-when-displaying-ado-data-in-a-masterdetail-scenario"></a>WPF 在主/从方案中显示 ADO 数据时更改主键

#### <a name="details"></a>详细信息

假设你有 `Order` 类型的 ADO 项集合，其关系名为 &quot;OrderDetails&quot;，通过主键 &quot;OrderID&quot; 将其关联到 `Detail` 类型的项集合。 在 WPF 应用程序中，你可以将列表控件绑定到给定顺序的详细信息：

```xaml
<ListBox ItemsSource="{Binding Path=OrderDetails}" >
```

其中 DataContext 是一个 `Order`。 WPF 会获取 `OrderDetails` 属性的值 - 其 `OrderID` 与主项的 `OrderID` 匹配的所有 `Detail` 项的集合 D。 更改主项的主密钥 `OrderID` 时，会出现行为更改。 ADO 自动更改详细信息集合中每个受影响记录的 `OrderID`（即复制到集合 D 的信息）。  但 D 会发生什么？

- 旧行为：清除集合 D。 主项*不会*发出属性 `OrderDetails` 的更改通知。 列表框将继续使用集合 D，现为空。
- 新行为：集合 D 保持不变。 其中每一项都将发出 `OrderID` 属性的更改通知。 列表框将持续使用集合 D，并显示新 `OrderID` 的详细信息。 WPF 通过不同的方式创建集合 D 来实现新行为：通过调用 ADO 方法 <xref:System.Data.DataRowView.CreateChildView(System.Data.DataRelation,System.Boolean)?displayProperty=nameWithType>，并将 `followParent` 参数设置为 `true`。

#### <a name="suggestion"></a>建议

应用通过使用以下 AppContext 开关获取新行为。

```xml
<configuration>
  <runtime>
    <AppContextSwitchOverrides value="Switch.System.Windows.Data.DoNotUseFollowParentWhenBindingToADODataRelation=false"/>
  </runtime>
</configuration>
```

对于面向 .NET 4.7.1 或更低版本的应用，开关默认为 `true`（旧行为），而对于面向 .NET 4.7.2 或更高版本的应用，开关默认为 `false`（新行为）。

| “属性”    | “值”       |
|:--------|:------------|
| 范围   | 次要       |
| 版本 | 4.7.2       |
| 类型    | 重定目标 |
