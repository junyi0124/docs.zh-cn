---
title: C# 中的继承
description: 了解如何在 C# 库和应用程序中运用继承。
ms.date: 07/05/2018
ms.technology: csharp-fundamentals
ms.assetid: aeb68c74-0ea0-406f-9fbe-2ce02d47ef31
ms.openlocfilehash: b0d6a4a3db4d6606375cc3364ee7abc3029f1d9b
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91190457"
---
# <a name="inheritance-in-c-and-net"></a>C# 和 .NET 中的继承

此教程将介绍 C# 中的继承。 继承是面向对象的编程语言的一项功能，可方便你定义提供特定功能（数据和行为）的基类，并定义继承或重写此功能的派生类。

## <a name="prerequisites"></a>先决条件

本教程假定你已安装 .NET Core SDK。 请访问 [.NET Core 下载](https://dotnet.microsoft.com/download)页进行下载。 还需要安装代码编辑器。 此教程使用 [Visual Studio Code](https://code.visualstudio.com)，但你可以选择使用任何代码编辑器。

## <a name="running-the-examples"></a>运行示例

若要创建并运行此教程中的示例，请通过命令行使用 [dotnet](../../core/tools/dotnet.md) 实用工具。 对于每个示例，请按照以下步骤操作：

1. 创建用于存储示例的目录。
1. 在命令提示符处，输入 [dotnet new console](../../core/tools/dotnet-new.md) 命令，新建 .NET Core 项目。
1. 将示例中的代码复制并粘贴到代码编辑器中。
1. 在命令行处输入 [dotnet restore](../../core/tools/dotnet-restore.md) 命令，加载或还原项目的依赖项。

   [!INCLUDE[DotNet Restore Note](~/includes/dotnet-restore-note.md)]

1. 输入 [dotnet run](../../core/tools/dotnet-run.md) 命令，编译并执行示例。

## <a name="background-what-is-inheritance"></a>背景：什么是继承？

*继承*是面向对象的编程的一种基本特性。 借助继承，能够定义可重用（继承）、扩展或修改父类行为的子类。 成员被继承的类称为*基类*。 继承基类成员的类称为*派生类*。

C# 和 .NET 只支持*单一继承*。 也就是说，类只能继承自一个类。 不过，继承是可传递的。这样一来，就可以为一组类型定义继承层次结构。 换言之，类型 `D` 可继承自类型 `C`，其中类型 `C` 继承自类型 `B`，类型 `B` 又继承自基类类型 `A`。 由于继承是可传递的，因此类型 `D` 继承了类型 `A` 的成员。

并非所有基类成员都可供派生类继承。 以下成员无法继承：

- [静态构造函数](../programming-guide/classes-and-structs/static-constructors.md)：用于初始化类的静态数据。

- [实例构造函数](../programming-guide/classes-and-structs/constructors.md)：在创建类的新实例时调用。 每个类都必须定义自己的构造函数。

- [终结器](../programming-guide/classes-and-structs/destructors.md)：由运行时的垃圾回收器调用，用于销毁类实例。

虽然基类的其他所有成员都可供派生类继承，但这些成员是否可见取决于它们的可访问性。 成员的可访问性决定了其是否在派生类中可见，如下所述：

- 只有在基类中嵌套的派生类中，[私有](../language-reference/keywords/private.md)成员才可见。 否则，此类成员在派生类中不可见。 在以下示例中，`A.B` 是派生自 `A` 的嵌套类，而 `C` 则派生自 `A`。 私有 `A.value` 字段在 A.B 中可见。 不过，如果从 `C.GetValue` 方法中删除注释并尝试编译示例，则会生成编译器错误 CS0122：“'A.value' 不可访问，因为它具有一定的保护级别。”

  [!code-csharp[Inheritance](../../../samples/snippets/csharp/tutorials/inheritance/private.cs#1)]

- [受保护](../language-reference/keywords/protected.md)成员仅在派生类中可见。

- [内部](../language-reference/keywords/internal.md)成员仅在与基类同属一个程序集的派生类中可见， 在与基类属于不同程序集的派生类中不可见。

- [公共](../language-reference/keywords/public.md)成员在派生类中可见，并且属于派生类的公共接口。 可以调用继承的公共成员，就像它们是在派生类中定义一样。 在以下示例中，类 `A` 定义 `Method1` 方法，类 `B` 继承自类 `A`。 然后，以下示例调用 `Method1`，就像它是 `B` 中的实例方法一样。

  [!code-csharp[Inheritance](../../../samples/snippets/csharp/tutorials/inheritance/basics.cs#1)]

派生类还可以通过提供重写实现代码来*重写*继承的成员。 基类成员必须标记有 [virtual](../language-reference/keywords/virtual.md) 关键字，才能重写继承的成员。 默认情况下，基类成员没有 `virtual` 标记，因此无法被重写。 如果尝试重写非虚成员（如以下示例所示），则会生成编译器错误 CS0506：“\<member> 无法重写继承的成员 \<member>，因为继承的成员没有 virtual、abstract 或 override 标记。”

```csharp
public class A
{
    public void Method1()
    {
        // Do something.
    }
}

public class B : A
{
    public override void Method1() // Generates CS0506.
    {
        // Do something else.
    }
}
```

在某些情况下，派生类*必须*重写基类实现代码。 标记有 [abstract](../language-reference/keywords/abstract.md) 关键字的基类成员要求派生类必须重写它们。 如果尝试编译以下示例，则会生成编译器错误 CS0534：“&lt;class&gt; 不实现继承的抽象成员 &lt;member&gt;”，因为类 `B` 没有提供 `A.Method1` 的实现代码。

```csharp
public abstract class A
{
    public abstract void Method1();
}

public class B : A // Generates CS0534.
{
    public void Method3()
    {
        // Do something.
    }
}
```

继承仅适用于类和接口。 其他各种类型（结构、委托和枚举）均不支持继承。 因为这些规则的存在，如果尝试编译以下代码，则会生成编译器错误 CS0527：“接口列表中的类型“ValueType”不是接口。” 此错误消息指明，尽管可以定义结构实现的接口，但不支持继承。

```csharp
using System;

public struct ValueStructure : ValueType // Generates CS0527.
{
}
```

## <a name="implicit-inheritance"></a>隐式继承

.NET 类型系统中的所有类型除了可以通过单一继承进行继承之外，还可以隐式继承自 <xref:System.Object> 或其派生的类型。 <xref:System.Object> 的常用功能可用于任何类型。

为了说明隐式继承的具体含义，让我们来定义一个新类 `SimpleClass`，这只是一个空类定义：

[!code-csharp[Inheritance](../../../samples/snippets/csharp/tutorials/inheritance/simpleclass.cs#1)]

然后可以使用反射（便于检查类型的元数据，从而获取此类型的相关信息），获取 `SimpleClass` 类型的成员列表。 尽管没有在 `SimpleClass` 类中定义任何成员，但示例输出表明它实际上有九个成员。 这些成员的其中之一是由 C# 编译器自动为 `SimpleClass` 类型提供的无参数（或默认）构造函数。 剩余八个是 <xref:System.Object>（.NET 类型系统中的所有类和接口最终隐式继承自的类型）的成员。

[!code-csharp[Inheritance](../../../samples/snippets/csharp/tutorials/inheritance/simpleclass.cs#2)]

由于隐式继承自 <xref:System.Object> 类，因此 `SimpleClass` 类可以使用下面这些方法：

- 公共 `ToString` 方法将 `SimpleClass` 对象转换为字符串表示形式，返回完全限定的类型名称。 在这种情况下，`ToString` 方法返回字符串“SimpleClass”。

- 三个用于测试两个对象是否相等的方法：公共实例 `Equals(Object)` 方法、公共静态 `Equals(Object, Object)` 方法和公共静态 `ReferenceEquals(Object, Object)` 方法。 默认情况下，这三个方法测试的是引用相等性；也就是说，两个对象变量必须引用同一个对象，才算相等。

- 公共 `GetHashCode` 方法：计算允许在经哈希处理的集合中使用类型实例的值。

- 公共 `GetType` 方法：返回表示 `SimpleClass` 类型的 <xref:System.Type> 对象。

- 受保护 <xref:System.Object.Finalize%2A> 方法：用于在垃圾回收器回收对象的内存之前释放非托管资源。

- 受保护 <xref:System.Object.MemberwiseClone%2A> 方法：创建当前对象的浅表复制。

由于是隐式继承，因此可以调用 `SimpleClass` 对象中任何继承的成员，就像它实际上是 `SimpleClass` 类中定义的成员一样。 例如，下面的示例调用 `SimpleClass` 从 <xref:System.Object> 继承而来的 `SimpleClass.ToString` 方法。

[!code-csharp[Inheritance](../../../samples/snippets/csharp/tutorials/inheritance/simpleclass2.cs#1)]

下表列出了可以在 C# 中创建的各种类型及其隐式继承自的类型。 每个基类型通过继承向隐式派生的类型提供一组不同的成员。

| 类型类别 | 隐式继承自                                                      |
| ------------- | ----------------------------------------------------------------------------- |
| class         | <xref:System.Object>                                                          |
| struct        | <xref:System.ValueType>, <xref:System.Object>                                 |
| enum          | <xref:System.Enum>, <xref:System.ValueType>, <xref:System.Object>             |
| delegate      | <xref:System.MulticastDelegate>, <xref:System.Delegate>, <xref:System.Object> |

## <a name="inheritance-and-an-is-a-relationship"></a>继承和“is a”关系

通常情况下，继承用于表示基类和一个或多个派生类之间的“is a”关系，其中派生类是基类的特定版本；派生类是基类的具体类型。 例如，`Publication` 类表示任何类型的出版物，`Book` 和 `Magazine` 类表示出版物的具体类型。

> [!NOTE]
> 一个类或结构可以实现一个或多个接口。 虽然接口实现代码通常用作单一继承的解决方法或对结构使用继承的方法，但它旨在表示接口与其实现类型之间的不同关系（即“can do”关系），而不是继承关系。 接口定义了其向实现类型提供的一部分功能（如测试相等性、比较或排序对象，或支持区域性敏感的分析和格式设置）。

请注意，“is a”还表示类型与其特定实例化之间的关系。 在以下示例中，`Automobile` 类包含三个唯一只读属性：`Make`（汽车制造商）、`Model`（汽车型号）和 `Year`（汽车出厂年份）。 `Automobile` 类还有一个自变量被分配给属性值的构造函数，并将 <xref:System.Object.ToString%2A?displayProperty=nameWithType> 方法重写为生成唯一标识 `Automobile` 实例（而不是 `Automobile` 类）的字符串。

[!code-csharp[Inheritance](../../../samples/snippets/csharp/tutorials/inheritance/is-a.cs#1)]

在这种情况下，不得依赖继承来表示特定汽车品牌和型号。 例如，不需要定义 `Packard` 类型来表示帕卡德制造的汽车。 相反，可以通过创建将相应值传递给其类构造函数的 `Automobile` 对象来进行表示，如以下示例所示。

[!code-csharp[Inheritance](../../../samples/snippets/csharp/tutorials/inheritance/is-a.cs#2)]

基于继承的“is a”关系最适用于基类和向基类添加附加成员或需要基类没有的其他功能的派生类。

## <a name="designing-the-base-class-and-derived-classes"></a>设计基类及其派生类

让我们来看看如何设计基类及其派生类。 在此部分中，将定义一个基类 `Publication`，用于表示任何类型的出版物，如书籍、杂志、报纸、期刊、文章等。还将定义一个从 `Publication` 派生的 `Book` 类。 可以将示例轻松扩展为定义其他派生类，如 `Magazine`、`Journal`、`Newspaper` 和 `Article`。

### <a name="the-base-publication-class"></a>Publication 基类

设计 `Publication` 类时，需要做出下面几项设计决策：

- 要在 `Publication` 基类中添加哪些成员、`Publication` 成员是否提供方法实现或 `Publication` 是否是用作派生类模板的抽象基类。

  在此示例中，`Publication` 类提供方法实现代码。 [设计抽象基类及其派生类](#abstract)部分中的示例就使用抽象基类定义派生类必须重写的方法。 派生类可以随时提供适合派生类型的任意实现代码。

  能够重用代码（即多个派生类共用基类方法的声明和实现代码，无需重写它们）是非抽象基类的优势所在。 因此，如果代码可能由某些或大多数特定 `Publication` 类型共用，则应向 `Publication` 添加成员。 如果无法有效地提供基类实现，则最终将不得不在派生类中提供基本相同的成员实现代码，而不是共用基类中的同一实现代码。 如果需要在多个位置保留重复的代码，可能会导致 bug 出现。

  为了最大限度地提高代码重用性并创建合乎逻辑的直观继承层次结构，需要确保在 `Publication` 类中只添加所有或大多数出版物通用的数据和功能。 然后，派生类可以实现所表示的特定出版物种类的唯一成员。

- 类层次结构的扩展空间大小。 是否要开发包含三个或更多类的层次结构，而不是仅包含一个基类和一个或多个派生类？ 例如，`Publication` 可以是 `Periodical` 的基类，而后者又是 `Magazine`、`Journal` 和 `Newspaper` 的基类。

  在示例中，将使用包含 `Publication` 类和一个派生类 `Book` 的小型层次结构。 可以轻松扩展此示例，使其可以创建其他许多派生自 `Publication` 的类，如 `Magazine` 和 `Article`。

- 能否实例化基类。 如果不可以，则应向类应用 [abstract](../language-reference/keywords/abstract.md) 关键字。 否则，可通过调用类构造函数来实例化 `Publication` 类。 如果尝试通过直接调用类构造函数来实例化标记有 `abstract` 关键字的类，则 C# 编译器会生成错误 CS0144：“无法创建抽象类或接口的实例。” 如果尝试使用反射来实例化类，则反射方法会抛出 <xref:System.MemberAccessException>。

  默认情况下，可以通过调用类构造函数来实例化基类。 无需显式定义类构造函数。 如果基类的源代码中没有类构造函数，C# 编译器会自动提供默认的（无参数）构造函数。

  在此示例中，将把 `Publication` 类标记为 [abstract](../language-reference/keywords/abstract.md)，使其无法实例化。  一个不具备任何 `abstract` 方法的 `abstract` 类表示该类代表一个在几个具体类（例如 `Book` 和 `Journal`）之间共享的抽象概念。

- 派生类是否必须继承特定成员的基类实现代码、是否能选择重写基类实现代码或者是否必须提供实现代码。 使用 [abstract](../language-reference/keywords/abstract.md) 关键字来强制派生类提供实现代码。 使用 [virtual](../language-reference/keywords/virtual.md) 关键字来允许派生类重写基类方法。 默认情况下，*不*可重写基类中定义的方法。

  `Publication` 类不具备任何 `abstract` 方法，不过类本身是 `abstract`。

- 派生类是否表示继承层次结构中的最终类，且本身不能用作其他派生类的基类。 默认情况下，任何类都可以用作基类。 可以应用 [sealed](../language-reference/keywords/sealed.md) 关键字来指明类不能用作其他任何类的基类。 如果尝试从密封类派生，则会生成编译器错误 CS0509：“无法从密封类型 \<typeName> 派生。”

  在此示例中，将把派生类标记为 `sealed`。

以下示例展示了 `Publication` 类的源代码，以及 `Publication.PublicationType` 属性返回的 `PublicationType` 枚举。 除了继承自 <xref:System.Object> 的成员之外，`Publication` 类还定义了以下唯一成员和成员重写：

[!code-csharp[Inheritance](../../../samples/snippets/csharp/tutorials/inheritance/base-and-derived.cs#1)]

- 构造函数

  由于 `Publication` 类标记有 `abstract`，因此无法直接通过以下代码进行实例化：

  ```csharp
  var publication = new Publication("Tiddlywinks for Experts", "Fun and Games",
                                    PublicationType.Book);
  ```

  不过，它的实例构造函数可以直接通过派生类构造函数进行调用，如 `Book` 类的源代码所示。

- 两个与出版物相关的属性

  `Title` 是只读 <xref:System.String> 属性，其值通过调用 `Publication` 构造函数提供。

  `Pages` 是读写 <xref:System.Int32> 属性，用于指明出版物的总页数。 值存储在 `totalPages` 私有字段中。 值必须为正数，否则会抛出 <xref:System.ArgumentOutOfRangeException>。

- 与出版商相关的成员

  两个只读属性：`Publisher` 和 `Type`。 值最初是通过调用 `Publication` 类构造函数来提供。

- 与出版相关的成员

  两个方法（`Publish` 和 `GetPublicationDate`）用于设置并返回发布日期。 调用时，`Publish` 方法会将 `published` 标志设置为 `true`，并将传递给它的日期作为自变量分配给 `datePublished` 私有字段。 如果 `published` 标志为 `false`，`GetPublicationDate` 方法会返回字符串“NYP”；如果为 `true`，则会返回 `datePublished` 字段的值。

- 与版权相关的成员

  `Copyright` 方法需要将版权所有者的姓名和版权授予年份用作参数，并将它们分配给属性 `CopyrightName` 和 `CopyrightDate`。

- 重写 `ToString` 方法

  如果类型不重写 <xref:System.Object.ToString%2A?displayProperty=nameWithType> 方法，则返回类型的完全限定的名称，这对于区分实例没什么用。 `Publication` 类将 <xref:System.Object.ToString%2A?displayProperty=nameWithType> 重写为返回 `Title` 属性值。

下图展示了基类 `Publication` 及其隐式继承类 <xref:System.Object> 之间的关系。

![Object 和 Publication 类](media/publication-class.jpg)

### <a name="the-book-class"></a>`Book` 类

`Book` 类表示作为一种特定类型出版物的书籍。 下面的示例展示了 `Book` 类的源代码。

[!code-csharp[Inheritance](../../../samples/snippets/csharp/tutorials/inheritance/base-and-derived.cs#2)]

除了继承自 `Publication` 的成员之外，`Book` 类还定义了以下唯一成员和成员重写：

- 两个构造函数

  两个 `Book` 构造函数共用三个常见参数。 其中两个参数（*title* 和 *publisher*）对应于 `Publication` 构造函数的相应参数。 第三个参数是 *author*，存储在不可变的 `Author` 属性中。 其中一个构造函数包含存储在 `ISBN` 自动属性中的 isbn 参数。

  第一个构造函数使用 [this](../language-reference/keywords/this.md) 关键字来调用另一个构造函数。 构造函数链是常见的构造函数定义模式。 调用参数最多的构造函数时，由参数较少的构造函数提供默认值。

  第二个构造函数使用 [base](../language-reference/keywords/base.md) 关键字，将标题和出版商名称传递给基类构造函数。 如果没有在源代码中显式调用基类构造函数，那么 C# 编译器会自动提供对基类的默认或无参数构造函数的调用。

- 只读 `ISBN` 属性，用于返回 `Book` 对象的国际标准书号，即 10 位或 13 位的专属编号。 ISBN 作为参数提供给 `Book` 构造函数之一。 ISBN 存储在私有支持字段中，由编译器自动生成。

- 只读 `Author` 属性。 作者姓名作为参数提供给两个 `Book` 构造函数，并存储在属性中。

- 两个与价格相关的只读属性（`Price` 和 `Currency`）。 值作为自变量提供给调用的 `SetPrice` 方法。 `Currency` 属性是三位的 ISO 货币符号（例如，USD 表示美元）。 可以从 <xref:System.Globalization.RegionInfo.ISOCurrencySymbol%2A> 属性检索 ISO 货币符号。 这两个属性均为外部只读，但均可在 `Book` 类中由代码设置。

- `SetPrice` 方法，用于设置 `Price` 和 `Currency` 属性的值。 这些值由那些相同属性返回。

- 重写 `ToString` 方法（继承自 `Publication`）、<xref:System.Object.Equals%28System.Object%29?displayProperty=nameWithType> 和 <xref:System.Object.GetHashCode%2A> 方法（继承自 <xref:System.Object>）。

  除非重写，否则 <xref:System.Object.Equals%28System.Object%29?displayProperty=nameWithType> 方法测试的是引用相等性。 也就是说，两个对象变量必须引用同一个对象，才算相等。 相比之下，在 `Book` 类中，两个 `Book` 对象必须包含相同的 ISBN，才算相等。

  重写 <xref:System.Object.Equals%28System.Object%29?displayProperty=nameWithType> 方法时，还必须重写 <xref:System.Object.GetHashCode%2A> 方法，此方法返回运行时为了实现高效检索，在经哈希处理的集合中存储项所使用的值。 哈希代码应返回与测试相等性一致的值。 由于已将 <xref:System.Object.Equals%28System.Object%29?displayProperty=nameWithType> 重写为在两个 `Book` 对象的 ISBN 属性相等时返回 `true`，因此返回的哈希代码是通过调用 `ISBN` 属性返回的字符串的 <xref:System.String.GetHashCode%2A> 方法计算得出。

下图展示了 `Book` 类及其基类 `Publication` 之间的关系。

![Publication 和 Book 类](media/book-class.jpg)

现在可以实例化 `Book` 对象，调用其唯一成员和继承的成员，并将其作为自变量传递给需要 `Publication` 或 `Book` 类型参数的方法，如以下示例所示。

[!code-csharp[Inheritance](../../../samples/snippets/csharp/tutorials/inheritance/use-publication.cs#1)]

## <a name="designing-abstract-base-classes-and-their-derived-classes"></a>设计抽象基类及其派生类

<a name="abstract"></a>

在上面的示例中定义了一个基类，它提供了许多方法的实现代码，以便派生类可以共用代码。 然而，在许多情况下，我们并不希望基类提供实现代码。 相反，基类是声明抽象方法的抽象类，用作定义每个派生类必须实现的成员的模板 。 通常情况下，在抽象基类中，每个派生类型的实现代码都是相应类型的专属代码。 尽管该类提供了出版物通用的功能的实现代码，但由于实例化 `Publication` 对象毫无意义，因此，使用 abstract 关键字来标记该类。

例如，每个封闭的二维几何形状都包含两个属性：面积（即形状的内部空间）和周长（或沿形状一周的长度）。 然而，这两个属性的计算方式完全取决于具体的形状。 例如，圆和三角形的周长计算公式就有所不同。 `Shape` 类是一个包含 `abstract` 方法的 `abstract` 类。 这表示派生类共享相同的功能，但这些派生类以不同的方式实现该功能。

以下示例定义了 `Shape` 抽象基类，此基类又定义了两个属性：`Area` 和 `Perimeter`。 除了用 [abstract](../language-reference/keywords/abstract.md) 关键字标记类之外，还需要用 [abstract](../language-reference/keywords/abstract.md) 关键字标记每个实例成员。 在此示例中，`Shape` 还将 <xref:System.Object.ToString%2A?displayProperty=nameWithType> 方法重写为返回类型的名称，而不是其完全限定的名称。 基类还定义了两个静态成员（`GetArea` 和 `GetPerimeter`），以便调用方可以轻松检索任何派生类实例的面积和周长。 将派生类实例传递给两个方法中的任意一个时，运行时调用的是派生类重写的方法。

[!code-csharp[Inheritance](../../../samples/snippets/csharp/tutorials/inheritance/shape.cs#1)]

然后可以从表示特定形状的 `Shape` 派生一些类。 以下示例定义了三个类：`Triangle`、`Rectangle` 和 `Circle`。 每个类都使用特定形状的专属公式来计算面积和周长。 一些派生类还定义所表示形状的专属属性（如 `Rectangle.Diagonal` 和 `Circle.Diameter`）。

[!code-csharp[Inheritance](../../../samples/snippets/csharp/tutorials/inheritance/shape.cs#2)]

以下示例使用派生自 `Shape` 的对象。 它实例化派生自 `Shape` 的一组对象，然后调用 `Shape` 类的静态方法，用于包装返回的 `Shape` 属性值。 运行时从派生类型的重写属性检索值。 以下示例还将数组中的每个 `Shape` 对象显式转换成其派生类型；如果显式转换成功，则检索 `Shape` 的特定子类的属性。

[!code-csharp[Inheritance](../../../samples/snippets/csharp/tutorials/inheritance/shape.cs#3)]

## <a name="see-also"></a>请参阅

- [继承（C# 编程指南）](../programming-guide/classes-and-structs/inheritance.md)
