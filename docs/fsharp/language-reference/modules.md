---
title: 模块
description: '了解 f # 模块是 f # 代码的分组，如 f # 程序中的值、类型和函数值。'
ms.date: 04/24/2017
ms.openlocfilehash: 5f99bbd8069478bf0c7db2800ae545f31926728a
ms.sourcegitcommit: de7f589de07a9979b6ac28f54c3e534a617d9425
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/05/2020
ms.locfileid: "82794359"
---
# <a name="modules"></a>模块

在 F # 语言的上下文中，*模块*是 f # 代码的分组，如 f # 程序中的值、类型和函数值。 对模块中的代码进行分组有助于将相关代码放在一起，并有助于避免程序中的名称冲突。

## <a name="syntax"></a>语法

```fsharp
// Top-level module declaration.
module [accessibility-modifier] [qualified-namespace.]module-name
declarations
// Local module declaration.
module [accessibility-modifier] module-name =
    declarations
```

## <a name="remarks"></a>备注

F # 模块是 F # 代码构造的分组，如类型、值、函数值和绑定中`do`的代码。 它实现为只有静态成员的公共语言运行时（CLR）类。 模块声明有两种类型，具体取决于整个文件是否包含在模块中：顶级模块声明和局部模块声明。 顶级模块声明在模块中包含整个文件。 顶级模块声明只能显示为文件中的第一个声明。

在顶级模块声明的语法中，可选的*限定命名空间*是包含模块的嵌套命名空间名称的序列。 不需要事先声明限定的命名空间。

无需缩进顶级模块中的声明。 必须缩进本地模块中的所有声明。 在本地模块声明中，仅在该模块声明下缩进的声明是该模块的一部分。

如果代码文件不是以顶级模块声明或命名空间声明开头，则该文件的整个内容（包括任何本地模块）将成为隐式创建的顶级模块的一部分，该模块的名称与该文件的名称相同，并且第一个字母转换为大写。 例如，请考虑以下文件。

[!code-fsharp[Main](~/samples/snippets/fsharp/modules/snippet6601.fs)]

此文件的编译方式如下：

[!code-fsharp[Main](~/samples/snippets/fsharp/modules/snippet6602.fs)]

如果文件中有多个模块，则必须对每个模块使用本地模块声明。 如果声明了封闭命名空间，则这些模块是封闭命名空间的一部分。 如果未声明封闭命名空间，模块将成为隐式创建的顶级模块的一部分。 下面的代码示例显示了一个包含多个模块的代码文件。 编译器隐式创建一个名为`Multiplemodules`、 `MyModule1`和的顶级模块， `MyModule2`它们嵌套在该顶级模块中。

[!code-fsharp[Main](~/samples/snippets/fsharp/modules/snippet6603.fs)]

如果在一个项目中或在单个编译中有多个文件，或者如果要生成一个库，则必须在该文件的顶部包含一个命名空间声明或模块声明。 F # 编译器仅在项目或编译命令行中只有一个文件，并且你正在创建应用程序时，才会隐式确定模块名称。

可*访问性修饰符*可以是下列其中一项： `public`、 `private`、 `internal`。 有关详细信息，请参阅[访问控制](access-control.md)。 默认值为 public。

## <a name="referencing-code-in-modules"></a>引用模块中的代码

当您从其他模块引用函数、类型和值时，您必须使用限定的名称或打开该模块。 如果使用限定名，则必须为所需的程序元素指定命名空间、模块和标识符。 使用句点（.）分隔限定路径的每个部分，如下所示。

`Namespace1.Namespace2.ModuleName.Identifier`

您可以打开该模块或一个或多个命名空间来简化代码。 有关打开命名空间和模块的详细信息，请参阅[导入`open`声明：关键字](import-declarations-the-open-keyword.md)。

下面的代码示例演示一个顶级模块，其中包含直到文件结尾的所有代码。

[!code-fsharp[Main](~/samples/snippets/fsharp/modules/snippet6604.fs)]

若要在同一项目中的另一个文件中使用此代码，请使用限定的名称，或者在使用函数之前打开该模块，如以下示例中所示。

[!code-fsharp[Main](~/samples/snippets/fsharp/modules/snippet6605.fs)]

## <a name="nested-modules"></a>嵌套模块

模块可以嵌套。 内部模块必须缩进到外部模块声明，以指示它们是内部模块，而不是新模块。 例如，比较以下两个示例。 模块`Z`是以下代码中的内部模块。

[!code-fsharp[Main](~/samples/snippets/fsharp/modules/snippet6607.fs)]

但模块`Z`是以下代码中的`Y`模块的同级。

[!code-fsharp[Main](~/samples/snippets/fsharp/modules/snippet6608.fs)]
模块`Z`在下面的代码中也是同级模块，因为它不会像模块`Y`中的其他声明那样缩进。

[!code-fsharp[Main](~/samples/snippets/fsharp/modules/snippet6609.fs)]
最后，如果外部模块没有声明，后面紧跟另一个模块声明，则假定新的模块声明为内部模块，但编译器将在第二个模块定义未缩进大于第一个模块时发出警告。

[!code-fsharp[Main](~/samples/snippets/fsharp/modules/snippet6610.fs)]
若要消除此警告，请缩进内部模块。

[!code-fsharp[Main](~/samples/snippets/fsharp/modules/snippet6611.fs)]
如果要将文件中的所有代码都放在单个外部模块中，并且需要内部模块，则外部模块不需要等号，而包含任何内部模块声明的声明（包括任何内部模块声明）无需缩进。 内部模块声明内的声明必须缩进。 下面的代码演示了这种情况。

[!code-fsharp[Main](~/samples/snippets/fsharp/modules/snippet6612.fs)]

## <a name="recursive-modules"></a>递归模块

F # 4.1 引入了模块的概念，允许所有包含的代码相互递归。  这是通过`module rec`实现的。  使用`module rec`可以减少无法在类型和模块之间编写相互引用代码的一些难题。  下面是一个示例：

```fsharp
module rec RecursiveModule =
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

请注意，异常`DontSqueezeTheBananaException`和类`Banana`都相互引用。  此外，模块`BananaHelpers`和类`Banana`也相互引用。  如果从`rec` `RecursiveModule`模块中删除关键字，则无法在 F # 中表示。

在 F # 4.1 的[命名空间](namespaces.md)中也可以实现此功能。

## <a name="see-also"></a>另请参阅

- [F # 语言参考](index.md)
- [命名空间](namespaces.md)
- [F # RFC FS-1009-允许在文件中的更大范围内进行相互引用类型和模块](https://github.com/fsharp/fslang-design/blob/master/FSharp-4.1/FS-1009-mutually-referential-types-and-modules-single-scope.md)
