---
title: 定义类型及其成员 - C# 教程
description: 程序的构建基块是类型。 了解如何使用 C# 创建类、结构、接口等。
ms.date: 08/06/2020
ms.openlocfilehash: efd353fe8c1e6a57952bcb2586a05ad38ecd52b9
ms.sourcegitcommit: 0802ac583585110022beb6af8ea0b39188b77c43
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "88559110"
---
# <a name="types-and-members"></a>类型和成员

## <a name="classes-and-objects"></a>类和对象

*类* 是最基本的 C# 类型。 类是一种数据结构，可在一个单元中就将状态（字段）和操作（方法和其他函数成员）结合起来。 类为类实例（亦称为“对象”）提供了定义 。 类支持 *继承* 和 *多形性*，即 *派生类* 可以扩展和专门针对 *基类* 的机制。

新类使用类声明进行创建。 类声明以标头开头。 标头指定以下内容：

- 类的特性和修饰符
- 类的名称
- 基类（从[基类](#base-classes)继承时）
- 接口由该类实现。

标头后面是类主体，由在分隔符 `{` 和 `}` 内编写的成员声明列表组成。

以下代码展示的是简单类 `Point` 的声明：

:::code language="csharp" source="./snippets/shared/Types.cs" ID="PointClass":::

类实例是使用 `new` 运算符进行创建，此运算符为新实例分配内存，调用构造函数来初始化实例，并返回对实例的引用。 以下语句创建两个 `Point` 对象，并将对这些对象的引用存储在两个变量中：

:::code language="csharp" source="./snippets/shared/Types.cs" ID="CreatePoints":::

当无法再访问对象时，对象占用的内存会被自动回收。 既没必要，也无法在 C# 中显式解除分配对象。

### <a name="type-parameters"></a>类型参数

泛型类定义[类型参数](../programming-guide/generics/index.md)*。 类型参数是用尖括号括起来的类型参数名称列表。 类型参数跟在类名后面。 然后，可以在类声明的主体中使用类型参数来定义类成员。 在以下示例中，`Pair` 的类型参数是 `TFirst` 和 `TSecond`：

:::code language="csharp" source="./snippets/shared/Types.cs" ID="DefinePairClass":::

声明为需要使用类型参数的类类型被称为“泛型类类型”。 结构、接口和委托类型也可以是泛型。
使用泛型类时，必须为每个类型参数提供类型自变量：

:::code language="csharp" source="./snippets/shared/Types.cs" ID="CreatePairObject":::

包含类型自变量的泛型类型（如上面的 `Pair<int,string>`）被称为 *构造泛型类型*。

### <a name="base-classes"></a>基类

类声明可以指定基类。 在类名和类型参数后面加上冒号和基类的名称。 省略基类规范与从 `object` 类型派生相同。 在以下示例中，`Point3D` 的基类是 `Point` 在第一个示例中，`Point` 的基类是 `object`：

:::code language="csharp" source="./snippets/shared/Types.cs" ID="Create3DPoint":::

类继承其基类的成员。 继承意味着一个类隐式包含其基类的几乎所有成员。 类不继承实例、静态构造函数以及终结器。 派生类可以在其继承的成员中添加新成员，但无法删除继承成员的定义。 在上面的示例中，`Point3D` 从 `Point` 继承了 `X` 和 `Y` 成员，每个 `Point3D` 实例均包含三种属性（`X`、`Y` 和 `Z`）。

可以将类类型隐式转换成其任意基类类型。 类类型的变量可以引用相应类的实例或任意派生类的实例。 例如，类声明如上，`Point` 类型的变量可以引用 `Point` 或 `Point3D`：

:::code language="csharp" source="./snippets/shared/Types.cs" ID="ImplicitCastToBase":::

## <a name="structs"></a>结构

类定义可支持继承和多形性的类型。 它们使你能够基于派生类的层次结构创建复杂的行为。 相比之下，[结构](../language-reference/builtin-types/struct.md)类型是较为简单的类型，其主要目的是存储数据值*。 结构不能声明基类型；它们从 <xref:System.ValueType?displayProperty=nameWithType> 隐式派生。 不能从 `struct` 类型派生其他 `struct` 类型。 这些类型已隐式密封。

:::code language="csharp" source="./snippets/shared/Types.cs" ID="PointStruct":::

## <a name="interfaces"></a>接口

[接口](../programming-guide/interfaces/index.md)定义了可由类和结构实现的协定。 接口可以包含方法、属性、事件和索引器。 接口通常不提供所定义成员的实现，仅指定必须由实现接口的类或结构提供的成员。

接口可以采用“多重继承”。 在以下示例中，接口 `IComboBox` 同时继承自 `ITextBox` 和 `IListBox`。

:::code language="csharp" source="./snippets/shared/Types.cs" ID="FirstInterfaces":::

类和结构可以实现多个接口。 在以下示例中，类 `EditBox` 同时实现 `IControl` 和 `IDataBound`。

:::code language="csharp" source="./snippets/shared/Types.cs" ID="ImplementInterfaces":::

当类或结构实现特定接口时，此类或结构的实例可以隐式转换成相应的接口类型。 例如

:::code language="csharp" source="./snippets/shared/Types.cs" ID="UseInterfaces":::

## <a name="enums"></a>枚举

[枚举](../language-reference/builtin-types/enum.md)类型定义了一组常数值。 以下 `enum` 声明了定义不同根蔬菜的常数：

:::code language="csharp" source="./snippets/shared/Types.cs" ID="EnumDeclaration":::

还可以定义一个 `enum` 作为标志组合使用。 以下声明为四季声明了一组标志。 可以随意搭配季节组合，包括 `All` 值（包含所有季节）：

:::code language="csharp" source="./snippets/shared/Types.cs" ID="FlagsEnumDeclaration":::

以下示例显示了前面两个枚举的声明：

:::code language="csharp" source="./snippets/shared/Types.cs" ID="UsingEnums":::

## <a name="nullable-types"></a>可为 null 的类型

任何类型的变量都可以声明为“不可为 null”或“可为 null” 。 可为 null 的变量包含一个额外的 `null` 值，表示没有值。 可为 null 的值类型（结构或枚举）由 <xref:System.Nullable%601?displayProperty=nameWithType> 表示。 不可为 null 和可为 null 的引用类型都由基础引用类型表示。 这种区别由编译器和某些库读取的元数据体现。 当可为 null 的引用在没有先对照 `null` 检查其值的情况下取消引用时，编译器会发出警告。 当对不可为 null 的引用分配了可能为 `null` 的值时，编译器也会发出警告。 以下示例声明了“可为 null 的 int”，并将其初始化为 `null`。 然后将值设置为 `5`。 该例通过“可为 null 的字符串”演示了同一概念。 有关详细信息，请参阅[可为 null 的值类型](../language-reference/builtin-types/nullable-value-types.md)和[可为 null 的引用类型](../nullable-references.md)。

:::code language="csharp" source="./snippets/shared/Types.cs" ID="DeclareNullable":::

## <a name="tuples"></a>元组

C# 支持[元组](../language-reference/builtin-types/value-tuples.md)，后者提供了简洁的语法来将多个数据元素分组成一个轻型数据结构*。 通过声明 `(` 和 `)` 之间的成员的类型和名称来实例化元组，如下例所示：

:::code language="csharp" source="./snippets/shared/Types.cs" ID="DeclareTuples":::

元组为具有多个成员的数据结构提供了一种替代方法，且无需使用下一篇文章中介绍的构建基块。

>[!div class="step-by-step"]
>[上一页](index.md)
>[下一页](program-building-blocks.md)
