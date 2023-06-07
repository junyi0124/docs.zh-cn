---
title: 依赖项属性
ms.date: 10/22/2008
ms.assetid: 212cfb1e-cec4-4047-94a6-47209b387f6f
ms.openlocfilehash: ab30da59670c146874defe86b1d048f97eebf449
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95734759"
---
# <a name="dependency-properties"></a>依赖项属性

 (DP) 的依赖属性是一个常规属性，它将其值存储在属性存储中，而不是将其存储在类型变量中，而不是将其存储在类型变量 (字段) 中。

 附加的依赖属性是作为静态 Get 和 Set 方法建模的一种依赖属性，用于描述对象及其容器之间的关系 (例如， `Button` 对象在容器) 上的位置。 `Panel`

 如果需要属性来支持 WPF 功能，如样式、触发器、数据绑定、动画、动态资源和继承，✔️提供依赖属性。

## <a name="dependency-property-design"></a>依赖项属性设计

 <xref:System.Windows.DependencyObject>当实现依赖项属性时，✔️确实从或它的一个子类型继承。 类型提供了一种非常有效的属性存储实现，并自动支持 WPF 数据绑定。

 ✔️ <xref:System.Windows.DependencyProperty?displayProperty=nameWithType> 为每个依赖属性提供一个常规 CLR 属性和一个存储实例的公共静态只读字段。

 ✔️通过调用实例方法和实现依赖项 <xref:System.Windows.DependencyObject.GetValue%2A?displayProperty=nameWithType> 属性 <xref:System.Windows.DependencyObject.SetValue%2A?displayProperty=nameWithType> 。

 ✔️通过将属性的名称 suffixing 为 "属性"，来命名依赖属性静态字段。

 ❌ 不要在代码中显式设置依赖项属性的默认值;请改为在元数据中设置它们。

 如果显式设置了属性默认值，则可能会阻止某些隐式方式（如样式）设置该属性。

 ❌ 不要将代码放在属性访问器中，而不是标准代码访问静态字段。

 如果通过隐式方式（如样式）设置属性，则不会执行该代码，因为样式设置直接使用静态字段。

 ❌ 不要使用依赖项属性来存储安全数据。 甚至可以公开访问专用依赖属性。

## <a name="attached-dependency-property-design"></a>附加的依赖属性设计

 上一部分中所述的依赖项属性表示声明类型的内部属性;例如， `Text` 属性是声明它的的属性 `TextButton` 。 特殊类型的依赖项属性是附加的依赖属性。

 附加属性的典型示例是 <xref:System.Windows.Controls.Grid.Column%2A?displayProperty=nameWithType> 属性。 属性表示按钮 (不是网格的) 列位置，但它仅在按钮包含在网格中时才相关，因此它通过网格 "附加" 到按钮。

```xaml
<Grid>
    <Grid.ColumnDefinitions>
        <ColumnDefinition />
        <ColumnDefinition />
    </Grid.ColumnDefinitions>

    <Button Grid.Column="0">Click</Button>
    <Button Grid.Column="1">Clack</Button>
</Grid>
```

 附加属性的定义与常规依赖属性的定义大致相同，不同之处在于访问器由静态 Get 和 Set 方法表示：

```csharp
public class Grid {

    public static int GetColumn(DependencyObject obj) {
        return (int)obj.GetValue(ColumnProperty);
    }

    public static void SetColumn(DependencyObject obj, int value) {
        obj.SetValue(ColumnProperty,value);
    }

    public static readonly DependencyProperty ColumnProperty =
        DependencyProperty.RegisterAttached(
            "Column",
            typeof(int),
            typeof(Grid)
    );
}
```

## <a name="dependency-property-validation"></a>依赖属性验证

 属性通常实现称为 "验证"。 尝试更改属性的值时，将执行验证逻辑。

 可惜依赖属性访问器不能包含任意验证代码。 相反，需要在属性注册期间指定依赖属性验证逻辑。

 ❌ 不要在属性的访问器中放置依赖属性验证逻辑。 而是将验证回调传递给 `DependencyProperty.Register` 方法。

## <a name="dependency-property-change-notifications"></a>依赖属性更改通知

 ❌ 不要在依赖属性访问器中实现更改通知逻辑。 依赖属性具有内置的更改通知功能，该功能必须通过向提供更改通知回调来使用 <xref:System.Windows.PropertyMetadata> 。

## <a name="dependency-property-value-coercion"></a>依赖项属性值强制

 在实际修改属性存储区之前，由 setter 修改了向属性 setter 提供的值时，会发生属性强制。

 ❌ 不要在依赖属性访问器中实现强制逻辑。

 依赖属性具有内置强制功能，可通过向提供强制回调来使用 `PropertyMetadata` 。

 *部分©2005，2009 Microsoft Corporation。保留所有权利。*

 *经许可重印皮尔逊教育，Inc. 的作者 [：从框架设计指导原则：用于可重复使用的 .Net 库的约定、惯例和模式; 第2版](https://www.informit.com/store/framework-design-guidelines-conventions-idioms-and-9780321545619) By Krzysztof Cwalina，Brad Abrams，通过 Addison-Wesley Professional 作为 Microsoft Windows 开发系列的一部分2008发布。*

## <a name="see-also"></a>另请参阅

- [框架设计准则](index.md)
- [常见设计模式](common-design-patterns.md)
