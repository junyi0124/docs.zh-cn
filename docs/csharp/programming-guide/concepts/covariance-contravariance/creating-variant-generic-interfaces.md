---
title: 创建变体泛型接口 (C#)
description: 了解如何使用协变或逆变泛型类型参数创建变体泛型接口。
ms.date: 07/20/2015
ms.assetid: 30330ec4-9df2-4838-a535-6c406d0ed4df
ms.openlocfilehash: 38b32784b681e748cd508c3d431fd4b18ec2c81a
ms.sourcegitcommit: 04022ca5d00b2074e1b1ffdbd76bec4950697c4c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/23/2020
ms.locfileid: "87105721"
---
# <a name="creating-variant-generic-interfaces-c"></a>创建变体泛型接口 (C#)

接口中的泛型类型参数可以声明为协变或逆变。 协变允许接口方法具有与泛型类型参数定义的返回类型相比，派生程度更大的返回类型。 逆变允许接口方法具有与泛型形参指定的实参类型相比，派生程度更小的实参类型。 具有协变或逆变泛型类型参数的泛型接口称为“变体”。

> [!NOTE]
> .NET Framework 4 引入了对多个现有泛型接口的变体支持。 有关 .NET 中变体接口的列表，请参阅[泛型接口中的变体 (C#)](./variance-in-generic-interfaces.md)。

## <a name="declaring-variant-generic-interfaces"></a>声明变体泛型接口

可通过对泛型类型参数使用 `in` 和 `out` 关键字来声明变体泛型接口。

> [!IMPORTANT]
> C# 中的 `ref`、`in` 和 `out` 参数不能为变体。 值类型也不支持变体。

可以使用 `out` 关键字将泛型类型参数声明为协变。 协变类型必须满足以下条件：

- 类型仅用作接口方法的返回类型，不用作方法参数的类型。 下例演示了此要求，其中类型 `R` 为声明的协变。

    ```csharp
    interface ICovariant<out R>
    {
        R GetSomething();
        // The following statement generates a compiler error.
        // void SetSomething(R sampleArg);

    }
    ```

    此规则有一个例外。 如果具有用作方法参数的逆变泛型委托，则可将类型用作该委托的泛型类型参数。 下例中的类型 `R` 演示了此情形。 有关详细信息，请参阅[委托中的变体 (C#)](./variance-in-delegates.md) 和[对 Func 和 Action 泛型委托使用变体 (C#)](./using-variance-for-func-and-action-generic-delegates.md)。

    ```csharp
    interface ICovariant<out R>
    {
        void DoSomething(Action<R> callback);
    }
    ```

- 类型不用作接口方法的泛型约束。 下面的代码阐释了这一点。

    ```csharp
    interface ICovariant<out R>
    {
        // The following statement generates a compiler error
        // because you can use only contravariant or invariant types
        // in generic constraints.
        // void DoSomething<T>() where T : R;
    }
    ```

可以使用 `in` 关键字将泛型类型参数声明为逆变。 逆变类型只能用作方法参数的类型，不能用作接口方法的返回类型。 逆变类型还可用于泛型约束。 以下代码演示如何声明逆变接口，以及如何将泛型约束用于其方法之一。

```csharp
interface IContravariant<in A>
{
    void SetSomething(A sampleArg);
    void DoSomething<T>() where T : A;
    // The following statement generates a compiler error.
    // A GetSomething();
}
```

此外，还可以在同一接口中同时支持协变和逆变，但需应用于不同的类型参数，如以下代码示例所示。

```csharp
interface IVariant<out R, in A>
{
    R GetSomething();
    void SetSomething(A sampleArg);
    R GetSetSomethings(A sampleArg);
}
```

## <a name="implementing-variant-generic-interfaces"></a>实现变体泛型接口

在类中实现变体泛型接口时，所用语法和用于固定接口的语法相同。 以下代码示例演示如何在泛型类中实现协变接口。

```csharp
interface ICovariant<out R>
{
    R GetSomething();
}
class SampleImplementation<R> : ICovariant<R>
{
    public R GetSomething()
    {
        // Some code.
        return default(R);
    }
}
```

实现变体接口的类是固定类。 例如，考虑下面的代码。

```csharp
// The interface is covariant.
ICovariant<Button> ibutton = new SampleImplementation<Button>();
ICovariant<Object> iobj = ibutton;

// The class is invariant.
SampleImplementation<Button> button = new SampleImplementation<Button>();
// The following statement generates a compiler error
// because classes are invariant.
// SampleImplementation<Object> obj = button;
```

## <a name="extending-variant-generic-interfaces"></a>扩展变体泛型接口

扩展变体泛型接口时，必须使用 `in` 和 `out` 关键字来显式指定派生接口是否支持变体。 编译器不会根据正在扩展的接口来推断变体。 例如，考虑以下接口。

```csharp
interface ICovariant<out T> { }
interface IInvariant<T> : ICovariant<T> { }
interface IExtCovariant<out T> : ICovariant<T> { }
```

尽管 `IInvariant<T>` 接口和 `IExtCovariant<out T>` 接口扩展的是同一个接口，但泛型类型参数 `T` 在前者中为固定参数，在后者中为协变参数。 此规则也适用于逆变泛型类型参数。

无论泛型类型参数 `T` 在接口中是协变还是逆变，都可以创建一个接口来扩展这两类接口，只要在扩展接口中，该 `T` 泛型类型参数为固定参数。 以下代码示例阐释了这一点。

```csharp
interface ICovariant<out T> { }
interface IContravariant<in T> { }
interface IInvariant<T> : ICovariant<T>, IContravariant<T> { }
```

但是，如果泛型类型参数 `T` 在一个接口中声明为协变，则无法在扩展接口中将其声明为逆变，反之亦然。 以下代码示例阐释了这一点。

```csharp
interface ICovariant<out T> { }
// The following statement generates a compiler error.
// interface ICoContraVariant<in T> : ICovariant<T> { }
```

### <a name="avoiding-ambiguity"></a>避免多义性

实现变体泛型接口时，变体有时可能会导致多义性。 应避免这样的多义性。

例如，如果在一个类中使用不同的泛型类型参数来显式实现同一变体泛型接口，便会产生多义性。 在这种情况下，编译器不会产生错误，但未指定将在运行时选择哪个接口实现。 这种多义性可能导致代码中出现小 bug。 请考虑以下代码示例。

```csharp
// Simple class hierarchy.
class Animal { }
class Cat : Animal { }
class Dog : Animal { }

// This class introduces ambiguity
// because IEnumerable<out T> is covariant.
class Pets : IEnumerable<Cat>, IEnumerable<Dog>
{
    IEnumerator<Cat> IEnumerable<Cat>.GetEnumerator()
    {
        Console.WriteLine("Cat");
        // Some code.
        return null;
    }

    IEnumerator IEnumerable.GetEnumerator()
    {
        // Some code.
        return null;
    }

    IEnumerator<Dog> IEnumerable<Dog>.GetEnumerator()
    {
        Console.WriteLine("Dog");
        // Some code.
        return null;
    }
}
class Program
{
    public static void Test()
    {
        IEnumerable<Animal> pets = new Pets();
        pets.GetEnumerator();
    }
}
```

在此示例中，没有指定 `pets.GetEnumerator` 方法如何在 `Cat` 和 `Dog` 之间选择。 这可能导致代码中出现问题。

## <a name="see-also"></a>请参阅

- [泛型接口中的变体 (C#)](./variance-in-generic-interfaces.md)
- [对 Func 和 Action 泛型委托使用变体 (C#)](./using-variance-for-func-and-action-generic-delegates.md)
