---
title: 类型成员的名称
description: 了解在 .NET 中命名类型成员的准则，如方法、属性、事件和字段。
ms.date: 10/22/2008
helpviewer_keywords:
- events [.NET Framework], names
- methods [.NET Framework], names
- type members
- properties [.NET Framework], names
- fields, names
- field names
- names [.NET Framework], type members
- members [.NET Framework], type
ms.assetid: af5a0903-36af-4c2a-b848-cf959affeaa5
ms.openlocfilehash: 409e881198a359fa28356e22ea73d5b724742a0d
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95706471"
---
# <a name="names-of-type-members"></a>类型成员的名称

类型由以下成员构成：方法、属性、事件、构造函数和字段。 以下各节介绍命名类型成员的准则。

## <a name="names-of-methods"></a>方法的名称

 方法是执行操作的方式，设计准则要求方法名称为谓词或谓词短语。 遵循此准则，还有利于区分方法名称与属性和类型名称，后者为名词或形容词性短语。

 ✔️为谓词或谓词短语指定方法名称。

```csharp
public class String {
    public int CompareTo(...);
    public string[] Split(...);
    public string Trim();
}
```

## <a name="names-of-properties"></a>属性的名称

 与其他成员不同，应向属性给定名词性短语或形容词性名称。 这是因为属性是指数据，属性的名称应反映这一点。 属性名称总是采用帕斯卡大小写。

 ✔️使用名词、名词短语或形容词名称属性。

 ❌ 没有与 "Get" 方法的名称相匹配的属性，如以下示例中所示：

 `public string TextWriter { get {...} set {...} }` `public string GetTextWriter(int value) { ... }`

 此模式通常意味着该属性事实上是一种方法。

 ✔️使用一个复数短语来描述集合中的项，而不是使用后跟 "List" 或 "Collection" 的短语来命名集合属性。

 ✔️使用赞成短语 (， `CanSeek` 而不是) 来命名布尔属性 `CantSeek` 。 或者，还可以在布尔属性前面添加 "Is"、"Can" 或 "has" 前缀，但前提是在它添加值的位置。

 ✔️考虑为属性提供与其类型相同的名称。

 例如，以下属性可正确获取和设置名为 `Color` 的枚举值，因此属性名为 `Color`：

```csharp
public enum Color {...}
public class Control {
    public Color Color { get {...} set {...} }
}
```

## <a name="names-of-events"></a>事件的名称

 事件始终指操作，可以是即将发生的，也可以是已经发生的。 因此，对于方法，事件用谓词命名，并用谓词时态指示引发事件的时间。

 ✔️使用动词或动词短语来命名事件。

 示例包括`Clicked`、`Painting` 和 `DroppedDown`。

 ✔️使用现有的和过去的时态为事件名称提供前后的概念。

 例如，在窗口关闭前引发的关闭事件可命名为 `Closing`，而在窗口关闭后后引发的关闭事件可命名为 `Closed`。

 ❌ 不要使用 "Before" 或 "After" 前缀或 postfixes 来指示前和后事件。 请如上所示使用现在时和过去时。

 ✔️命名事件处理程序 (使用 "EventHandler" 后缀的事件类型) 委托，如下面的示例中所示：

 `public delegate void ClickedEventHandler(object sender, ClickedEventArgs e);`

 ✔️确实要 `sender` `e` 在事件处理程序中使用两个名为和的参数。

 sender 参数表示引发事件的对象。 sender 参数的类型通常是 `object`，即使可以使用更具体的类型。

 ✔️用 "EventArgs" 后缀命名事件参数类。

## <a name="names-of-fields"></a>字段的名称

 字段命名准则适用于静态公开字段和受保护的字段。 原则不涉及内部和专用字段，而[成员设计准则](member.md)不允许使用公开字段或受保护的实例字段。

 ✔️在字段名称中使用 PascalCasing。

 ✔️使用名词、名词短语或形容词来命名字段。

 ❌ 不要对字段名称使用前缀。

 例如，请勿使用“g_”或“s_”来指示静态字段。

 *部分©2005，2009 Microsoft Corporation。保留所有权利。*

 *经许可重印皮尔逊教育，Inc. 的作者 [：从框架设计指导原则：用于可重复使用的 .Net 库的约定、惯例和模式; 第2版](https://www.informit.com/store/framework-design-guidelines-conventions-idioms-and-9780321545619) By Krzysztof Cwalina，Brad Abrams，通过 Addison-Wesley Professional 作为 Microsoft Windows 开发系列的一部分2008发布。*

## <a name="see-also"></a>另请参阅

- [框架设计准则](index.md)
- [命名准则](naming-guidelines.md)
