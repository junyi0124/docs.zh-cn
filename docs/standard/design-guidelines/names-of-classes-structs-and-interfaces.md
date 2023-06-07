---
title: 类、结构和接口的名称
description: 使用这些准则来命名类、结构和接口，作为设计扩展和交互 .NET 库的库的指南的一部分。
ms.date: 10/22/2008
helpviewer_keywords:
- type names, guidelines
- classes [.NET Framework], names
- enumerations [.NET Framework], names
- names [.NET Framework], interfaces
- common type names
- names [.NET Framework], type names
- names [.NET Framework], classes
- interfaces [.NET Framework], names
- generic type parameters
ms.assetid: 87a4b0da-ed64-43b1-ac43-968576c444ce
ms.openlocfilehash: 49bafda0d5c362fa02313c5304436069d054cfd9
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95706510"
---
# <a name="names-of-classes-structs-and-interfaces"></a>类、结构和接口的名称

遵循的命名准则适用于常规类型命名。

 ✔️使用 PascalCasing 将类和结构命名为名词或名词短语。

 这区分了方法中的类型名称，这些类型名为带有谓词短语。

 ✔️利用形容词短语或偶尔用名词或名词短语来命名接口。

 应很少使用名词和名词短语，它们可能会指示该类型应为抽象类，而不是接口。

 ❌ 不要为类名称指定前缀 (例如，"C" ) 。

 ✔️考虑用基类的名称结束派生类的名称。

 这是非常可读的，并且清楚地说明了该关系。 代码中的一些示例是：，它是一种 `ArgumentOutOfRangeException` `Exception` ，而 `SerializableAttribute` 是一种类型的 `Attribute` 。 但是，在应用此准则时使用合理的判断非常重要;例如， `Button` 类是一种 `Control` 事件，但 `Control` 它的名称中没有显示。

 ✔️使用字母 I 来为接口名称加上前缀，以指示该类型是接口。

 例如， `IComponent` (描述性名词) ， `ICustomAttributeProvider` (名词短语) ， `IPersistable` (形容词) 是适当的接口名称。 对于其他类型名称，请避免缩写形式。

 ✔️确保在定义类接口对（其中类是接口的标准实现）时，接口名称上的 "I" 前缀仅有不同的名称。

## <a name="names-of-generic-type-parameters"></a>泛型类型参数的名称

 已将泛型添加到 .NET Framework 2.0。 此功能引入了一种称为 *类型参数* 的新标识符。

 ✔️使用描述性名称命名泛型类型参数，除非单个字母名称完全一目了然，并且描述性名称不会添加值。

 ✔️考虑使用 `T` 作为具有一个单字母类型参数的类型的类型参数名称。

```csharp
public int IComparer<T> { ... }
public delegate bool Predicate<T>(T item);
public struct Nullable<T> where T:struct { ... }
```

 ✔️在中为描述性类型参数名称加上前缀 `T` 。

```csharp
public interface ISessionChannel<TSession> where TSession : ISession {
    TSession Session { get; }
}
```

 ✔️考虑，指示对参数名称中的类型参数施加的约束。

 例如，可以调用约束为的参数 `ISession` `TSession` 。

## <a name="names-of-common-types"></a>常见类型的名称

 当命名类型派生自或实现某些 .NET Framework 类型时，✔️按照下表中所述的指导原则进行操作。

|基类型|派生/实现类型准则|
|---------------|------------------------------------------|
|`System.Attribute`|✔️将后缀 "Attribute" 添加到自定义特性类的名称中。|
|`System.Delegate`|✔️将后缀 "EventHandler" 添加到在事件中使用的委托的名称。<br /><br /> ✔️将后缀 "Callback" 添加到作为事件处理程序使用的委托的名称。<br /><br /> ❌ 不要将后缀 "Delegate" 添加到委托。|
|`System.EventArgs`|✔️添加后缀 "EventArgs"。|
|`System.Enum`|❌ 不要从此类派生;改为使用您的语言支持的关键字;例如，在 c # 中，使用 `enum` 关键字。<br /><br /> ❌ 不要添加后缀 "Enum" 或 "标志"。|
|`System.Exception`|✔️添加后缀 "Exception"。|
|`IDictionary` <br /> `IDictionary<TKey,TValue>`|✔️添加后缀 "Dictionary"。 请注意， `IDictionary` 是一种特定类型的集合，但是此准则优先于下面的更常见的集合原则。|
|`IEnumerable` <br /> `ICollection` <br /> `IList` <br /> `IEnumerable<T>` <br /> `ICollection<T>` <br /> `IList<T>`|✔️添加后缀 "Collection"。|
|`System.IO.Stream`|✔️添加后缀 "Stream"。|
|`CodeAccessPermission IPermission`|✔️添加后缀 "权限"。|

## <a name="naming-enumerations"></a>命名枚举

 通常情况下 ) ，枚举类型 (名称也称为枚举) 应遵循标准类型命名规则 (PascalCasing 等。 不过，还有其他一些准则专门适用于枚举。

 ✔️为枚举使用单数类型名称，除非其值为位域。

 ✔️对将位域作为值的枚举使用复数类型名称，也称为标志枚举。

 ❌ 不要在枚举类型名称中使用 "Enum" 后缀。

 ❌ 不要在枚举类型名称中使用 "标记" 或 "标志" 后缀。

 ❌ 不要在枚举值名称上使用前缀 (例如，将 "ad" 用于 ADO 枚举，使用 "rtf" 进行丰富文本枚举等 ) 。

 *部分©2005，2009 Microsoft Corporation。保留所有权利。*

 *经许可重印皮尔逊教育，Inc. 的作者 [：从框架设计指导原则：用于可重复使用的 .Net 库的约定、惯例和模式; 第2版](https://www.informit.com/store/framework-design-guidelines-conventions-idioms-and-9780321545619) By Krzysztof Cwalina，Brad Abrams，通过 Addison-Wesley Professional 作为 Microsoft Windows 开发系列的一部分2008发布。*

## <a name="see-also"></a>另请参阅

- [框架设计准则](index.md)
- [命名准则](naming-guidelines.md)
