---
title: 泛型接口中的变体 (C#)
description: 查看泛型接口的变体支持信息，包括 .NET Framework 4 和 4.5 中现有接口的更新信息。
ms.date: 06/06/2019
ms.assetid: 4828a8f9-48c0-4128-9749-7fcd6bf19a06
ms.openlocfilehash: 91218742c01eeb6e65ea26d9dc41ed7c98827377
ms.sourcegitcommit: 04022ca5d00b2074e1b1ffdbd76bec4950697c4c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/23/2020
ms.locfileid: "87105634"
---
# <a name="variance-in-generic-interfaces-c"></a>泛型接口中的变体 (C#)

.NET Framework 4 引入了对多个现有泛型接口的变体支持。 变体支持允许实现这些接口的类进行隐式转换。

自 .NET Framework 4 起，以下接口为变体：

- <xref:System.Collections.Generic.IEnumerable%601>（T 是协变）

- <xref:System.Collections.Generic.IEnumerator%601>（T 是协变）

- <xref:System.Linq.IQueryable%601>（T 是协变）

- <xref:System.Linq.IGrouping%602>（`TKey` 和 `TElement` 都是协变）

- <xref:System.Collections.Generic.IComparer%601>（T 是逆变）

- <xref:System.Collections.Generic.IEqualityComparer%601>（T 是逆变）

- <xref:System.IComparable%601>（T 是逆变）

自 .NET Framework 4.5 起，以下接口是变体：

- <xref:System.Collections.Generic.IReadOnlyList%601>（T 是协变）

- <xref:System.Collections.Generic.IReadOnlyCollection%601>（T 是协变）

协变允许方法具有的返回类型比接口的泛型类型参数定义的返回类型的派生程度更大。 若要演示协变功能，请考虑以下泛型接口：`IEnumerable<Object>` 和 `IEnumerable<String>`。 `IEnumerable<String>` 接口不继承 `IEnumerable<Object>` 接口。 但是，`String` 类型会继承 `Object` 类型，在某些情况下，建议为这些接口互相指派彼此的对象。 下面的代码示例对此进行了演示。

```csharp
IEnumerable<String> strings = new List<String>();
IEnumerable<Object> objects = strings;
```

在旧版 .NET Framework 中，此代码会导致 C# 中出现编译错误；如果启用 `Option Strict` 条件，则会导致在 Visual Basic 中出现编译错误。 但现在可使用 `strings` 代替 `objects`，如上例所示，因为 <xref:System.Collections.Generic.IEnumerable%601> 接口是协变接口。

逆变允许方法具有的实参类型比接口的泛型形参定义的类型的派生程度更小。 若要演示逆变，假设已创建了 `BaseComparer` 类来比较 `BaseClass` 类的实例。 `BaseComparer` 类实现 `IEqualityComparer<BaseClass>` 接口。 因为 <xref:System.Collections.Generic.IEqualityComparer%601> 接口现在是逆变接口，因此可使用 `BaseComparer` 比较继承 `BaseClass` 类的类的实例。 下面的代码示例对此进行了演示。

```csharp
// Simple hierarchy of classes.
class BaseClass { }
class DerivedClass : BaseClass { }

// Comparer class.
class BaseComparer : IEqualityComparer<BaseClass>
{
    public int GetHashCode(BaseClass baseInstance)
    {
        return baseInstance.GetHashCode();
    }
    public bool Equals(BaseClass x, BaseClass y)
    {
        return x == y;
    }
}
class Program
{
    static void Test()
    {
        IEqualityComparer<BaseClass> baseComparer = new BaseComparer();

        // Implicit conversion of IEqualityComparer<BaseClass> to
        // IEqualityComparer<DerivedClass>.
        IEqualityComparer<DerivedClass> childComparer = baseComparer;
    }
}
```

有关更多示例，请参阅[在泛型集合的接口中使用变体 (C#)](./using-variance-in-interfaces-for-generic-collections.md)。

只有引用类型才支持使用泛型接口中的变体。 值类型不支持变体。 例如，无法将 `IEnumerable<int>` 隐式转换为 `IEnumerable<object>`，因为整数由值类型表示。

```csharp
IEnumerable<int> integers = new List<int>();
// The following statement generates a compiler error,
// because int is a value type.
// IEnumerable<Object> objects = integers;
```

还需记住，实现变体接口的类仍是固定类。 例如，虽然 <xref:System.Collections.Generic.List%601> 实现协变接口 <xref:System.Collections.Generic.IEnumerable%601>，但不能将 `List<String>` 隐式转换为 `List<Object>`。 以下代码示例阐释了这一点。

```csharp
// The following line generates a compiler error
// because classes are invariant.
// List<Object> list = new List<String>();

// You can use the interface object instead.
IEnumerable<Object> listObjects = new List<String>();
```

## <a name="see-also"></a>请参阅

- [在泛型集合的接口中使用变体 (C#)](./using-variance-in-interfaces-for-generic-collections.md)
- [创建变体泛型接口 (C#)](./creating-variant-generic-interfaces.md)
- [泛型接口](../../../../standard/generics/interfaces.md)
- [委托中的变体 (C#)](./variance-in-delegates.md)
