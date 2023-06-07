---
title: 属性设计
ms.date: 10/22/2008
helpviewer_keywords:
- member design guidelines, properties
- properties [.NET Framework], design guidelines
ms.assetid: 127cbc0c-cbed-48fd-9c89-7c5d4f98f163
ms.openlocfilehash: ed287b98c012622caa5f8f1cc90fced90dda3e62
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95730963"
---
# <a name="property-design"></a>属性设计

尽管从技术上讲，属性非常类似于方法，但它们的使用方案非常不同。 它们应显示为智能字段。 它们具有字段的调用语法以及方法的灵活性。

 如果调用方不能更改属性的值，✔️确实要创建只获得属性。

 请记住，如果属性的类型是可变的引用类型，则即使属性是只读的，也可以更改属性值。

 ❌ 不要提供仅限集的属性或属性，其中 setter 具有比 getter 更广泛的可访问性。

 例如，不要将属性与公共资源库和受保护的 getter 一起使用。

 如果无法提供属性 getter，请改为将该功能作为方法实现。 考虑使用启动方法名称 `Set` ，并遵循您命名的属性。 例如， <xref:System.AppDomain> 有一个名为的方法， `SetCachePath` 而不是只调用一个设置的属性 `CachePath` 。

 ✔️为所有属性提供合理的默认值，确保默认值不会导致安全漏洞或极低效的代码。

 ✔️允许以任意顺序设置属性，即使这样会导致对象的临时无效状态。

 将两个或更多属性关联到一个点，在该点上，对于同一对象的其他属性的值，可能会有一个属性的值无效。 在这种情况下，不应延迟由无效状态引发的异常，直到对象实际使用相关属性。

 如果属性 setter 引发异常，✔️保留以前的值。

 ❌ 避免从属性 getter 引发异常。

 属性 getter 应为简单操作，不应具有任何前置条件。 如果 getter 可能引发异常，则可能会将其重新设计为方法。 请注意，此规则不适用于索引器，在这种情况下，我们预期会在验证参数时引发异常。

### <a name="indexed-property-design"></a>索引属性设计

 索引属性是一个特殊属性，它可以具有参数，并可使用类似于数组索引的特殊语法进行调用。

 索引属性通常称为索引器。 索引器仅应在提供对逻辑集合中的项的访问的 Api 中使用。 例如，字符串是字符的集合，并添加了的索引器 <xref:System.String?displayProperty=nameWithType> 以访问其字符。

 ✔️考虑使用索引器来提供对存储在内部数组中的数据的访问。

 ✔️考虑为表示项集合的类型提供索引器。

 ❌ 避免使用具有多个参数的索引属性。

 如果设计需要多个参数，请重新考虑属性是否确实表示逻辑集合的访问器。 如果不是，请改用方法。 请考虑以或开头的方法名称 `Get` `Set` 。

 ❌ 避免使用除 <xref:System.Int32?displayProperty=nameWithType> 、 <xref:System.Int64?displayProperty=nameWithType> 、 <xref:System.String?displayProperty=nameWithType> 、 <xref:System.Object?displayProperty=nameWithType> 或枚举以外的参数类型的索引器。

 如果设计需要其他类型的参数，请强重新评估 API 是否确实表示逻辑集合的访问器。 否则，请使用方法。 请考虑以或开头的方法名称 `Get` `Set` 。

 ✔️使用 `Item` 索引属性的名称，除非有明显更好的名称 (例如，请参阅 <xref:System.String.Chars%2A>) 上的属性 `System.String` 。

 在 c # 中，索引器默认命名为 Item。 <xref:System.Runtime.CompilerServices.IndexerNameAttribute>可用于自定义此名称。

 ❌ 不要提供在语义上等效的索引器和方法。

 ❌ 不要提供一个类型的多个重载索引器系列。

 这是由 c # 编译器强制执行的。

 ❌ 不要使用非默认的索引属性。

 这是由 c # 编译器强制执行的。

### <a name="property-change-notification-events"></a>属性更改通知事件

 有时，提供事件来向用户通知属性值的更改是非常有用的。 例如， `System.Windows.Forms.Control` `TextChanged` 在其属性的值发生更改后引发事件 `Text` 。

 ✔️考虑在高级别 Api 中的属性值 (通常修改设计器组件) 时引发更改通知事件。

 如果用户需要知道何时更改对象的属性，则该对象应该为属性引发更改通知事件。。

 但是，对于低级别 Api （如基类型或集合），引发此类事件的开销不太大。 例如， <xref:System.Collections.Generic.List%601> 当向列表中添加新项并且 `Count` 属性发生更改时，将不会引发此类事件。

 ✔️考虑当属性的值通过外部强制更改时引发更改通知事件。

 如果某个属性值通过某个外部强制 (的方式发生更改，而不是通过对对象) 调用方法进行更改，则引发事件向开发人员指示值正在更改并且已更改。 一个很好的示例是 `Text` 文本框控件的属性。 当用户在中键入文本时 `TextBox` ，属性值将自动更改。

 *部分©2005，2009 Microsoft Corporation。保留所有权利。*

 *经许可重印皮尔逊教育，Inc. 的作者 [：从框架设计指导原则：用于可重复使用的 .Net 库的约定、惯例和模式; 第2版](https://www.informit.com/store/framework-design-guidelines-conventions-idioms-and-9780321545619) By Krzysztof Cwalina，Brad Abrams，通过 Addison-Wesley Professional 作为 Microsoft Windows 开发系列的一部分2008发布。*

## <a name="see-also"></a>另请参阅

- [成员设计准则](member.md)
- [框架设计准则](index.md)
