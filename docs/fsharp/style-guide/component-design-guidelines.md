---
title: F# 组件设计准则
description: '了解用于编写用于其他调用方的使用的 F # 组件的准则。'
ms.date: 05/14/2018
ms.openlocfilehash: 24be2a422c97b9334f749e3d9dfcccd0feec219b
ms.sourcegitcommit: e395fabeeea5c705d243d246fa64446839ac85b6
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/03/2021
ms.locfileid: "97856102"
---
# <a name="f-component-design-guidelines"></a>F# 组件设计准则

本文档是针对 F # 编程的一组组件设计准则，基于 F # 组件设计准则、v14、Microsoft Research 以及 F # Software Foundation 最初特选和维护的版本。

本文档假设你熟悉 F # 编程。 很多感谢在本指南的各个版本上对 F # 社区进行贡献和有用的反馈。

## <a name="overview"></a>概述

本文档介绍与 F # 组件的设计和编码相关的一些问题。 组件可以表示以下任意内容：

* F # 项目中的一个层，它在该项目中具有外部使用者。
* 用于跨程序集边界的 F # 代码使用的库。
* 用于跨程序集边界使用任意 .NET 语言的库。
* 一个库，用于通过包存储库（如 [NuGet](https://nuget.org)）进行分发。

本文中所述的方法遵循 [良好 F # 代码的五个原则](index.md#five-principles-of-good-f-code)，并根据需要使用函数和对象编程。

无论采用哪种方法，组件和库设计器都在尝试创建最能由开发人员使用的 API 时，会面临许多实际和 prosaic 的问题。 [.Net 库设计准则](../../standard/design-guidelines/index.md)的 Conscientious 应用程序将指导你创建一组一致的 api，这些 api 可供使用。

## <a name="general-guidelines"></a>一般指南

无论库的目标受众如何，都有一些适用于 F # 库的通用准则。

### <a name="learn-the-net-library-design-guidelines"></a>了解 .NET 库设计准则

无论你正在执行哪种 F # 编码，都有必要了解 [.Net 库设计准则](../../standard/design-guidelines/index.md)的工作知识。 大多数其他 F # 和 .NET 程序员都将熟悉这些准则，并且预计 .NET 代码符合这些准则。

.NET 库设计指南提供了有关命名、设计类和接口、成员设计 (属性、方法、事件 ) 等）的一般指南，它们是各种设计指南的一种有用的第一个参考点。

### <a name="add-xml-documentation-comments-to-your-code"></a>向代码添加 XML 文档注释

有关公共 Api 的 XML 文档可确保用户可以在使用这些类型和成员时获得出色的 Intellisense 和 Quickinfo，并为库启用生成文档文件。 请参阅 [Xml 文档](../language-reference/xml-documentation.md) ，了解可用于 xmldoc 注释中的其他标记的各种 xml 标记。

```fsharp
/// A class for representing (x,y) coordinates
type Point =

    /// Computes the distance between this point and another
    member DistanceTo: otherPoint:Point -> float
```

您可以使用缩写形式 XML 注释 (`/// comment`) 或标准 xml 注释 (`///<summary>comment</summary>`) 。

### <a name="consider-using-explicit-signature-files-fsi-for-stable-library-and-component-apis"></a>请考虑使用显式签名文件 ( fsi.exe) 用于稳定库和组件 Api

在 F # 库中使用显式签名文件可提供公共 API 的简洁摘要，有助于确保了解库的完整公共表面，并提供公共文档和内部实现细节之间的完全分离。 签名文件通过要求在实现和签名文件中进行更改，增加了更改公共 API 的摩擦。 因此，通常只应在 API 变为日臻完善并且不再需要明显更改时才引入签名文件。

### <a name="always-follow-best-practices-for-using-strings-in-net"></a>始终遵循在 .NET 中使用字符串的最佳做法

遵循 [在 .net 指南中使用字符串的最佳做法](../../standard/base-types/best-practices-strings.md) 。 具体而言，在适用)  (，应始终明确地说明字符串转换和比较中的 *区域性意向* 。

## <a name="guidelines-for-f-facing-libraries"></a>面向 F # 的库的准则

本部分提供有关开发面向公共 F # 的库的建议;也就是说，库公开旨在供 F # 开发人员使用的公共 Api。 有多种适用于 F # 的库设计建议。 如果没有遵循的具体建议，.NET 库设计指导原则就是回退指导。

### <a name="naming-conventions"></a>命名约定

#### <a name="use-net-naming-and-capitalization-conventions"></a>使用 .NET 命名和大小写约定

下表遵循 .NET 命名和大小写约定。 还增加了 F # 构造。

| 构造 | 案例 | 组成部分 | 示例 | 备注 |
|-----------|------|------|----------|-------|
| 具体类型 | PascalCase | 名词/形容词 | List、Double、Complex | 具体类型为结构、类、枚举、委托、记录和联合。 尽管类型名称在 OCaml 中采用小写形式，但 F # 采用了用于类型的 .NET 命名方案。
| DLL           | PascalCase |                 | Fabrikam.Core.dll |  |
| 联合标记     | PascalCase | 名词 | 部分、添加、成功 | 不要在公共 Api 中使用前缀。 （可选）在内部使用前缀，如 `type Teams = TAlpha | TBeta | TDelta.` |
| 事件          | PascalCase | 谓词 | ValueChanged/ValueChanging |  |
| 例外     | PascalCase |      | WebException | 名称应以 "Exception" 结尾。 |
| 字段          | PascalCase | 名词 | CurrentName  | |
| 接口类型 |  PascalCase | 名词/形容词 | IDisposable | 名称应以 "I" 开头。 |
| 方法 |  PascalCase |  谓词 | ToString | |
| 命名空间 | PascalCase | | Fsharp.core | 通常 `<Organization>.<Technology>[.<Subnamespace>]` 会使用，但如果该技术独立于组织，则删除组织。 |
| 参数 | camelCase | 名词 |  类型名称，转换，范围 | |
| 允许值 (内部)  | camelCase 或 PascalCase | 名词/动词 |  getValue，myTable |
| 允许值 (外部)  | camelCase 或 PascalCase | 名词/动词  | 列出地图，日期. 今天 | 当遵循传统的函数设计模式时，允许绑定值通常是公共的。 但是，当标识符可用于其他 .NET 语言时，通常使用 PascalCase。 |
| 属性  | PascalCase  | 名词/形容词  | IsEndOfFile，背景色  | 布尔值属性通常使用 Is 和应为赞成，如 IsEndOfFile 中所示，not IsNotEndOfFile。

#### <a name="avoid-abbreviations"></a>避免缩写

.NET 准则不鼓励使用缩写 (例如，"use `OnButtonClick` 而不是 `OnBtnClick` " ) 。 常见缩写（如 `Async` "异步"）是可接受的。 对于函数编程，有时会忽略此准则;例如， `List.iter` 使用 "迭代" 的缩写。 出于此原因，在 F # 对 F # 编程中使用缩写的程度往往更高，但在公共组件设计中通常不应避免。

#### <a name="avoid-casing-name-collisions"></a>避免大小写名称冲突

.NET 指导原则只使用大小写来区分名称冲突，因为某些客户端语言 (例如，Visual Basic) 不区分大小写。

#### <a name="use-acronyms-where-appropriate"></a>在合适的位置使用首字母缩写

XML 的首字母缩写词不是缩写词，在 uncapitalized 格式 (Xml) 的 .NET 库中广泛使用。 只应使用众所周知的、公认的首字母缩写词。

#### <a name="use-pascalcase-for-generic-parameter-names"></a>对泛型参数名称使用 PascalCase

请在公共 Api 中使用 PascalCase 作为泛型参数名称，包括面向 F # 的库。 特别是，使用、、 `T` `U` `T1` 、适用于 `T2` 任意泛型参数的名称，并且在特定名称有意义时，对于面向 F # 的库，使用类似于 `Key` 、、 (的名称， `Value` `Arg` 如 `TKey`) 。

#### <a name="use-either-pascalcase-or-camelcase-for-public-functions-and-values-in-f-modules"></a>在 F # 模块中为公共函数和值使用 PascalCase 或 camelCase

camelCase 用于设计为使用非限定 (的公共函数，例如 `invalidArg`) 和 "标准集合函数" (例如，List. map) 。 在这两种情况下，函数名称的作用与语言中的关键字非常类似。

### <a name="object-type-and-module-design"></a>对象、类型和模块设计

#### <a name="use-namespaces-or-modules-to-contain-your-types-and-modules"></a>使用命名空间或模块包含类型和模块

组件中的每个 F # 文件都应以命名空间声明或模块声明开头。

```fsharp
namespace Fabrikam.BasicOperationsAndTypes

type ObjectType1() =
    ...

type ObjectType2() =
     ...

module CommonOperations =
    ...
```

或

```fsharp
module Fabrikam.BasicOperationsAndTypes

type ObjectType1() =
    ...

type ObjectType2() =
    ...

module CommonOperations =
    ...
```

使用模块和命名空间在顶层组织代码之间的差异如下：

* 命名空间可以跨多个文件
* 命名空间不能包含 F # 函数，除非它们位于内部模块内
* 任何给定模块的代码都必须包含在单个文件中
* 顶级模块可以包含 F # 函数，而无需内部模块

在顶级命名空间或模块之间进行的选择会影响编译形式的代码，因此，如果您的 API 最终在 F # 代码之外使用，则将影响其他 .NET 语言的视图。

#### <a name="use-methods-and-properties-for-operations-intrinsic-to-object-types"></a>对对象类型的内部操作使用方法和属性

使用对象时，最好确保将使用的功能作为该类型的方法和属性实现。

```fsharp
type HardwareDevice() =

    member this.ID = ...

    member this.SupportedProtocols = ...

type HashTable<'Key,'Value>(comparer: IEqualityComparer<'Key>) =

    member this.Add(key, value) = ...

    member this.ContainsKey(key) = ...

    member this.ContainsValue(value) = ...
```

不一定要在该成员中实现给定成员的大容量功能，但该功能的可耗用部分应该是。

#### <a name="use-classes-to-encapsulate-mutable-state"></a>使用类封装可变状态

在 F # 中，只需在该状态尚未由其他语言构造（如闭包、序列表达式或异步计算）进行封装的位置执行此操作。

```fsharp
type Counter() =
    // let-bound values are private in classes.
    let mutable count = 0

    member this.Next() =
        count <- count + 1
        count
```

#### <a name="use-interfaces-to-group-related-operations"></a>使用接口对相关操作进行分组

使用接口类型来表示一组操作。 这是对其他选项（如函数或函数的记录的元组）的首选。

```fsharp
type Serializer =
    abstract Serialize<'T> : preserveRefEq: bool -> value: 'T -> string
    abstract Deserialize<'T> : preserveRefEq: bool -> pickle: string -> 'T
```

首选为：

```fsharp
type Serializer<'T> = {
    Serialize: bool -> 'T -> string
    Deserialize: bool -> string -> 'T
}
```

接口是 .NET 中的一流概念，你可以使用它们来实现通常会给你带来的函子。 此外，它们还可用于将存在性类型编码到程序中，哪些功能记录不能。

#### <a name="use-a-module-to-group-functions-that-act-on-collections"></a>使用模块对集合的函数进行分组

定义集合类型时，请考虑 `CollectionType.map` 为新集合类型提供一组标准操作，例如和 `CollectionType.iter`) 。

```fsharp
module CollectionType =
    let map f c =
        ...
    let iter f c =
        ...
```

如果包括此类模块，请遵循 Fsharp.core 中的函数的标准命名约定。

#### <a name="use-a-module-to-group-functions-for-common-canonical-functions-especially-in-math-and-dsl-libraries"></a>使用模块将函数分组为常见的规范函数，特别是在数学和 DSL 库中

例如， `Microsoft.FSharp.Core.Operators` 是自动打开的顶级函数集合 (如 `abs` `sin` FSharp.Core.dll 提供的和) 。

同样，统计信息库还可能包括一个模块， `erf` 其中包含函数和 `erfc` ，此模块设计为显式或自动打开。

#### <a name="consider-using-requirequalifiedaccess-and-carefully-apply-autoopen-attributes"></a>请考虑使用 RequireQualifiedAccess 并认真应用 AutoOpen 属性

将 `[<RequireQualifiedAccess>]` 属性添加到模块指示可能未打开该模块，并且对该模块中的元素的引用需要显式限定访问权限。 例如， `Microsoft.FSharp.Collections.List` 模块具有此属性。

当模块中的函数和值有可能与其他模块中的名称冲突的名称时，这将非常有用。 要求限定访问权限可大大增加库的长期可维护性和可进化性。

将 `[<AutoOpen>]` 属性添加到模块意味着当包含命名空间打开时将打开该模块。 `[<AutoOpen>]`特性还可以应用于程序集，以指示在引用程序集时自动打开的模块。

例如，统计信息库 **MathsHeaven** 可能包含 `module MathsHeaven.Statistics.Operators` 包含函数 `erf` 和 `erfc` 。 将此模块标记为是合理的 `[<AutoOpen>]` 。 这意味着 `open MathsHeaven.Statistics` 还将打开此模块并将名称 `erf` 和 `erfc` 纳入范围。 的另一个好用法 `[<AutoOpen>]` 是适用于包含扩展方法的模块。

过度 `[<AutoOpen>]` 使用受污染无法命名空间，并且应谨慎使用特性。 对于特定域中的特定库，明智 `[<AutoOpen>]` 地使用会导致更好的可用性。

#### <a name="consider-defining-operator-members-on-classes-where-using-well-known-operators-is-appropriate"></a>考虑在适当的情况下使用已知运算符，在类上定义运算符成员

有时，类用于为数学构造（如矢量）建模。 当要建模的域具有已知的运算符时，将它们定义为类的内部成员很有用。

```fsharp
type Vector(x: float) =

    member v.X = x

    static member (*) (vector: Vector, scalar: float) = Vector(vector.X * scalar)

    static member (+) (vector1: Vector, vector2: Vector) = Vector(vector1.X + vector2.X)

let v = Vector(5.0)

let u = v * 10.0
```

本指南对应于这些类型的常规 .NET 指导。 但是，在 F # 编码中，这一点也很重要，因为这允许将这些类型与 F # 函数和具有成员约束的方法（如 sumBy）结合使用。

#### <a name="consider-using-compiledname-to-provide-a-net-friendly-name-for-other-net-language-consumers"></a>请考虑使用 CompiledName 来提供。其他 .NET 语言使用者的网络友好名称

有时，你可能想要在一个样式中对 F # 使用方 (（例如，小写的静态成员）进行命名，使其看起来像是模块绑定函数) ，但在将其编译到程序集时，该名称的样式不同。 您可以使用 `[<CompiledName>]` 属性为使用程序集的非 F # 代码提供不同的样式。

```fsharp
type Vector(x:float, y:float) =

    member v.X = x
    member v.Y = y

    [<CompiledName("Create")>]
    static member create x y = Vector (x, y)

let v = Vector.create 5.0 3.0
```

通过使用 `[<CompiledName>]` ，可以将 .net 命名约定用于程序集的非 F # 使用者。

#### <a name="use-method-overloading-for-member-functions-if-doing-so-provides-a-simpler-api"></a>如果这样做，则为成员函数使用方法重载，从而提供更简单的 API

方法重载是一个功能强大的工具，用于简化可能需要执行类似功能但具有不同选项或参数的 API。

```fsharp
type Logger() =

    member this.Log(message) =
        ...
    member this.Log(message, retryPolicy) =
        ...
```

在 F # 中，对参数数量而不是参数类型进行重载更常见。

#### <a name="hide-the-representations-of-record-and-union-types-if-the-design-of-these-types-is-likely-to-evolve"></a>如果这些类型的设计可能会演化，则隐藏记录和联合类型的表示形式

避免泄漏对象的具体表示形式。 例如，值的具体表示形式 <xref:System.DateTime> 不是由 .net 库设计的外部公共 API 显示的。 在运行时，公共语言运行时知道将在整个执行过程中使用的已提交实现。 不过，编译的代码本身不会选取具体表示形式的依赖项。

#### <a name="avoid-the-use-of-implementation-inheritance-for-extensibility"></a>避免对扩展性使用实现继承

在 F # 中，很少使用实现继承。 此外，在新的要求到达时，继承层次结构通常很复杂且难以更改。 在 F # 中仍存在继承实现，这是解决问题的最佳解决方案，但在设计多态性（如接口实现）时，应在 F # 程序中寻找替代方法。

### <a name="function-and-member-signatures"></a>函数和成员签名

#### <a name="use-tuples-for-return-values-when-returning-a-small-number-of-multiple-unrelated-values"></a>当返回的多个不相关值时，将元组用于返回值

下面是在返回类型中使用元组的一个很好的示例：

```fsharp
val divrem: BigInteger -> BigInteger -> BigInteger * BigInteger
```

对于包含多个组件或组件与单个可识别实体相关的返回类型，请考虑使用命名类型而不是元组。

#### <a name="use-asynct-for-async-programming-at-f-api-boundaries"></a>用于 `Async<T>` F # API 边界的异步编程

如果有一个名为的对应同步操作 `Operation` 返回 `T` ，则应将异步操作命名为（ `AsyncOperation` 如果它返回） `Async<T>` 或返回 `OperationAsync` `Task<T>` 。 对于公开开始/结束方法的常用 .NET 类型，请考虑使用 `Async.FromBeginEnd` 将扩展方法编写为外观，以便向这些 .Net api 提供 F # 异步编程模型。

```fsharp
type SomeType =
    member this.Compute(x:int): int =
        ...
    member this.AsyncCompute(x:int): Async<int> =
        ...

type System.ServiceModel.Channels.IInputChannel with
    member this.AsyncReceive() =
        ...
```

### <a name="exceptions"></a>例外

若要了解异常、结果和选项的适当用法，请参阅 [错误管理](conventions.md#error-management) 。

### <a name="extension-members"></a>扩展成员

#### <a name="carefully-apply-f-extension-members-in-f-to-f-components"></a>仔细应用 F # 到 F # 组件中的 F # 扩展成员

F # 扩展成员通常仅适用于在其使用的大多数模式下与类型相关联的内部操作的关闭操作。 一种常见用途是为各种 .NET 类型提供更惯用 F # 的 Api：

```fsharp
type System.ServiceModel.Channels.IInputChannel with
    member this.AsyncReceive() =
        Async.FromBeginEnd(this.BeginReceive, this.EndReceive)

type System.Collections.Generic.IDictionary<'Key,'Value> with
    member this.TryGet key =
        let ok, v = this.TryGetValue key
        if ok then Some v else None
```

### <a name="union-types"></a>联合类型

#### <a name="use-discriminated-unions-instead-of-class-hierarchies-for-tree-structured-data"></a>对树结构数据使用可区分联合，而不是类层次结构

以递归方式定义类似树的结构。 这对继承非常难，但在可区分联合的情况下典雅。

```fsharp
type BST<'T> =
    | Empty
    | Node of 'T * BST<'T> * BST<'T>
```

用可区分联合表示类似于树的数据还可让你从模式匹配中的 exhaustiveness 受益。

#### <a name="use-requirequalifiedaccess-on-union-types-whose-case-names-are-not-sufficiently-unique"></a>`[<RequireQualifiedAccess>]`在大小写名称不够唯一的联合类型上使用

你可能会发现自己在一个域中，其中相同的名称是不同因素的最佳名称，如可区分联合用例。 您可以使用 `[<RequireQualifiedAccess>]` 来消除事例名称的歧义，以避免由于隐藏依赖于语句排序而导致的混乱错误 `open`

#### <a name="hide-the-representations-of-discriminated-unions-for-binary-compatible-apis-if-the-design-of-these-types-is-likely-to-evolve"></a>如果这些类型的设计可能会演化，隐藏二进制兼容 Api 的可区分联合的表示形式

联合类型依赖于简洁编程模型的 F # 模式匹配窗体。 如前文所述，如果这些类型的设计可能会演化，应避免暴露具体的数据表示形式。

例如，可使用私有或内部声明或使用签名文件隐藏可区分联合的表示形式。

```fsharp
type Union =
    private
    | CaseA of int
    | CaseB of string
```

如果未加上地显示可区分的联合，你可能会发现很难对库进行版本分解，而不会破坏用户代码。 应考虑公开一个或多个活动模式，以允许对类型值进行模式匹配。

活动模式提供另一种方法，用于向 F # 使用者提供模式匹配，同时避免直接公开 F # 联合类型。

### <a name="inline-functions-and-member-constraints"></a>内联函数和成员约束

#### <a name="define-generic-numeric-algorithms-using-inline-functions-with-implied-member-constraints-and-statically-resolved-generic-types"></a>通过隐含的成员约束和静态解析的泛型类型，使用内联函数定义一般数值算法

算术成员约束和 F # 比较约束是 F # 编程的标准。 例如，考虑以下代码：

```fsharp
let inline highestCommonFactor a b =
    let rec loop a b =
        if a = LanguagePrimitives.GenericZero<_> then b
        elif a < b then loop a (b - a)
        else loop (a - b) b
    loop a b
```

此函数的类型如下：

```fsharp
val inline highestCommonFactor : ^T -> ^T -> ^T
                when ^T : (static member Zero : ^T)
                and ^T : (static member ( - ) : ^T * ^T -> ^T)
                and ^T : equality
                and ^T : comparison
```

对于数学库中的公共 API，这是一个合适的函数。

#### <a name="avoid-using-member-constraints-to-simulate-type-classes-and-duck-typing"></a>避免使用成员约束来模拟类型类和类型化类型

可以使用 F # 成员约束来模拟 "有类型的键入"。 但是，使用此的成员不应在 F #-F # 库设计中使用。 这是因为基于不熟悉或非标准隐式约束的库设计往往导致用户代码变得不灵活，并与一个特定的框架模式相关联。

此外，通过这种方式大量使用成员约束可能会导致编译时间非常长。

### <a name="operator-definitions"></a>运算符定义

#### <a name="avoid-defining-custom-symbolic-operators"></a>避免定义自定义符号运算符

在某些情况下，自定义运算符非常重要，在大范围的实现代码内非常有用的符号设备。 对于库的新用户，命名函数通常更易于使用。 此外，自定义符号运算符很难记录，用户会发现，由于 IDE 和搜索引擎中存在现有限制，因此更难查找有关运算符的帮助。

因此，最好将您的功能作为命名函数和成员发布，并且仅当符号权益超出了文档和使用它们的认知成本时，才公开此功能的运算符。

### <a name="units-of-measure"></a>度量单位

#### <a name="carefully-use-units-of-measure-for-added-type-safety-in-f-code"></a>在 F # 代码中仔细使用度量单位添加的类型安全

其他 .NET 语言查看时，将清除度量单位的其他键入信息。 请注意，.NET 组件、工具和反射将显示 "类型"-"san-单位"。 例如，c # 使用者将看到 `float` 而不是 `float<kg>` 。

### <a name="type-abbreviations"></a>类型缩写

#### <a name="carefully-use-type-abbreviations-to-simplify-f-code"></a>仔细使用类型缩写来简化 F # 代码

.NET 组件、工具和反射不会查看类型的缩写名称。 类型缩写的重要用法还会使域显得更复杂，这可能会使用户感到困惑。

#### <a name="avoid-type-abbreviations-for-public-types-whose-members-and-properties-should-be-intrinsically-different-to-those-available-on-the-type-being-abbreviated"></a>避免其成员和属性本质上不同于要缩写的类型上可用的公共类型的类型缩写

在这种情况下，要缩写的类型会显示过多关于所定义的实际类型的表示形式。 相反，请考虑在类类型或单用例可区分联合 (中包装缩写，如果性能非常重要，请考虑使用结构类型来包装缩写) 。

例如，将多个地图定义为 F # map 的特殊情况，例如：

```fsharp
type MultiMap<'Key,'Value> = Map<'Key,'Value list>
```

但是，此类型的逻辑点符号运算与映射上的操作不相同–例如，查找运算符映射是合理的。[key] 如果该键不在字典中，则返回空列表，而不是引发异常。

## <a name="guidelines-for-libraries-for-use-from-other-net-languages"></a>用于其他 .NET 语言的库的指南

设计用于其他 .NET 语言的库时，必须遵守 [.Net 库设计准则](../../standard/design-guidelines/index.md)。 在本文档中，这些库标记为 vanilla .NET 库，而不是使用无限制的 F # 的面向 F # 构造的库。 设计 vanilla .NET 库意味着通过最大程度地减少在公共 API 中使用 F # 特定构造，使熟悉的惯用 Api 与 .NET Framework 的其余部分保持一致。 以下部分介绍了这些规则。

### <a name="namespace-and-type-design-for-libraries-for-use-from-other-net-languages"></a>命名空间和类型设计 (适用于其他 .NET 语言的库) 

#### <a name="apply-the-net-naming-conventions-to-the-public-api-of-your-components"></a>将 .NET 命名约定应用于组件的公共 API

请特别注意缩写名称和 .NET 大小写准则的使用。

```fsharp
type pCoord = ...
    member this.theta = ...

type PolarCoordinate = ...
    member this.Theta = ...
```

#### <a name="use-namespaces-types-and-members-as-the-primary-organizational-structure-for-your-components"></a>使用命名空间、类型和成员作为组件的主要组织结构

包含公共功能的所有文件都应以 `namespace` 声明开头，而命名空间中的唯一面向公众的实体应为类型。 不要使用 F # 模块。

使用非公共模块来保存实现代码、实用工具类型和实用工具函数。

静态类型应优先于模块，因为它们允许 API 的未来发展使用重载，以及可能不会在 F # 模块内使用的其他 .NET API 设计概念。

例如，代替以下公共 API：

```fsharp
module Fabrikam

module Utilities =
    let Name = "Bob"
    let Add2 x y = x + y
    let Add3 x y z = x + y + z
```

改为考虑：

```fsharp
namespace Fabrikam

[<AbstractClass; Sealed>]
type Utilities =
    static member Name = "Bob"
    static member Add(x,y) = x + y
    static member Add(x,y,z) = x + y + z
```

#### <a name="use-f-record-types-in-vanilla-net-apis-if-the-design-of-the-types-wont-evolve"></a>如果类型的设计不会演化，请使用 vanilla .NET Api 中的 F # 记录类型

F # 记录类型编译为一个简单的 .NET 类。 它们适用于 Api 中的一些简单的稳定类型。 请考虑使用 `[<NoEquality>]` 和 `[<NoComparison>]` 属性来禁止自动生成接口。 还应避免在 vanilla .NET Api 中使用可变记录字段，因为这些字段公开公共字段。 始终考虑某个类是否会为 API 的未来发展提供更灵活的选项。

例如，以下 F # 代码向 c # 使用者公开公共 API：

F#：

```fsharp
[<NoEquality; NoComparison>]
type MyRecord =
    { FirstThing: int
        SecondThing: string }
```

C#：

```csharp
public sealed class MyRecord
{
    public MyRecord(int firstThing, string secondThing);
    public int FirstThing { get; }
    public string SecondThing { get; }
}
```

#### <a name="hide-the-representation-of-f-union-types-in-vanilla-net-apis"></a>隐藏 vanilla .NET Api 中 F # 联合类型的表示形式

F # 联合类型不常用于组件边界，即使对于 F # 到 F # 编码也是如此。 在组件和库内部使用时，它们是一种优秀的实现设备。

设计 vanilla .NET API 时，请考虑使用私有声明或签名文件隐藏联合类型的表示形式。

```fsharp
type PropLogic =
    private
    | And of PropLogic * PropLogic
    | Not of PropLogic
    | True
```

你还可以使用成员在内部使用联合表示形式来提供所需的类型。面向网络的 API。

```fsharp
type PropLogic =
    private
    | And of PropLogic * PropLogic
    | Not of PropLogic
    | True

    /// A public member for use from C#
    member x.Evaluate =
        match x with
        | And(a,b) -> a.Evaluate && b.Evaluate
        | Not a -> not a.Evaluate
        | True -> true

    /// A public member for use from C#
    static member CreateAnd(a,b) = And(a,b)
```

#### <a name="design-gui-and-other-components-using-the-design-patterns-of-the-framework"></a>使用框架的设计模式设计 GUI 和其他组件

.NET 中提供了许多不同的框架，如 WinForms、WPF 和 ASP.NET。 如果要设计在这些框架中使用的组件，则应该使用每个的命名和设计约定。 例如，对于 WPF 编程，为正在设计的类采用 WPF 设计模式。 对于用户界面编程中的模型，请使用设计模式，如事件和基于通知的集合（如中所述） <xref:System.Collections.ObjectModel> 。

### <a name="object-and-member-design-for-libraries-for-use-from-other-net-languages"></a>用于从其他 .NET 语言使用的库的对象和成员设计 () 

#### <a name="use-the-clievent-attribute-to-expose-net-events"></a>使用 CLIEvent 特性公开 .NET 事件

构造一个 `DelegateEvent` 具有特定 .net 委托类型的，该类型采用对象并 `EventArgs` (而不是 `Event` ，后者仅 `FSharpHandler` 在默认情况下使用该类型) 以便将这些事件以熟悉的方式发布到其他 .net 语言。

```fsharp
type MyBadType() =
    let myEv = new Event<int>()

    [<CLIEvent>]
    member this.MyEvent = myEv.Publish

type MyEventArgs(x: int) =
    inherit System.EventArgs()
    member this.X = x

    /// A type in a component designed for use from other .NET languages
type MyGoodType() =
    let myEv = new DelegateEvent<EventHandler<MyEventArgs>>()

    [<CLIEvent>]
    member this.MyEvent = myEv.Publish
```

#### <a name="expose-asynchronous-operations-as-methods-that-return-net-tasks"></a>将异步操作作为返回 .NET 任务的方法公开

在 .NET 中使用任务来表示活动的异步计算。 任务通常比 F # 对象少复合 `Async<T>` ，因为它们表示 "已在执行" 任务，不能以执行并行组合的方式组合在一起，或者隐藏取消信号和其他上下文参数的传播。

但尽管如此，返回任务的方法是 .NET 上异步编程的标准表示形式。

```fsharp
/// A type in a component designed for use from other .NET languages
type MyType() =

    let compute (x: int): Async<int> = async { ... }

    member this.ComputeAsync(x) = compute x |> Async.StartAsTask
```

经常需要接受显式取消标记：

```fsharp
/// A type in a component designed for use from other .NET languages
type MyType() =
    let compute(x: int): Async<int> = async { ... }
    member this.ComputeAsTask(x, cancellationToken) = Async.StartAsTask(compute x, cancellationToken)
```

#### <a name="use-net-delegate-types-instead-of-f-function-types"></a>使用 .NET 委托类型，而不是 F # 函数类型

此处的 "F # 函数类型" 表示 "箭头" 类型（如） `int -> int` 。

而不是：

```fsharp
member this.Transform(f: int->int) =
    ...
```

执行此操作：

```fsharp
member this.Transform(f: Func<int,int>) =
    ...
```

F # 函数类型显示为 `class FSharpFunc<T,U>` 其他 .net 语言，并不适合理解委托类型的语言功能和工具。 在创作目标为 .NET Framework 3.5 或更高版本的高阶方法时， `System.Func` 和 `System.Action` 委托是正确发布的 api，以使 .net 开发人员能够以低的方式使用这些 api。  (为 .NET Framework 2.0 时，系统定义的委托类型会受到更多的限制;请考虑使用预定义的委托类型，如 `System.Converter<T,U>` 或定义特定的委托类型。 ) 

在反向，.NET 委托并不是面向 F # 的库的自然 (参见面向 F # 的库) 的下一节。 因此，开发 vanilla .NET 库的高阶方法时，一种常见的实现策略是使用 F # 函数类型创作所有实现，然后使用委托作为实际 F # 实现顶部的瘦外观创建公共 API。

#### <a name="use-the-trygetvalue-pattern-instead-of-returning-f-option-values-and-prefer-method-overloading-to-taking-f-option-values-as-arguments"></a>使用 TryGetValue 模式而不是返回 F # 选项值，并首选方法重载以将 F # 选项值作为参数

使用标准的 .NET 设计方法在 vanilla .NET Api 中更好地实现了 Api 中 F # 选项类型的常用模式。 请考虑使用 bool 返回类型和 out 参数，而不是返回 F # 选项值，如 "TryGetValue" 模式。 请考虑使用方法重载或可选参数，而不是将 F # 选项值用作参数。

```fsharp
member this.ReturnOption() = Some 3

member this.ReturnBoolAndOut(outVal: byref<int>) =
    outVal <- 3
    true

member this.ParamOption(x: int, y: int option) =
    match y with
    | Some y2 -> x + y2
    | None -> x

member this.ParamOverload(x: int) = x

member this.ParamOverload(x: int, y: int) = x + y
```

#### <a name="use-the-net-collection-interface-types-ienumerablet-and-idictionarykeyvalue-for-parameters-and-return-values"></a>使用 .NET 集合接口类型的 IEnumerable \<T\> 和 IDictionary \<Key,Value\> 作为参数和返回值

避免使用具体的集合类型，如 .NET 数组 `T[]` 、F # 类型和 `list<T>` `Map<Key,Value>` `Set<T>` .net 具体集合类型 `Dictionary<Key,Value>` （如）。 .NET 库设计指南对于何时使用各种集合类型（如）有很好的建议 `IEnumerable<T>` 。 某些 `T[]` 情况下，在某些情况下，某些情况下使用数组 () 是性能的。 请注意 `seq<T>` ，尤其是的 F # 别名 `IEnumerable<T>` ，因此，seq 通常是 VANILLA .net API 的合适类型。

而不是 F # 列表：

```fsharp
member this.PrintNames(names: string list) =
    ...
```

使用 F # 序列：

```fsharp
member this.PrintNames(names: seq<string>) =
    ...
```

#### <a name="use-the-unit-type-as-the-only-input-type-of-a-method-to-define-a-zero-argument-method-or-as-the-only-return-type-to-define-a-void-returning-method"></a>使用单元类型作为方法的唯一输入类型来定义零参数方法，或用作唯一的返回类型以定义返回 void 的方法

避免使用单元类型。 这很好：

```fsharp
✔ member this.NoArguments() = 3

✔ member this.ReturnVoid(x: int) = ()
```

这是错误的：

```fsharp
member this.WrongUnit( x: unit, z: int) = ((), ())
```

#### <a name="check-for-null-values-on-vanilla-net-api-boundaries"></a>在 vanilla .NET API 边界上检查 null 值

由于不变的设计模式和对 F # 类型使用 null 文本的限制，f # 实现代码的值往往更少。 其他 .NET 语言通常使用 null 作为值。 因此，公开 vanilla .NET API 的 F # 代码应检查 API 边界的 null 参数，并防止这些值更深入地流向 F # 实现代码。 `isNull`可以使用模式的函数或模式匹配 `null` 。

```fsharp
let checkNonNull argName (arg: obj) =
    match arg with
    | null -> nullArg argName
    | _ -> ()

let checkNonNull` argName (arg: obj) =
    if isNull arg then nullArg argName
    else ()
```

#### <a name="avoid-using-tuples-as-return-values"></a>避免使用元组作为返回值

相反，更倾向于返回保存聚合数据的命名类型，或使用 out 参数返回多个值。 虽然元组和结构元组存在于 .NET (包括对结构元组) 的 c # 语言支持，但它们通常不会为 .NET 开发人员提供理想和预期的 API。

#### <a name="avoid-the-use-of-currying-of-parameters"></a>避免使用参数的 currying

请改用 .NET 调用约定 `Method(arg1,arg2,…,argN)` 。

```fsharp
member this.TupledArguments(str, num) = String.replicate num str
```

提示：如果你正在设计用于任何 .NET 语言的库，则不能使用它来实际执行某些试验性 c # 和 Visual Basic 编程，以确保你的库从这些语言 "感觉正确"。 你还可以使用 .NET 反射器和 Visual Studio 对象浏览器等工具来确保库及其文档与开发人员按预期方式显示。

## <a name="appendix"></a>附录

### <a name="end-to-end-example-of-designing-f-code-for-use-by-other-net-languages"></a>设计用于其他 .NET 语言的 F # 代码的端到端示例

请考虑以下类：

```fsharp
open System

type Point1(angle,radius) =
    new() = Point1(angle=0.0, radius=0.0)
    member x.Angle = angle
    member x.Radius = radius
    member x.Stretch(l) = Point1(angle=x.Angle, radius=x.Radius * l)
    member x.Warp(f) = Point1(angle=f(x.Angle), radius=x.Radius)
    static member Circle(n) =
        [ for i in 1..n -> Point1(angle=2.0*Math.PI/float(n), radius=1.0) ]
```

此类的推断 F # 类型如下所示：

```fsharp
type Point1 =
    new : unit -> Point1
    new : angle:double * radius:double -> Point1
    static member Circle : n:int -> Point1 list
    member Stretch : l:double -> Point1
    member Warp : f:(double -> double) -> Point1
    member Angle : double
    member Radius : double
```

让我们看一下如何使用另一种 .NET 语言向程序员显示此 F # 类型。 例如，"签名" 大致如下所示：

```csharp
// C# signature for the unadjusted Point1 class
public class Point1
{
    public Point1();

    public Point1(double angle, double radius);

    public static Microsoft.FSharp.Collections.List<Point1> Circle(int count);

    public Point1 Stretch(double factor);

    public Point1 Warp(Microsoft.FSharp.Core.FastFunc<double,double> transform);

    public double Angle { get; }

    public double Radius { get; }
}
```

有关 F # 表示构造的信息，请注意以下几点。 例如：

* 参数名称等元数据已保留。

* 采用两个参数的 F # 方法成为采用两个自变量的 c # 方法。

* 函数和列表将成为 F # 库中的相应类型的引用。

下面的代码演示如何调整此代码以考虑这些问题。

```fsharp
namespace SuperDuperFSharpLibrary.Types

type RadialPoint(angle:double, radius:double) =

    /// Return a point at the origin
    new() = RadialPoint(angle=0.0, radius=0.0)

    /// The angle to the point, from the x-axis
    member x.Angle = angle

    /// The distance to the point, from the origin
    member x.Radius = radius

    /// Return a new point, with radius multiplied by the given factor
    member x.Stretch(factor) =
        RadialPoint(angle=angle, radius=radius * factor)

    /// Return a new point, with angle transformed by the function
    member x.Warp(transform:Func<_,_>) =
        RadialPoint(angle=transform.Invoke angle, radius=radius)

    /// Return a sequence of points describing an approximate circle using
    /// the given count of points
    static member Circle(count) =
        seq { for i in 1..count ->
                RadialPoint(angle=2.0*Math.PI/float(count), radius=1.0) }
```

推断出的 F # 类型的代码如下所示：

```fsharp
type RadialPoint =
    new : unit -> RadialPoint
    new : angle:double * radius:double -> RadialPoint
    static member Circle : count:int -> seq<RadialPoint>
    member Stretch : factor:double -> RadialPoint
    member Warp : transform:System.Func<double,double> -> RadialPoint
    member Angle : double
    member Radius : double
```

现在，c # 签名如下所示：

```csharp
public class RadialPoint
{
    public RadialPoint();

    public RadialPoint(double angle, double radius);

    public static System.Collections.Generic.IEnumerable<RadialPoint> Circle(int count);

    public RadialPoint Stretch(double factor);

    public RadialPoint Warp(System.Func<double,double> transform);

    public double Angle { get; }

    public double Radius { get; }
}
```

为准备此类型作为 vanilla .NET 库的一部分而进行的修复，如下所示：

* 调整了几个名称： `Point1` 、 `n` 、 `l` 和 `f` 分别为 `RadialPoint` 、 `count` `factor` `transform` 、和。

* 使用的返回类型， `seq<RadialPoint>` 而不是 `RadialPoint list` 通过将使用的列表构造更改 `[ ... ]` 为使用的序列构造 `IEnumerable<RadialPoint>` 。

* 使用 .NET 委托类型， `System.Func` 而不是 F # 函数类型。

这使得在 c # 代码中使用更好。
