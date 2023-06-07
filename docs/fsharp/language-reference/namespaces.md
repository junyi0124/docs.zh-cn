---
title: 命名空间
description: '了解 F # 命名空间如何允许用户将名称附加到一组程序元素，从而将代码组织到相关功能的各个区域。'
ms.date: 12/08/2018
ms.openlocfilehash: bf71843349434a1ea91c58dbc0477373dbb0c449
ms.sourcegitcommit: de7f589de07a9979b6ac28f54c3e534a617d9425
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/05/2020
ms.locfileid: "82796127"
---
# <a name="namespaces"></a>命名空间

命名空间使你能够将一个名称附加到一组 F # 程序元素，从而将代码组织到相关功能的区域中。 命名空间通常是 F # 文件中的顶级元素。

## <a name="syntax"></a>语法

```fsharp
namespace [rec] [parent-namespaces.]identifier
```

## <a name="remarks"></a>备注

如果要将代码放入命名空间中，则文件中的第一个声明必须声明命名空间。 如果文件中不存在其他命名空间声明，则整个文件的内容将成为命名空间的一部分。 如果是这种情况，则所有代码直到下一个命名空间声明都被视为位于第一个命名空间内。

命名空间不能直接包含值和函数。 相反，值和函数必须包含在模块中，且模块包含在命名空间中。 命名空间可以包含类型和模块。

XML 文档注释可以在命名空间之上声明，但会被忽略。 还可以在命名空间之上声明编译器指令。

可以使用 namespace 关键字显式声明命名空间，或在声明模块时隐式声明命名空间。 若要显式声明命名空间，请使用 namespace 关键字，后跟命名空间名称。 下面的示例演示一个代码文件，该代码文件`Widgets`声明具有类型的命名空间和包含在该命名空间中的模块。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet6406.fs)]

如果该文件的所有内容都在一个模块中，则还可以使用`module`关键字隐式声明命名空间，并在完全限定的模块名称中提供新的命名空间名称。 下面的示例演示一个声明命名空间`Widgets`的代码文件和一个包含`WidgetsModule`函数的模块。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet6401.fs)]

下面的代码等效于前面的代码，但模块为本地模块声明。 在这种情况下，命名空间必须出现在其自己的行上。

[!code-fsharp[Main](~/samples/snippets/fsharp/namespaces/snippet6402.fs)]

如果一个或多个命名空间中的同一文件中需要多个模块，则必须使用本地模块声明。 使用本地模块声明时，不能在模块声明中使用限定的命名空间。 下面的代码演示一个具有命名空间声明和两个本地模块声明的文件。 在这种情况下，模块直接包含在命名空间中;没有隐式创建的与该文件同名的模块。 文件中的任何其他代码（如`do`绑定）都位于命名空间中，但不在内部模块中，因此需要使用模块名称来限定模块`widgetFunction`成员。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet6403.fs)]

此示例的输出如下所示。

```fsharp
Module1 10 20
Module2 5 6
```

有关详细信息，请参阅[模块](modules.md)。

## <a name="nested-namespaces"></a>嵌套命名空间

创建嵌套命名空间时，必须完全限定该命名空间。 否则，将创建一个新的顶级命名空间。 命名空间声明中将忽略缩进。

下面的示例演示如何声明嵌套命名空间。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet6404.fs)]

## <a name="namespaces-in-files-and-assemblies"></a>文件和程序集中的命名空间

命名空间可跨单个项目或编译中的多个文件。 术语 "*命名空间片段*" 描述包含在一个文件中的命名空间的一部分。 命名空间也可以跨多个程序集。 例如， `System`命名空间包含整个 .NET Framework，这跨多个程序集，并且包含许多嵌套命名空间。

## <a name="global-namespace"></a>全局命名空间

使用预定义的命名`global`空间将名称放在 .net 顶级命名空间中。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet6407.fs)]

你还可以使用 global 引用顶层 .NET 命名空间，例如，解析与其他命名空间的名称冲突。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet6408.fs)]

## <a name="recursive-namespaces"></a>递归命名空间

还可以将命名空间声明为 recursive，以允许所有包含的代码相互递归。  这是通过`namespace rec`实现的。 使用`namespace rec`可以减少无法在类型和模块之间编写相互引用代码的一些难题。 下面是一个示例：

```fsharp
namespace rec MutualReferences

type Orientation = Up | Down
type PeelState = Peeled | Unpeeled

// This exception depends on the type below.
exception DontSqueezeTheBananaException of Banana

type Banana(orientation : Orientation) =
    member val IsPeeled = false with get, set
    member val Orientation = orientation with get, set
    member val Sides: PeelState list = [ Unpeeled; Unpeeled; Unpeeled; Unpeeled] with get, set

    member self.Peel() = BananaHelpers.peel self // Note the dependency on the BananaHelpers module.
    member self.SqueezeJuiceOut() = raise (DontSqueezeTheBananaException self) // This member depends on the exception above.

module BananaHelpers =
    let peel (b: Banana) =
        let flip (banana: Banana) =
            match banana.Orientation with
            | Up ->
                banana.Orientation <- Down
                banana
            | Down -> banana

        let peelSides (banana: Banana) =
            banana.Sides
            |> List.map (function
                         | Unpeeled -> Peeled
                         | Peeled -> Peeled)

        match b.Orientation with
        | Up ->   b |> flip |> peelSides
        | Down -> b |> peelSides
```

请注意，异常`DontSqueezeTheBananaException`和类`Banana`都相互引用。  此外，模块`BananaHelpers`和类`Banana`也相互引用。 如果从`rec` `MutualReferences`命名空间中删除关键字，则无法在 F # 中表示。

此功能也适用于顶级[模块](modules.md)。

## <a name="see-also"></a>另请参阅

- [F # 语言参考](index.md)
- [模块](modules.md)
- [F # RFC FS-1009-允许在文件中的更大范围内进行相互引用类型和模块](https://github.com/fsharp/fslang-design/blob/master/FSharp-4.1/FS-1009-mutually-referential-types-and-modules-single-scope.md)
