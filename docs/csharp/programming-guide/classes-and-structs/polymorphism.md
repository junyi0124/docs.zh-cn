---
title: 多形性 - C# 编程指南
description: 了解多形性，这是一个面向对象的编程语言（如 C#）中的关键概念，描述基类与派生类之间的关系。
ms.date: 02/08/2020
helpviewer_keywords:
- C# language, polymorphism
- polymorphism [C#]
ms.assetid: 086af969-29a5-4ce8-a993-0b7d53839dab
ms.openlocfilehash: 59b5f5d2d5a8f274845607aeca370c316670bd68
ms.sourcegitcommit: 40de8df14289e1e05b40d6e5c1daabd3c286d70c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/22/2020
ms.locfileid: "86925444"
---
# <a name="polymorphism-c-programming-guide"></a>多态性（C# 编程指南）

多态性常被视为自封装和继承之后，面向对象的编程的第三个支柱。 Polymorphism（多态性）是一个希腊词，指“多种形态”，多态性具有两个截然不同的方面：
  
- 在运行时，在方法参数和集合或数组等位置，派生类的对象可以作为基类的对象处理。 在出现此多形性时，该对象的声明类型不再与运行时类型相同。
- 基类可以定义并实现[虚](../../language-reference/keywords/virtual.md)方法，派生类可以[重写](../../language-reference/keywords/override.md)这些方法，即派生类提供自己的定义和实现。 在运行时，客户端代码调用该方法，CLR 查找对象的运行时类型，并调用虚方法的重写方法。 你可以在源代码中调用基类的方法，执行该方法的派生类版本。

虚方法允许你以统一方式处理多组相关的对象。 例如，假定你有一个绘图应用程序，允许用户在绘图图面上创建各种形状。 你在编译时不知道用户将创建哪些特定类型的形状。 但应用程序必须跟踪创建的所有类型的形状，并且必须更新这些形状以响应用户鼠标操作。 你可以使用多态性通过两个基本步骤解决这一问题：

1. 创建一个类层次结构，其中每个特定形状类均派生自一个公共基类。
1. 使用虚方法通过对基类方法的单个调用来调用任何派生类上的相应方法。

首先，创建一个名为 `Rectangle``Shape` 的基类，并创建一些派生类，例如 `Triangle``Circle`、 和 。 为 `Shape` 类提供一个名为 `Draw` 的虚拟方法，并在每个派生类中重写该方法以绘制该类表示的特定形状。 创建 `List<Shape>` 对象，并向其添加 `Circle`、`Triangle` 和 `Rectangle`。

[!code-csharp[Polymorphism overview](~/samples/snippets/csharp/objectoriented/Inheritance.cs#PolymorphismOverview)]

若要更新绘图图面，请使用 [foreach](../../language-reference/keywords/foreach-in.md) 循环对该列表进行循环访问，并对其中的每个 `Shape` 对象调用 `Draw` 方法。 虽然列表中的每个对象都具有声明类型 `Shape`，但调用的将是运行时类型（该方法在每个派生类中的重写版本）。

[!code-csharp[Polymorphism overview](~/samples/snippets/csharp/objectoriented/Inheritance.cs#UsePolymorphism)]

在 C# 中，每个类型都是多态的，因为包括用户定义类型在内的所有类型都继承自 <xref:System.Object>。  

## <a name="polymorphism-overview"></a>多形性概述

### <a name="virtual-members"></a>虚拟成员

当派生类从基类继承时，它会获得基类的所有方法、字段、属性和事件。 派生类的设计器可以针对虚拟方法的行为做出不同的选择：

- 派生类可以重写基类中的虚拟成员，并定义新行为。
- 派生类继承最接近的基类方法而不重写方法，同时保留现有的行为，但允许进一步派生的类重写方法。
- 派生类可以定义隐藏基类实现的成员的新非虚实现。

仅当基类成员声明为 [virtual](../../language-reference/keywords/virtual.md) 或 [abstract](../../language-reference/keywords/abstract.md) 时，派生类才能重写基类成员。 派生成员必须使用 [override](../../language-reference/keywords/override.md) 关键字显式指示该方法将参与虚调用。 以下代码提供了一个示例：

[!code-csharp[Virtual overview](~/samples/snippets/csharp/objectoriented/Inheritance.cs#VirtualMethods)]

字段不能是虚拟的，只有方法、属性、事件和索引器才可以是虚拟的。 当派生类重写某个虚拟成员时，即使该派生类的实例被当作基类的实例访问，也会调用该成员。 以下代码提供了一个示例：

[!code-csharp[Virtual overview example](~/samples/snippets/csharp/objectoriented/Inheritance.cs#SnippetTestVirtualMethods)]

虚方法和属性允许派生类扩展基类，而无需使用方法的基类实现。 有关详细信息，请参阅[使用 Override 和 New 关键字进行版本控制](./versioning-with-the-override-and-new-keywords.md)。 接口提供另一种方式来定义将实现留给派生类的方法或方法集。 有关详细信息，请参阅[接口](../interfaces/index.md)。

### <a name="hide-base-class-members-with-new-members"></a>使用新成员隐藏基类成员

如果希望派生类具有与基类中的成员同名的成员，则可以使用 [new](../../language-reference/keywords/new-modifier.md) 关键字隐藏基类成员。 `new` 关键字放置在要替换的类成员的返回类型之前。 以下代码提供了一个示例：

[!code-csharp[New method overview example](~/samples/snippets/csharp/objectoriented/Inheritance.cs#NewMethods)]

通过将派生类的实例强制转换为基类的实例，可以从客户端代码访问隐藏的基类成员。 例如：

[!code-csharp[New method overview usage](~/samples/snippets/csharp/objectoriented/Inheritance.cs#UseNewMethods)]

### <a name="prevent-derived-classes-from-overriding-virtual-members"></a>阻止派生类重写虚拟成员  

无论在虚拟成员和最初声明虚拟成员的类之间已声明了多少个类，虚拟成员都是虚拟的。 如果类 `A` 声明了一个虚拟成员，类 `B` 从 `A` 派生，类 `C` 从类 `B` 派生，则不管类 `B` 是否为虚拟成员声明了重写，类 `C` 都会继承该虚拟成员，并可以重写它。 以下代码提供了一个示例：

[!code-csharp[Basic hierarchy](~/samples/snippets/csharp/objectoriented/Hierarchy.cs#FirstHierarchy)]

派生类可以通过将重写声明为 [sealed](../../language-reference/keywords/sealed.md) 来停止虚拟继承。 停止继承需要在类成员声明中的 `override` 关键字前面放置 `sealed` 关键字。 以下代码提供了一个示例：

[!code-csharp[A sealed overridden member](~/samples/snippets/csharp/objectoriented/Hierarchy.cs#SealedOverride)]

在上一个示例中，方法 `DoWork` 对从 `C` 派生的任何类都不再是虚拟方法。 即使它们转换为类型 `B` 或类型 `A`，它对于 `C` 的实例仍然是虚拟的。 通过使用 `new` 关键字，密封的方法可以由派生类替换，如下面的示例所示：

[!code-csharp[New method declaration](~/samples/snippets/csharp/objectoriented/Hierarchy.cs#NewDeclaration)]

在此情况下，如果在 `D` 中使用类型为 `D` 的变量调用 `DoWork`，被调用的将是新的 `DoWork`。 如果使用类型为 `C`、`B` 或 `A` 的变量访问 `D` 的实例，对 `DoWork` 的调用将遵循虚拟继承的规则，即把这些调用传送到类 `C` 的 `DoWork` 实现。

### <a name="access-base-class-virtual-members-from-derived-classes"></a>从派生类访问基类虚拟成员

已替换或重写某个方法或属性的派生类仍然可以使用 `base` 关键字访问基类的该方法或属性。 以下代码提供了一个示例：

```csharp
public class Base
{
    public virtual void DoWork() {/*...*/ }
}
public class Derived : Base
{
    public override void DoWork()
    {
        //Perform Derived's work here
        //...
        // Call DoWork on base class
        base.DoWork();
    }
}
```

有关详细信息，请参阅 [base](../../language-reference/keywords/base.md)。

> [!NOTE]
> 建议虚拟成员在它们自己的实现中使用 `base` 来调用该成员的基类实现。 允许基类行为发生使得派生类能够集中精力实现特定于派生类的行为。 未调用基类实现时，由派生类负责使它们的行为与基类的行为兼容。

## <a name="in-this-section"></a>本节内容

- [使用 Override 和 New 关键字进行版本控制](./versioning-with-the-override-and-new-keywords.md)
- [了解何时使用 Override 和 New 关键字](./knowing-when-to-use-override-and-new-keywords.md)
- [如何替代 ToString 方法](./how-to-override-the-tostring-method.md)

## <a name="see-also"></a>请参阅

- [C# 编程指南](../index.md)
- [继承](./inheritance.md)
- [抽象类、密封类及类成员](./abstract-and-sealed-classes-and-class-members.md)
- [方法](./methods.md)
- [事件](../events/index.md)
- [属性](./properties.md)
- [索引器](../indexers/index.md)
- [类型](../types/index.md)
