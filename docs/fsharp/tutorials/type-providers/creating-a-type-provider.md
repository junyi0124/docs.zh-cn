---
title: 教程：创建类型提供程序
description: '了解如何在 F # 3.0 中创建自己的 F # 类型提供程序，具体方法是检查几个简单的类型提供程序来说明基本概念。'
ms.date: 11/04/2019
ms.openlocfilehash: 65cb9616f66b5850135dbfcdd9b9a9dad30421de
ms.sourcegitcommit: ecd9e9bb2225eb76f819722ea8b24988fe46f34c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/05/2020
ms.locfileid: "96739693"
---
# <a name="tutorial-create-a-type-provider"></a>教程：创建类型提供程序

F # 中的类型提供程序机制是其对信息丰富编程支持的重要组成部分。 本教程介绍了如何创建自己的类型提供程序，逐步讲解如何开发几个简单的类型提供程序来说明基本概念。 有关 F # 中的类型提供程序机制的详细信息，请参阅 [类型提供程序](index.md)。

F # 生态系统包含一系列常用的 Internet 和企业数据服务的类型提供程序。 例如：

- [Fsharp.core](https://fsharp.github.io/FSharp.Data/) 包含 JSON、XML、CSV 和 HTML 文档格式的类型提供程序。

- [SQLProvider](https://fsprojects.github.io/SQLProvider/) 通过对象映射以及针对这些数据源的 F # LINQ 查询，提供对 SQL 数据库的强类型访问。

- [FSharp.Data.SqlClient](https://fsprojects.github.io/FSharp.Data.SqlClient/) 包含一组类型提供程序，用于在 F# 中对 T-SQL 进行编译时检查。

- [Fsharp.core](https://fsprojects.github.io/FSharp.Data.TypeProviders/) 是一组较旧的类型提供程序，仅用于访问 SQL、实体框架、ODATA 和 WSDL 数据服务 .NET Framework 编程。

必要时，可以创建自定义类型提供程序，也可以引用其他人创建的类型提供程序。 例如，你的组织可能有一个数据服务，该服务提供了数量不断增长的命名数据集，每个数据集都有其自己的稳定数据架构。 您可以创建一个类型提供程序，用于读取架构并以强类型方式向程序员显示当前数据集。

## <a name="before-you-start"></a>开始之前

类型提供程序机制主要用于向 F # 编程体验中注入稳定的数据和服务信息空间。

此机制不能用于注入其架构在程序执行过程中发生更改的信息空间，这种方式与程序逻辑相关。 此外，该机制不是针对语言内的元编程而设计的，即使该域包含一些有效的用途。 只有在必要时才应使用此机制，而在何处开发类型提供程序会产生非常高的值。

你应避免在架构不可用的情况下编写类型提供程序。 同样，您应该避免编写一种类型提供程序，在这种情况下，普通 (甚至现有) .NET 库都能满足需要。

在开始之前，您可能会提出以下问题：

- 你的信息源是否有架构？ 如果是这样，什么是 F # 和 .NET 类型系统的映射？

- 能否使用现有 (动态类型化) API 作为实现的起始点？

- 你和你的组织是否有足够的类型提供商的使用来使写入它有价值？ 正常的 .NET 库是否能满足您的需要？

- 你的架构将更改多少？

- 它是否会在编码过程中更改？

- 它是否会在编码会话之间更改？

- 它是否会在程序执行过程中更改？

类型提供程序最适用于架构在运行时和编译代码的生存期内稳定的情况。

## <a name="a-simple-type-provider"></a>简单类型提供程序

此示例为 HelloWorldTypeProvider，类似于 `examples` [F # 类型提供程序 SDK](https://github.com/fsprojects/FSharp.TypeProviders.SDK/)的目录中的示例。 提供程序提供了一个包含100个已擦除类型的 "类型空间"，如以下代码通过使用 F # 签名语法和省略除之外的详细信息中所示 `Type1` 。 有关已擦除的类型的详细信息，请参阅本主题后面的有关已擦除的所 [提供类型的详细](#details-about-erased-provided-types) 信息。

```fsharp
namespace Samples.HelloWorldTypeProvider

type Type1 =
    /// This is a static property.
    static member StaticProperty : string

    /// This constructor takes no arguments.
    new : unit -> Type1

    /// This constructor takes one argument.
    new : data:string -> Type1

    /// This is an instance property.
    member InstanceProperty : int

    /// This is an instance method.
    member InstanceMethod : x:int -> char

    nested type NestedType =
        /// This is StaticProperty1 on NestedType.
        static member StaticProperty1 : string
        …
        /// This is StaticProperty100 on NestedType.
        static member StaticProperty100 : string

type Type2 =
…
…

type Type100 =
…
```

请注意，提供的类型和成员集是静态已知的。 此示例不利用提供程序提供依赖于架构的类型的能力。 下面的代码概述了类型提供程序的实现，详细信息将在本主题的后面部分中介绍。

> [!WARNING]
> 此代码与联机示例之间可能存在差异。

```fsharp
namespace Samples.FSharp.HelloWorldTypeProvider

open System
open System.Reflection
open ProviderImplementation.ProvidedTypes
open FSharp.Core.CompilerServices
open FSharp.Quotations

// This type defines the type provider. When compiled to a DLL, it can be added
// as a reference to an F# command-line compilation, script, or project.
[<TypeProvider>]
type SampleTypeProvider(config: TypeProviderConfig) as this =

  // Inheriting from this type provides implementations of ITypeProvider
  // in terms of the provided types below.
  inherit TypeProviderForNamespaces(config)

  let namespaceName = "Samples.HelloWorldTypeProvider"
  let thisAssembly = Assembly.GetExecutingAssembly()

  // Make one provided type, called TypeN.
  let makeOneProvidedType (n:int) =
  …
  // Now generate 100 types
  let types = [ for i in 1 .. 100 -> makeOneProvidedType i ]

  // And add them to the namespace
  do this.AddNamespace(namespaceName, types)

[<assembly:TypeProviderAssembly>]
do()
```

若要使用此提供程序，请打开一个单独的 Visual Studio 实例，创建一个 F # 脚本，然后使用 #r 从脚本添加对提供程序的引用，如以下代码所示：

```fsharp
#r @".\bin\Debug\Samples.HelloWorldTypeProvider.dll"

let obj1 = Samples.HelloWorldTypeProvider.Type1("some data")

let obj2 = Samples.HelloWorldTypeProvider.Type1("some other data")

obj1.InstanceProperty
obj2.InstanceProperty

[ for index in 0 .. obj1.InstanceProperty-1 -> obj1.InstanceMethod(index) ]
[ for index in 0 .. obj2.InstanceProperty-1 -> obj2.InstanceMethod(index) ]

let data1 = Samples.HelloWorldTypeProvider.Type1.NestedType.StaticProperty35
```

然后，在该 `Samples.HelloWorldTypeProvider` 类型提供程序生成的命名空间下查找类型。

重新编译提供程序之前，请确保已关闭使用提供程序 DLL 的所有 Visual Studio 和 F# 交互窗口的实例。 否则，将发生生成错误，因为输出 DLL 将被锁定。

若要使用 print 语句调试此访问接口，请生成一个脚本，用于公开提供程序的问题，然后使用以下代码：

```console
fsc.exe -r:bin\Debug\HelloWorldTypeProvider.dll script.fsx
```

若要使用 Visual Studio 调试此提供程序，请使用管理凭据打开适用于 Visual Studio 的开发人员命令提示，并运行以下命令：

```console
devenv.exe /debugexe fsc.exe -r:bin\Debug\HelloWorldTypeProvider.dll script.fsx
```

作为替代方法，请打开 Visual Studio，打开 "调试" 菜单，选择 `Debug/Attach to process…` 并附加到 `devenv` 正在编辑脚本的其他进程。 通过使用此方法，你可以更轻松地以类型提供程序中的特定逻辑为目标，方法是使用完整的 IntelliSense 和) 的其他功能以交互方式将表达式键入到第二个实例 (。

可以禁用仅我的代码调试，以便更好地识别生成的代码中的错误。 有关如何启用或禁用此功能的信息，请参阅 [使用调试器在代码中导航](/visualstudio/debugger/navigating-through-code-with-the-debugger)。 此外，还可以通过打开 `Debug` 菜单，然后选择 `Exceptions` "Ctrl + Alt + E 键" 打开对话框，来设置第一次异常捕获 `Exceptions` 。 在该对话框的 "" 下 `Common Language Runtime Exceptions` ，选中相应的 `Thrown` 复选框。

### <a name="implementation-of-the-type-provider"></a>类型提供程序的实现

本部分将指导你完成类型提供程序实现的主要部分。 首先，为自定义类型提供程序本身定义类型：

```fsharp
[<TypeProvider>]
type SampleTypeProvider(config: TypeProviderConfig) as this =
```

此类型必须是公共的，并且必须使用 [TypeProvider](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-core-compilerservices-typeproviderattribute.html) 属性进行标记，以便编译器在单独的 F # 项目引用包含该类型的程序集时识别该类型提供程序。 *Config* 参数是可选的，如果存在，则包含 F # 编译器创建的类型提供程序实例的上下文配置信息。

接下来，实现 [ITypeProvider](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-core-compilerservices-itypeprovider.html) 接口。 在这种情况下，将 `TypeProviderForNamespaces` API 中的类型用作 `ProvidedTypes` 基类型。 此帮助器类型可以提供积极提供的命名空间的有限集合，其中每个命名空间都直接包含有限数量的固定、积极提供的类型。 在此上下文中，提供程序 *积极* 将生成类型，即使它们不需要或不使用也是如此。

```fsharp
inherit TypeProviderForNamespaces(config)
```

接下来，定义为提供的类型指定命名空间的本地私有值，并查找类型提供程序程序集本身。 此程序集稍后用作所提供的已擦除类型的逻辑父类型。

```fsharp
let namespaceName = "Samples.HelloWorldTypeProvider"
let thisAssembly = Assembly.GetExecutingAssembly()
```

接下来，创建一个函数以提供每个类型 Type1 .。。Type100. 本主题稍后将对此函数进行更详细的介绍。

```fsharp
let makeOneProvidedType (n:int) = …
```

接下来，生成100提供的类型：

```fsharp
let types = [ for i in 1 .. 100 -> makeOneProvidedType i ]
```

接下来，将类型添加为提供的命名空间：

```fsharp
do this.AddNamespace(namespaceName, types)
```

最后，添加一个程序集特性，该特性指示你正在创建类型提供程序 DLL：

```fsharp
[<assembly:TypeProviderAssembly>]
do()
```

### <a name="providing-one-type-and-its-members"></a>提供一种类型及其成员

`makeOneProvidedType`函数执行提供一种类型的实际工作。

```fsharp
let makeOneProvidedType (n:int) =
…
```

此步骤说明了此函数的实现。 首先，在 n = 57) 时，创建提供的类型 (例如，Type1、n = 1 或 Type57。

```fsharp
// This is the provided type. It is an erased provided type and, in compiled code,
// will appear as type 'obj'.
let t = ProvidedTypeDefinition(thisAssembly, namespaceName,
                               "Type" + string n,
                               baseType = Some typeof<obj>)
```

您应注意以下几点：

- 此提供的类型会被清除。  由于您指示基类型为，因此 `obj` ，实例将在编译的代码中显示为类型为 [obj](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-core-obj.html) 的值。

- 指定非嵌套类型时，必须指定程序集和命名空间。 对于已擦除的类型，程序集应为类型提供程序程序集本身。

接下来，将 XML 文档添加到该类型。 此文档被延迟，即，如果主机编译器需要，则按需计算。

```fsharp
t.AddXmlDocDelayed (fun () -> $"""This provided type {"Type" + string n}""")
```

接下来，将提供的静态属性添加到类型中：

```fsharp
let staticProp = ProvidedProperty(propertyName = "StaticProperty",
                                  propertyType = typeof<string>,
                                  isStatic = true,
                                  getterCode = (fun args -> <@@ "Hello!" @@>))
```

获取此属性的计算结果将始终为字符串 "Hello！"。 属性的可 `GetterCode` 使用 F # 引号，表示宿主编译器为获取属性而生成的代码。 有关报价的详细信息，请参阅 [代码引用 (F # ) ](../../language-reference/code-quotations.md)。

将 XML 文档添加到属性。

```fsharp
staticProp.AddXmlDocDelayed(fun () -> "This is a static property")
```

现在，将提供的属性附加到提供的类型。 您必须将提供的成员附加到一个且仅有一个类型。 否则，该成员将永远无法访问。

```fsharp
t.AddMember staticProp
```

现在，创建一个不采用任何参数的提供的构造函数。

```fsharp
let ctor = ProvidedConstructor(parameters = [ ],
                               invokeCode = (fun args -> <@@ "The object data" :> obj @@>))
```

`InvokeCode`构造函数的返回一个 F # 引号，该引号表示在调用构造函数时宿主编译器生成的代码。 例如，可以使用以下构造函数：

```fsharp
new Type10()
```

将使用基础数据 "对象数据" 创建所提供类型的实例。 带引号的代码包含到 [obj](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-core-obj.html) 的转换，因为该类型是将所提供的类型的擦除 (你在声明提供的类型时指定的) 。

向构造函数添加 XML 文档，并将提供的构造函数添加到提供的类型中：

```fsharp
ctor.AddXmlDocDelayed(fun () -> "This is a constructor")

t.AddMember ctor
```

创建另一个采用一个参数的提供的构造函数：

```fsharp
let ctor2 =
ProvidedConstructor(parameters = [ ProvidedParameter("data",typeof<string>) ],
                    invokeCode = (fun args -> <@@ (%%(args.[0]) : string) :> obj @@>))
```

`InvokeCode`构造函数的再次返回 F # 引号，表示宿主编译器为对方法的调用而生成的代码。 例如，可以使用以下构造函数：

```fsharp
new Type10("ten")
```

使用基础数据 "10" 创建所提供类型的实例。 您可能已注意到该 `InvokeCode` 函数返回了一个引号。 此函数的输入为表达式列表，每个构造函数参数一个。 在这种情况下，可在中找到表示单个参数值的表达式 `args.[0]` 。 对构造函数的调用的代码会将返回值强制转换为已清除的类型 `obj` 。 将第二个提供的构造函数添加到类型后，将创建一个提供的实例属性：

```fsharp
let instanceProp =
    ProvidedProperty(propertyName = "InstanceProperty",
                     propertyType = typeof<int>,
                     getterCode= (fun args ->
                        <@@ ((%%(args.[0]) : obj) :?> string).Length @@>))
instanceProp.AddXmlDocDelayed(fun () -> "This is an instance property")
t.AddMember instanceProp
```

获取此属性将返回字符串的长度，即表示形式对象。 `GetterCode`属性返回一个 F # 引号，该引号指定宿主编译器生成以获取属性的代码。 与一样 `InvokeCode` ，该 `GetterCode` 函数返回一个引号。 宿主编译器使用参数列表调用此函数。 在这种情况下，参数只包含表示正在调用 getter 的实例的单个表达式，你可以使用访问该实例 `args.[0]` 。 `GetterCode`然后，将接合的实现转换为已删除类型的结果引用 `obj` ，并使用强制转换来满足编译器用于检查对象是否为字符串的类型的机制。 下一部分提供了 `makeOneProvidedType` 包含一个参数的实例方法。

```fsharp
let instanceMeth =
    ProvidedMethod(methodName = "InstanceMethod",
                   parameters = [ProvidedParameter("x",typeof<int>)],
                   returnType = typeof<char>,
                   invokeCode = (fun args ->
                       <@@ ((%%(args.[0]) : obj) :?> string).Chars(%%(args.[1]) : int) @@>))

instanceMeth.AddXmlDocDelayed(fun () -> "This is an instance method")
// Add the instance method to the type.
t.AddMember instanceMeth
```

最后，创建包含100嵌套属性的嵌套类型。 此嵌套类型和其属性的创建延迟，即按需计算。

```fsharp
t.AddMembersDelayed(fun () ->
  let nestedType = ProvidedTypeDefinition("NestedType", Some typeof<obj>)

  nestedType.AddMembersDelayed (fun () ->
    let staticPropsInNestedType =
      [
          for i in 1 .. 100 ->
              let valueOfTheProperty = "I am string "  + string i

              let p =
                ProvidedProperty(propertyName = "StaticProperty" + string i,
                  propertyType = typeof<string>,
                  isStatic = true,
                  getterCode= (fun args -> <@@ valueOfTheProperty @@>))

              p.AddXmlDocDelayed(fun () ->
                  $"This is StaticProperty{i} on NestedType")

              p
      ]

    staticPropsInNestedType)

  [nestedType])
```

### <a name="details-about-erased-provided-types"></a>已清除的提供的类型的详细信息

本部分中的示例仅提供已 *清除的提供的类型*，这些类型在下列情况下特别有用：

- 当你为仅包含数据和方法的信息空间编写提供程序时。

- 编写准确的运行时类型语义对于实际使用信息空间不重要的访问接口。

- 当你为信息空间编写提供程序时，这些信息空间非常大且相互互连，因此无法为信息空间生成真实的 .NET 类型。

在此示例中，每个提供的类型都将被清除为类型 `obj` ，并且该类型的所有使用将 `obj` 在编译的代码中显示为类型。 事实上，这些示例中的基础对象是字符串，但该类型在 `System.Object` .net 编译的代码中显示为。 与类型擦除的所有使用一样，可以使用显式装箱、取消装箱和强制转换为破坏的已擦除类型。 在这种情况下，当使用对象时，可能会导致无效的强制转换异常。 提供程序运行时可以定义自己的专用表示类型，以帮助防止出现 false 表示形式。 不能在 F # 本身中定义已删除的类型。 仅可删除所提供的类型。 您必须了解使用类型提供程序的已擦除类型或提供擦除的类型的提供程序的各种后果，这两者都是不切实际的。 擦除的类型没有真正的 .NET 类型。 因此，不能对该类型执行准确的反射，如果使用运行时强制转换和依赖于确切运行时类型语义的其他技术，则可能破坏已擦除的类型。 在运行时，通常会导致类型强制转换异常。

### <a name="choosing-representations-for-erased-provided-types"></a>为已清除的提供的类型选择表示形式

对于某些使用已擦除的提供类型，不需要表示形式。 例如，已清除的提供的类型可能只包含静态属性和成员，不包含任何构造函数，并且没有方法或属性返回类型的实例。 如果可以访问已清除的提供的类型的实例，则必须考虑以下问题：

**提供的类型的擦除内容是什么？**

- 所提供类型的擦除是类型在已编译的 .NET 代码中的显示方式。

- 所提供的已擦除类类型的擦除始终是该类型的继承链中的第一个非擦除基类型。

- 所提供的已擦除接口类型的擦除始终为 `System.Object` 。

**提供的类型的表示形式有哪些？**

- 已清除的提供的类型的可能的对象集称为其表示形式。 在本文档的示例中，所有已清除的提供的类型的表示形式 `Type1..Type100` 始终为字符串对象。

提供的类型的所有表示形式都必须与提供的类型的擦除兼容。  (否则，F # 编译器将在使用类型提供程序时提供错误，否则将生成无效的 .NET 代码。 如果类型提供程序返回的代码给出无效的表示形式，则该类型提供程序无效。）

您可以使用下列方法之一为所提供的对象选择表示形式，这两种方法都非常常见：

- 如果你只是在现有 .NET 类型上提供强类型包装，则你的类型可能会擦除到该类型，将该类型的实例用作表示形式，或同时使用这两种类型的实例。 使用强类型版本时，此方法适用于该类型的大多数现有方法。

- 如果要创建与任何现有 .NET API 明显不同的 API，请创建运行时类型，这些运行时类型将为所提供的类型的擦除和表示形式。

本文档中的示例使用字符串作为提供的对象的表示形式。 通常，将其他对象用于表示形式可能会非常合适。 例如，可以使用字典作为属性包：

```fsharp
ProvidedConstructor(parameters = [],
    invokeCode= (fun args -> <@@ (new Dictionary<string,obj>()) :> obj @@>))
```

作为替代方法，你可以在类型提供程序中定义一个类型，该类型将在运行时用于形成表示形式，以及一个或多个运行时操作：

```fsharp
type DataObject() =
    let data = Dictionary<string,obj>()
    member x.RuntimeOperation() = data.Count
```

然后，提供的成员可以构造该对象类型的实例：

```fsharp
ProvidedConstructor(parameters = [],
    invokeCode= (fun args -> <@@ (new DataObject()) :> obj @@>))
```

在这种情况下，你可以通过在构造时将此类型指定为， (（可选）) 将此类型用作类型擦除 `baseType` `ProvidedTypeDefinition` ：

```fsharp
ProvidedTypeDefinition(…, baseType = Some typeof<DataObject> )
…
ProvidedConstructor(…, InvokeCode = (fun args -> <@@ new DataObject() @@>), …)
```

### <a name="key-lessons"></a>关键课程

上一部分介绍了如何创建简单的擦除类型提供程序，该提供程序提供一系列类型、属性和方法。 本部分还介绍了类型擦除的概念，包括提供类型提供程序中的已删除类型的一些优点和缺点，并讨论了已擦除的类型的表示形式。

## <a name="a-type-provider-that-uses-static-parameters"></a>使用静态参数的类型提供程序

即使在访问接口不需要访问任何本地或远程数据的情况下，通过静态数据参数化类型提供程序的能力也能实现很多有趣的方案。 在本部分中，你将了解将此类提供程序组合在一起的一些基本方法。

### <a name="type-checked-regex-provider"></a>类型检查 Regex 提供程序

假设要实现用于 <xref:System.Text.RegularExpressions.Regex> 在提供以下编译时保证的接口中包装 .net 库的正则表达式的类型提供程序：

- 正在验证正则表达式是否有效。

- 提供基于正则表达式中的任何组名称的匹配项的命名属性。

本部分说明如何使用类型提供程序创建 `RegexTyped` 正则表达式模式参数化的类型以提供这些优点。 如果提供的模式无效，则编译器将报告错误，类型提供程序可以从模式中提取组，以便可以通过使用匹配的命名属性来访问这些组。 设计类型提供程序时，应考虑其公开的 API 应如何向最终用户显示，以及此设计如何转换为 .NET 代码。 下面的示例演示如何使用此类 API 获取区域代码的组件：

```fsharp
type T = RegexTyped< @"(?<AreaCode>^\d{3})-(?<PhoneNumber>\d{3}-\d{4}$)">
let reg = T()
let result = T.IsMatch("425-555-2345")
let r = reg.Match("425-555-2345").Group_AreaCode.Value //r equals "425"
```

下面的示例演示类型提供程序如何转换这些调用：

```fsharp
let reg = new Regex(@"(?<AreaCode>^\d{3})-(?<PhoneNumber>\d{3}-\d{4}$)")
let result = reg.IsMatch("425-123-2345")
let r = reg.Match("425-123-2345").Groups.["AreaCode"].Value //r equals "425"
```

请注意以下几点：

- 标准 Regex 类型表示参数化 `RegexTyped` 类型。

- `RegexTyped`构造函数导致调用 Regex 构造函数，并传入模式的静态类型参数。

- 方法的结果 `Match` 以标准 <xref:System.Text.RegularExpressions.Match> 类型表示。

- 每个命名组都生成提供的属性，而访问属性会导致在匹配的集合上使用索引器 `Groups` 。

下面的代码是实现此类提供程序的逻辑的核心，此示例省略了向所提供类型添加所有成员的情况。 有关每个已添加成员的信息，请参阅本主题后面的相应部分。 有关完整的代码，请从 CodePlex 网站上的 [F # 3.0 示例包](https://archive.codeplex.com/?p=fsharp3sample) 下载该示例。

```fsharp
namespace Samples.FSharp.RegexTypeProvider

open System.Reflection
open Microsoft.FSharp.Core.CompilerServices
open Samples.FSharp.ProvidedTypes
open System.Text.RegularExpressions

[<TypeProvider>]
type public CheckedRegexProvider() as this =
    inherit TypeProviderForNamespaces()

    // Get the assembly and namespace used to house the provided types
    let thisAssembly = Assembly.GetExecutingAssembly()
    let rootNamespace = "Samples.FSharp.RegexTypeProvider"
    let baseTy = typeof<obj>
    let staticParams = [ProvidedStaticParameter("pattern", typeof<string>)]

    let regexTy = ProvidedTypeDefinition(thisAssembly, rootNamespace, "RegexTyped", Some baseTy)

    do regexTy.DefineStaticParameters(
        parameters=staticParams,
        instantiationFunction=(fun typeName parameterValues ->

          match parameterValues with
          | [| :? string as pattern|] ->

            // Create an instance of the regular expression.
            //
            // This will fail with System.ArgumentException if the regular expression is not valid.
            // The exception will escape the type provider and be reported in client code.
            let r = System.Text.RegularExpressions.Regex(pattern)

            // Declare the typed regex provided type.
            // The type erasure of this type is 'obj', even though the representation will always be a Regex
            // This, combined with hiding the object methods, makes the IntelliSense experience simpler.
            let ty =
              ProvidedTypeDefinition(
                thisAssembly,
                rootNamespace,
                typeName,
                baseType = Some baseTy)

            ...

            ty
          | _ -> failwith "unexpected parameter values"))

    do this.AddNamespace(rootNamespace, [regexTy])

[<TypeProviderAssembly>]
do ()
```

请注意以下几点：

- 该类型提供程序采用两个静态参数： `pattern` 是必需的，而是 `options` 可选 (因为) 提供默认值。

- 提供静态参数后，可以创建正则表达式的实例。 如果正则表达式格式不正确，此实例将引发异常，并向用户报告此错误。

- 在 `DefineStaticParameters` 回调中，可以定义在提供参数后将返回的类型。

- 此代码会将设置 `HideObjectMethods` 为 true，以使 IntelliSense 体验保持简单。 此属性将导致 `Equals` `GetHashCode` `Finalize` `GetType` 从提供的对象的 IntelliSense 列表中抑制、、和成员。

- 使用 `obj` 作为方法的基类型，但会将 `Regex` 对象用作此类型的运行时表示形式，如下面的示例所示。

- `Regex` <xref:System.ArgumentException> 当正则表达式无效时，对构造函数的调用将引发。 编译器将捕获此异常并向用户报告编译时或 Visual Studio 编辑器中的错误消息。 此异常允许验证正则表达式，而无需运行应用程序。

上面定义的类型不太有用，因为它不包含任何有意义的方法或属性。 首先，添加一个静态 `IsMatch` 方法：

```fsharp
let isMatch =
    ProvidedMethod(
        methodName = "IsMatch",
        parameters = [ProvidedParameter("input", typeof<string>)],
        returnType = typeof<bool>,
        isStatic = true,
        invokeCode = fun args -> <@@ Regex.IsMatch(%%args.[0], pattern) @@>)

isMatch.AddXmlDoc "Indicates whether the regular expression finds a match in the specified input string."
ty.AddMember isMatch
```

前面的代码定义了一个方法 `IsMatch` ，该方法采用字符串作为输入并返回 `bool` 。 唯一难用的部分是 `args` 定义中的参数 `InvokeCode` 。 在此示例中， `args` 是表示此方法的参数的引号列表。 如果该方法是实例方法，则第一个参数表示 `this` 参数。 但对于静态方法，参数只是方法的显式参数。 请注意，在这种情况下，带引号的值的类型应与指定的返回类型 (， `bool`) 。 另请注意，此代码使用 `AddXmlDoc` 方法来确保所提供的方法也具有可通过 IntelliSense 提供的有用文档。

接下来，添加实例 Match 方法。 但是，此方法应返回提供的类型的值， `Match` 以便能够以强类型方式访问组。 因此，首先声明 `Match` 类型。 由于此类型依赖于以静态参数形式提供的模式，因此，此类型必须嵌套在参数化类型定义中：

```fsharp
let matchTy =
    ProvidedTypeDefinition(
        "MatchType",
        baseType = Some baseTy,
        hideObjectMethods = true)

ty.AddMember matchTy
```

然后，将一个属性添加到每个组的匹配类型。 在运行时，匹配项表示为 <xref:System.Text.RegularExpressions.Match> 值，因此，定义属性的引用必须使用 <xref:System.Text.RegularExpressions.Match.Groups> 索引属性来获取相关组。

```fsharp
for group in r.GetGroupNames() do
    // Ignore the group named 0, which represents all input.
    if group <> "0" then
    let prop =
      ProvidedProperty(
        propertyName = group,
        propertyType = typeof<Group>,
        getterCode = fun args -> <@@ ((%%args.[0]:obj) :?> Match).Groups.[group] @@>)
        prop.AddXmlDoc($"""Gets the ""{group}"" group from this match""")
    matchTy.AddMember prop
```

同样，请注意，将 XML 文档添加到提供的属性。 另请注意，如果提供了函数，则可以读取属性 `GetterCode` ; 如果提供了函数，则可以编写属性 `SetterCode` ，因此，生成的属性是只读的。

现在，你可以创建返回此类型的值的实例方法 `Match` ：

```fsharp
let matchMethod =
    ProvidedMethod(
        methodName = "Match",
        parameters = [ProvidedParameter("input", typeof<string>)],
        returnType = matchTy,
        invokeCode = fun args -> <@@ ((%%args.[0]:obj) :?> Regex).Match(%%args.[1]) :> obj @@>)

matchMeth.AddXmlDoc "Searches the specified input string for the first occurrence of this regular expression"

ty.AddMember matchMeth
```

由于您正在创建实例方法，因此 `args.[0]` 表示在 `RegexTyped` 其上调用方法的实例， `args.[1]` 是输入参数。

最后，提供一个构造函数，以便可以创建提供的类型的实例。

```fsharp
let ctor =
    ProvidedConstructor(
        parameters = [],
        invokeCode = fun args -> <@@ Regex(pattern, options) :> obj @@>)

ctor.AddXmlDoc("Initializes a regular expression instance.")

ty.AddMember ctor
```

构造函数只需擦除到标准 .NET 正则表达式实例的创建，这会再次装箱到对象，因为它 `obj` 是所提供的类型的擦除。 通过此更改，本主题中前面指定的示例 API 用法按预期方式工作。 以下代码已完成，并且是最终的：

```fsharp
namespace Samples.FSharp.RegexTypeProvider

open System.Reflection
open Microsoft.FSharp.Core.CompilerServices
open Samples.FSharp.ProvidedTypes
open System.Text.RegularExpressions

[<TypeProvider>]
type public CheckedRegexProvider() as this =
    inherit TypeProviderForNamespaces()

    // Get the assembly and namespace used to house the provided types.
    let thisAssembly = Assembly.GetExecutingAssembly()
    let rootNamespace = "Samples.FSharp.RegexTypeProvider"
    let baseTy = typeof<obj>
    let staticParams = [ProvidedStaticParameter("pattern", typeof<string>)]

    let regexTy = ProvidedTypeDefinition(thisAssembly, rootNamespace, "RegexTyped", Some baseTy)

    do regexTy.DefineStaticParameters(
        parameters=staticParams,
        instantiationFunction=(fun typeName parameterValues ->

            match parameterValues with
            | [| :? string as pattern|] ->

                // Create an instance of the regular expression.

                let r = System.Text.RegularExpressions.Regex(pattern)

                // Declare the typed regex provided type.

                let ty =
                    ProvidedTypeDefinition(
                        thisAssembly,
                        rootNamespace,
                        typeName,
                        baseType = Some baseTy)

                ty.AddXmlDoc "A strongly typed interface to the regular expression '%s'"

                // Provide strongly typed version of Regex.IsMatch static method.
                let isMatch =
                    ProvidedMethod(
                        methodName = "IsMatch",
                        parameters = [ProvidedParameter("input", typeof<string>)],
                        returnType = typeof<bool>,
                        isStatic = true,
                        invokeCode = fun args -> <@@ Regex.IsMatch(%%args.[0], pattern) @@>)

                isMatch.AddXmlDoc "Indicates whether the regular expression finds a match in the specified input string"

                ty.AddMember isMatch

                // Provided type for matches
                // Again, erase to obj even though the representation will always be a Match
                let matchTy =
                    ProvidedTypeDefinition(
                        "MatchType",
                        baseType = Some baseTy,
                        hideObjectMethods = true)

                // Nest the match type within parameterized Regex type.
                ty.AddMember matchTy

                // Add group properties to match type
                for group in r.GetGroupNames() do
                    // Ignore the group named 0, which represents all input.
                    if group <> "0" then
                        let prop =
                          ProvidedProperty(
                            propertyName = group,
                            propertyType = typeof<Group>,
                            getterCode = fun args -> <@@ ((%%args.[0]:obj) :?> Match).Groups.[group] @@>)
                        prop.AddXmlDoc(sprintf @"Gets the ""%s"" group from this match" group)
                        matchTy.AddMember(prop)

                // Provide strongly typed version of Regex.Match instance method.
                let matchMeth =
                  ProvidedMethod(
                    methodName = "Match",
                    parameters = [ProvidedParameter("input", typeof<string>)],
                    returnType = matchTy,
                    invokeCode = fun args -> <@@ ((%%args.[0]:obj) :?> Regex).Match(%%args.[1]) :> obj @@>)
                matchMeth.AddXmlDoc "Searches the specified input string for the first occurrence of this regular expression"

                ty.AddMember matchMeth

                // Declare a constructor.
                let ctor =
                  ProvidedConstructor(
                    parameters = [],
                    invokeCode = fun args -> <@@ Regex(pattern) :> obj @@>)

                // Add documentation to the constructor.
                ctor.AddXmlDoc "Initializes a regular expression instance"

                ty.AddMember ctor

                ty
            | _ -> failwith "unexpected parameter values"))

    do this.AddNamespace(rootNamespace, [regexTy])

[<TypeProviderAssembly>]
do ()
```

### <a name="key-lessons"></a>关键课程

本部分介绍如何创建在其静态参数上操作的类型提供程序。 提供程序将检查静态参数，并根据其值提供操作。

## <a name="a-type-provider-that-is-backed-by-local-data"></a>本地数据支持的类型提供程序

通常，可能需要类型提供程序基于静态参数提供 Api，而不是本地或远程系统中的信息。 本部分讨论基于本地数据（如本地数据文件）的类型提供程序。

### <a name="simple-csv-file-provider"></a>简单 CSV 文件提供程序

作为一个简单的示例，请考虑一种类型提供程序，用于以逗号分隔的值（ (CSV) 格式）访问科学数据。 本部分假定 CSV 文件包含标题行，后跟浮点数据，如下表所示：

|计量 (计量) |时间 (秒) |
|----------------|-------------|
|50.0|3.7|
|100.0|5.2|
|150.0|6.4|

本部分说明如何提供可用于获取具有 `Distance` 类型的属性的行 `float<meter>` 和 `Time` 类型的属性的类型 `float<second>` 。 为简单起见，进行了以下假设：

- 标头名称要么不小于单位，要么格式为 "Name (unit) " 且不包含逗号。

- 单位为所有系统国际 (SI) 单元， [ (F # ) ](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-data-unitsystems-si-unitnames.html) 模块定义的 unitnames.ohm 模块。

- 单位为简单 (例如，计量) ，而不是复合 (，例如计量/秒) 。

- 所有列都包含浮点数据。

更完整的提供程序会放宽这些限制。

第一步是考虑 API 的外观。 由于给出了包含上表内容的 `info.csv` 文件（采用逗号分隔格式），提供程序的用户应该能够编写与以下示例类似的代码：

```fsharp
let info = new MiniCsv<"info.csv">()
for row in info.Data do
let time = row.Time
printfn $"{float time}"
```

在这种情况下，编译器应将这些调用转换为类似于以下示例的内容：

```fsharp
let info = new CsvFile("info.csv")
for row in info.Data do
let (time:float) = row.[1]
printfn $"%f{float time}"
```

最佳转换需要类型提供程序 `CsvFile` 在类型提供程序的程序集中定义实类型。 类型提供程序经常依赖几个帮助程序类型和方法来包装重要逻辑。 由于度量值会在运行时被清除，因此可以使用 `float[]` 作为行的已擦除类型。 编译器会将不同的列视为具有不同的度量值类型。 例如，我们的示例中的第一列的类型为，第二列的类型为 `float<meter>` `float<second>` 。 但是，清除的表示形式可能会变得非常简单。

下面的代码显示了实现的核心。

```fsharp
// Simple type wrapping CSV data
type CsvFile(filename) =
    // Cache the sequence of all data lines (all lines but the first)
    let data =
        seq {
            for line in File.ReadAllLines(filename) |> Seq.skip 1 ->
                line.Split(',') |> Array.map float
        }
        |> Seq.cache
    member _.Data = data

[<TypeProvider>]
type public MiniCsvProvider(cfg:TypeProviderConfig) as this =
    inherit TypeProviderForNamespaces(cfg)

    // Get the assembly and namespace used to house the provided types.
    let asm = System.Reflection.Assembly.GetExecutingAssembly()
    let ns = "Samples.FSharp.MiniCsvProvider"

    // Create the main provided type.
    let csvTy = ProvidedTypeDefinition(asm, ns, "MiniCsv", Some(typeof<obj>))

    // Parameterize the type by the file to use as a template.
    let filename = ProvidedStaticParameter("filename", typeof<string>)
    do csvTy.DefineStaticParameters([filename], fun tyName [| :? string as filename |] ->

        // Resolve the filename relative to the resolution folder.
        let resolvedFilename = Path.Combine(cfg.ResolutionFolder, filename)

        // Get the first line from the file.
        let headerLine = File.ReadLines(resolvedFilename) |> Seq.head

        // Define a provided type for each row, erasing to a float[].
        let rowTy = ProvidedTypeDefinition("Row", Some(typeof<float[]>))

        // Extract header names from the file, splitting on commas.
        // use Regex matching to get the position in the row at which the field occurs
        let headers = Regex.Matches(headerLine, "[^,]+")

        // Add one property per CSV field.
        for i in 0 .. headers.Count - 1 do
            let headerText = headers.[i].Value

            // Try to decompose this header into a name and unit.
            let fieldName, fieldTy =
                let m = Regex.Match(headerText, @"(?<field>.+) \((?<unit>.+)\)")
                if m.Success then

                    let unitName = m.Groups.["unit"].Value
                    let units = ProvidedMeasureBuilder.Default.SI unitName
                    m.Groups.["field"].Value, ProvidedMeasureBuilder.Default.AnnotateType(typeof<float>,[units])

                else
                    // no units, just treat it as a normal float
                    headerText, typeof<float>

            let prop =
                ProvidedProperty(fieldName, fieldTy,
                    getterCode = fun [row] -> <@@ (%%row:float[]).[i] @@>)

            // Add metadata that defines the property's location in the referenced file.
            prop.AddDefinitionLocation(1, headers.[i].Index + 1, filename)
            rowTy.AddMember(prop)

        // Define the provided type, erasing to CsvFile.
        let ty = ProvidedTypeDefinition(asm, ns, tyName, Some(typeof<CsvFile>))

        // Add a parameterless constructor that loads the file that was used to define the schema.
        let ctor0 =
            ProvidedConstructor([],
                invokeCode = fun [] -> <@@ CsvFile(resolvedFilename) @@>)
        ty.AddMember ctor0

        // Add a constructor that takes the file name to load.
        let ctor1 = ProvidedConstructor([ProvidedParameter("filename", typeof<string>)],
            invokeCode = fun [filename] -> <@@ CsvFile(%%filename) @@>)
        ty.AddMember ctor1

        // Add a more strongly typed Data property, which uses the existing property at runtime.
        let prop =
            ProvidedProperty("Data", typedefof<seq<_>>.MakeGenericType(rowTy),
                getterCode = fun [csvFile] -> <@@ (%%csvFile:CsvFile).Data @@>)
        ty.AddMember prop

        // Add the row type as a nested type.
        ty.AddMember rowTy
        ty)

    // Add the type to the namespace.
    do this.AddNamespace(ns, [csvTy])
```

请注意以下有关实现的要点：

- 重载的构造函数允许读取原始文件或具有相同架构的文件。 当你为本地或远程数据源编写类型提供程序时，此模式很常见，并且此模式允许将本地文件用作远程数据的模板。

- 您可以使用传入到类型提供程序构造函数的 [TypeProviderConfig](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-core-compilerservices-typeproviderconfig.html) 值来解析相对文件名。

- 您可以使用 `AddDefinitionLocation` 方法来定义所提供属性的位置。 因此，如果在 `Go To Definition` 提供的属性上使用，则将在 Visual Studio 中打开 CSV 文件。

- 您可以使用该 `ProvidedMeasureBuilder` 类型来查找 SI 单位并生成相关 `float<_>` 类型。

### <a name="key-lessons"></a>关键课程

本部分说明了如何使用数据源本身中包含的简单架构为本地数据源创建类型提供程序。

## <a name="going-further"></a>深入探索

以下部分包含有关进一步研究的建议。

### <a name="a-look-at-the-compiled-code-for-erased-types"></a>查看已清除类型的已编译代码

若要让你了解如何使用类型提供程序与发出的代码相对应的情况，请参阅以下函数，方法是使用 `HelloWorldTypeProvider` 本主题前面部分中使用的。

```fsharp
let function1 () =
    let obj1 = Samples.HelloWorldTypeProvider.Type1("some data")
    obj1.InstanceProperty
```

下面是通过使用 ildasm.exe 生成的代码反编译的图像：

```il
.class public abstract auto ansi sealed Module1
extends [mscorlib]System.Object
{
.custom instance void [FSharp.Core]Microsoft.FSharp.Core.CompilationMappingAtt
ribute::.ctor(valuetype [FSharp.Core]Microsoft.FSharp.Core.SourceConstructFlags)
= ( 01 00 07 00 00 00 00 00 )
.method public static int32  function1() cil managed
{
// Code size       24 (0x18)
.maxstack  3
.locals init ([0] object obj1)
IL_0000:  nop
IL_0001:  ldstr      "some data"
IL_0006:  unbox.any  [mscorlib]System.Object
IL_000b:  stloc.0
IL_000c:  ldloc.0
IL_000d:  call       !!0 [FSharp.Core_2]Microsoft.FSharp.Core.LanguagePrimit
ives/IntrinsicFunctions::UnboxGeneric<string>(object)
IL_0012:  callvirt   instance int32 [mscorlib_3]System.String::get_Length()
IL_0017:  ret
} // end of method Module1::function1

} // end of class Module1
```

如示例所示， `Type1` 已清除类型和属性的所有提到 `InstanceProperty` ，只保留对所涉及的运行时类型的操作。

### <a name="design-and-naming-conventions-for-type-providers"></a>类型提供程序的设计和命名约定

在创作类型提供程序时，请遵循以下约定。

**连接协议的提供程序** 通常，数据和服务连接协议的大多数提供程序 Dll 的名称（如 OData 或 SQL 连接）应以或结尾 `TypeProvider` `TypeProviders` 。 例如，使用类似于以下字符串的 DLL 名称：

`Fabrikam.Management.BasicTypeProviders.dll`

确保你提供的类型为相应命名空间的成员，并指示你实现的连接协议：

```fsharp
  Fabrikam.Management.BasicTypeProviders.WmiConnection<…>
  Fabrikam.Management.BasicTypeProviders.DataProtocolConnection<…>
```

**用于常规编码的实用工具提供程序**。  对于适用于正则表达式的实用工具类型提供程序，类型提供程序可能是基库的一部分，如以下示例所示：

```fsharp
#r "Fabrikam.Core.Text.Utilities.dll"
```

在这种情况下，提供的类型会根据正常的 .NET 设计约定显示在适当的点上：

```fsharp
  open Fabrikam.Core.Text.RegexTyped

  let regex = new RegexTyped<"a+b+a+b+">()
```

**单独数据源**。 某些类型提供程序连接到单个专用数据源并只提供数据。 在这种情况下，应删除此 `TypeProvider` 后缀并使用标准的 .net 命名约定：

```fsharp
#r "Fabrikam.Data.Freebase.dll"

let data = Fabrikam.Data.Freebase.Astronomy.Asteroids
```

有关详细信息，请参阅 `GetConnection` 本主题后面部分介绍的设计约定。

### <a name="design-patterns-for-type-providers"></a>类型提供程序的设计模式

以下各节介绍了在创作类型提供程序时可以使用的设计模式。

#### <a name="the-getconnection-design-pattern"></a>GetConnection 设计模式

大多数类型提供程序应编写为使用 `GetConnection` FSharp.Data.TypeProviders.dll 中的类型提供程序使用的模式，如下面的示例所示：

```fsharp
#r "Fabrikam.Data.WebDataStore.dll"

type Service = Fabrikam.Data.WebDataStore<…static connection parameters…>

let connection = Service.GetConnection(…dynamic connection parameters…)

let data = connection.Astronomy.Asteroids
```

#### <a name="type-providers-backed-by-remote-data-and-services"></a>由远程数据和服务支持的类型提供程序

在创建由远程数据和服务支持的类型提供程序之前，必须考虑连接编程中固有的一系列问题。 这些问题包括以下注意事项：

- 架构映射

- 发生架构更改时的活动和无效

- 架构缓存

- 数据访问操作的异步实现

- 支持查询，包括 LINQ 查询

- 凭据和身份验证

本主题不会进一步探讨这些问题。

### <a name="additional-authoring-techniques"></a>其他创作技术

编写自己的类型提供程序时，可能需要使用以下其他技术。

### <a name="creating-types-and-members-on-demand"></a>按需创建类型和成员

ProvidedType API 具有延迟版本的 AddMember。

```fsharp
  type ProvidedType =
      member AddMemberDelayed  : (unit -> MemberInfo)      -> unit
      member AddMembersDelayed : (unit -> MemberInfo list) -> unit
```

这些版本用于创建类型的按需空间。

### <a name="providing-array-types-and-generic-type-instantiations"></a>提供数组类型和泛型类型实例化

通过在任何实例上使用 normal、和，使提供的成员 (其签名包括数组类型、byref 类型和泛型类型的实例化) `MakeArrayType` `MakePointerType` `MakeGenericType` <xref:System.Type> ，包括 `ProvidedTypeDefinitions` 。

> [!NOTE]
> 在某些情况下，您可能必须使用中的帮助器 `ProvidedTypeBuilder.MakeGenericType` 。  有关更多详细信息，请参阅 [类型提供程序 SDK 文档](https://github.com/fsprojects/FSharp.TypeProviders.SDK/blob/master/README.md#explicit-construction-of-code-makegenerictype-makegenericmethod-and-uncheckedquotations) 。

### <a name="providing-unit-of-measure-annotations"></a>提供度量单位批注

ProvidedTypes API 提供了提供度量值注释的帮助器。 例如，若要提供类型 `float<kg>` ，请使用以下代码：

```fsharp
  let measures = ProvidedMeasureBuilder.Default
  let kg = measures.SI "kilogram"
  let m = measures.SI "meter"
  let float_kg = measures.AnnotateType(typeof<float>,[kg])
```

  若要提供类型 `Nullable<decimal<kg/m^2>>` ，请使用以下代码：

```fsharp
  let kgpm2 = measures.Ratio(kg, measures.Square m)
  let dkgpm2 = measures.AnnotateType(typeof<decimal>,[kgpm2])
  let nullableDecimal_kgpm2 = typedefof<System.Nullable<_>>.MakeGenericType [|dkgpm2 |]
```

### <a name="accessing-project-local-or-script-local-resources"></a>访问 Project-Local 或 Script-Local 资源

在构造过程中，可以为类型提供程序的每个实例指定一个 `TypeProviderConfig` 值。 此值包含提供程序的 "解析文件夹" (即，编译的项目文件夹或包含脚本) 的目录、所引用程序集的列表和其他信息。

### <a name="invalidation"></a>失效

提供程序可以引发无效信号，以通知 F # 语言服务，架构假设可能已发生更改。 当发生失效时，如果提供程序承载于 Visual Studio 中，将重做 typecheck。 当提供程序托管在 F# 交互窗口或 F # 编译器 ( # A0) 中时，将忽略此信号。

### <a name="caching-schema-information"></a>缓存架构信息

提供程序通常必须缓存对架构信息的访问。 应使用指定为静态参数或用户数据的文件名来存储缓存的数据。 架构缓存的一个示例是 `LocalSchemaFile` 程序集的类型提供程序中的 `FSharp.Data.TypeProviders` 参数。 在这些提供程序的实现中，此静态参数指示类型提供程序使用指定的本地文件中的架构信息，而不是通过网络访问数据源。 若要使用缓存的架构信息，还必须将静态参数设置 `ForceUpdate` 为 `false` 。 您可以使用类似的技术来启用联机和脱机数据访问。

### <a name="backing-assembly"></a>支持程序集

在编译 `.dll` 或文件时 `.exe` ，生成的类型的支持 .dll 文件将静态链接到生成的程序集中。 通过将中间语言 (IL) 类型定义和支持程序集中的任何托管资源复制到最终程序集来创建此链接。 使用 F# 交互窗口时，不会复制后备 .dll 文件，而是直接加载到 F# 交互窗口进程。

### <a name="exceptions-and-diagnostics-from-type-providers"></a>类型提供程序中的异常和诊断

提供的类型中所有成员的所有使用可能会引发异常。 在所有情况下，如果类型提供程序引发异常，则宿主编译器会将错误的属性指定给特定的类型提供程序。

- 类型提供程序异常决不会导致内部编译器错误。

- 类型提供程序无法报告警告。

- 在 F # 编译器、F # 开发环境或 F# 交互窗口中承载类型提供程序时，将捕获该提供程序的所有异常。 Message 属性始终是错误文本，并且不会显示任何堆栈跟踪。 如果要引发异常，可以引发以下示例： `System.NotSupportedException` 、 `System.IO.IOException` 、 `System.Exception` 。

#### <a name="providing-generated-types"></a>提供生成的类型

到目前为止，本文档介绍了如何提供擦除的类型。 你还可以使用 F # 中的类型提供程序机制来提供生成的类型，这些类型将作为真实 .NET 类型定义添加到用户的程序中。 您必须使用类型定义引用生成的提供类型。

```fsharp
open Microsoft.FSharp.TypeProviders

type Service = ODataService<"http://services.odata.org/Northwind/Northwind.svc/">
```

F # 3.0 发行版中的 ProvidedTypes-0.2 helper 代码仅对提供生成的类型提供有限支持。 对于生成的类型定义，以下语句必须为 true：

- `isErased` 必须设置为 `false`。

- 必须将生成的类型添加到新构造的 `ProvidedAssembly()` ，它表示生成的代码片段的容器。

- 提供程序必须有一个程序集，该程序集具有包含在磁盘上的匹配 .dll 文件的实际后备 .NET .dll 文件。

## <a name="rules-and-limitations"></a>规则和限制

编写类型提供程序时，请记住以下规则和限制。

### <a name="provided-types-must-be-reachable"></a>提供的类型必须是可访问的

应可从非嵌套类型访问所有提供的类型。 非嵌套类型是在对 `TypeProviderForNamespaces` 构造函数的调用或对的调用中提供的 `AddNamespace` 。 例如，如果提供程序提供类型 `StaticClass.P : T` ，则必须确保 T 为非嵌套类型或嵌套在一个类型下。

例如，某些提供程序有一个 `DataTypes` 包含这些类型的静态类，例如 `T1, T2, T3, ...` 。 否则，此错误表明找到了对程序集 A 中的类型 T 的引用，但无法在该程序集中找到该类型。 如果出现此错误，请验证是否可以从提供程序类型中访问所有子类型。 注意：这些 `T1, T2, T3...` 类型称为 " *即时* 类型"。 请记住将其放在可访问的命名空间或父类型中。

### <a name="limitations-of-the-type-provider-mechanism"></a>类型提供程序机制的限制

F # 中的类型提供程序机制具有以下限制：

- F # 中的类型提供程序的基础结构不支持提供的泛型类型或提供的泛型方法。

- 该机制不支持具有静态参数的嵌套类型。

## <a name="development-tips"></a>开发提示

在开发过程中，你可能会发现以下提示很有用：

### <a name="run-two-instances-of-visual-studio"></a>运行 Visual Studio 的两个实例

您可以在一个实例中开发类型提供程序，并在另一个实例中测试该提供程序，因为测试 IDE 将对 .dll 文件执行锁定，从而阻止重新生成类型提供程序。 因此，在第一个实例中生成提供程序时，必须关闭 Visual Studio 的第二个实例，然后必须在生成提供程序之后重新打开第二个实例。

### <a name="debug-type-providers-by-using-invocations-of-fscexe"></a>使用 fsc.exe 的调用调试类型提供程序

您可以使用以下工具调用类型提供程序：

- F # 命令行编译器 (fsc.exe) 

- F# 交互窗口编译器 (fsi.exe) 

- devenv.exe (Visual Studio) 

通常可以通过对测试脚本文件使用 fsc.exe 来最轻松地调试类型提供程序 (例如 .fsx) 。 你可以从命令提示符处启动调试器。

```console
devenv /debugexe fsc.exe script.fsx
```

  您可以使用打印到 stdout 的日志记录。

## <a name="see-also"></a>请参阅

- [类型提供程序](index.md)
- [类型提供程序 SDK](https://github.com/fsprojects/FSharp.TypeProviders.SDK)
