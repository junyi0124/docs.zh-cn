---
title: 按位置和按名称传递参数
ms.date: 02/01/2018
helpviewer_keywords:
- arguments [Visual Basic], passing by name
- procedures [Visual Basic], arguments
- := passing arguments
- procedure arguments
- Visual Basic code, procedures
- named arguments [Visual Basic], passing arguments
- arguments [Visual Basic], passing by position
- Function procedures [Visual Basic], passing arguments
- named parameters [Visual Basic], passing arguments
- named arguments
- passing parameters [Visual Basic], named parameter arguments
- passing parameters [Visual Basic], by position
- procedures [Visual Basic], calling
- named parameters
- Sub procedures [Visual Basic], passing arguments
- argument passing
- passing parameters [Visual Basic], by name
- argument passing [Visual Basic], by position
- arguments [Visual Basic], listing by name
ms.assetid: 1ad7358f-1da9-48da-a95b-f3c7ed41eff3
ms.openlocfilehash: 686b64977f086c8128e56298a0ed8c5aa0c51efa
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84364027"
---
# <a name="passing-arguments-by-position-and-by-name-visual-basic"></a>按位置和名称传递自变量 (Visual Basic)

调用 `Sub` 或 `Function` 过程时，可以*按位置*（按照它们在过程定义中的显示顺序）传递参数，也可以*按名称*传递参数，而不考虑位置。

按名称传递参数时，请指定参数的声明名称，后跟冒号和等号（ `:=` ），后跟自变量值。 可以按任意顺序提供命名参数。

例如，下面的 `Sub` 过程采用三个参数：

[!code-vb[SampleProcedure](../../../../../samples/snippets/visualbasic/programming-guide/language-features/passing-named-arguments/module1.vb#1)]

调用此过程时，可以按位置、名称或同时使用这两个参数来提供参数。

## <a name="passing-arguments-by-position"></a>按位置传递参数

可以调用方法，该 `Display` 方法的参数按位置传递，并用逗号进行分隔，如以下示例中所示：

[!code-vb[ByPosition](../../../../../samples/snippets/visualbasic/programming-guide/language-features/passing-named-arguments/module1.vb#2)]

如果在位置参数列表中省略了可选参数，则必须用逗号保存其位置。 下面的示例调用 `Display` 不带参数的方法 `age` ：

[!code-vb[ByPositionWithOptionalArgument](../../../../../samples/snippets/visualbasic/programming-guide/language-features/passing-named-arguments/module1.vb#3)]

## <a name="passing-arguments-by-name"></a>按名称传递参数

另外， `Display` 还可以使用按名称传递的自变量（也用逗号分隔）调用，如下面的示例中所示：

[!code-vb[ByName](../../../../../samples/snippets/visualbasic/programming-guide/language-features/passing-named-arguments/module1.vb#4)]

当调用具有多个可选参数的过程时，以这种方式传递参数的方法特别有用。 如果按名称提供参数，则无需使用连续的逗号表示缺少的位置参数。 通过按名称传递参数，还可以更轻松地跟踪正在传递的参数和要省略的参数。

## <a name="mixing-arguments-by-position-and-by-name"></a>按位置和按名称混合参数

可以在单个过程调用中按位置和按名称提供参数，如以下示例中所示：

[!code-vb[ByNameAndPosition](../../../../../samples/snippets/visualbasic/programming-guide/language-features/passing-named-arguments/module1.vb#5)]

在前面的示例中， `age` 由于 `birth` 是按名称传递的，因此不需要额外的逗号来容纳省略的参数。

在15.5 之前的 Visual Basic 版本中，当你通过位置和名称的组合提供参数时，位置参数必须是第一个。 按名称提供参数后，所有剩余参数都必须按名称传递。  例如，以下对方法的调用 `Display` 显示编译器错误[BC30241：应为命名参数](../../../misc/bc30241.md)。

[!code-vb[ByNameAndPosition](../../../../../samples/snippets/visualbasic/programming-guide/language-features/passing-named-arguments/module1.vb#6)]

从 Visual Basic 15.5 开始，如果结束位置参数位于正确的位置，则位置参数可以跟随命名参数。 如果在 Visual Basic 15.5 下编译，则以前对方法的调用将 `Display` 成功编译，不再生成编译器错误[BC30241](../../../misc/bc30241.md)。

如果要使用命名参数使代码更具可读性，此功能可以按任意顺序混合并匹配命名参数和位置参数。 例如，下面的 `Person` 类构造函数需要类型的两个参数，这两个参数 `Person` 都可以是 `Nothing` 。

[!code-vb[ByNameAndPosition](../../../../../samples/snippets/visualbasic/programming-guide/language-features/passing-named-arguments/module1.vb#7)]

当和参数的值为时，使用混合的命名参数和位置参数可确保代码的 `father` 意图 `mother` `Nothing` ：

[!code-vb[ByNameAndPosition](../../../../../samples/snippets/visualbasic/programming-guide/language-features/passing-named-arguments/module1.vb#8)]

若要在位置参数后面添加命名参数，必须将以下元素添加到 Visual Basic 项目（ \* .vbproj）文件中：

```xml
<PropertyGroup>
  <LangVersion>15.5</LangVersion>
</PropertyGroup>
```

有关详细信息，请参阅[设置 Visual Basic 语言版本](../../../language-reference/configure-language-version.md)。

## <a name="restrictions-on-supplying-arguments-by-name"></a>按名称提供参数的限制

不能按名称传递参数，以避免输入所需的参数。 只能省略可选参数。

不能按名称传递参数数组。 这是因为，当你调用过程时，你为参数数组提供了不限数量的逗号分隔参数，且编译器无法将多个参数与单个名称相关联。

## <a name="see-also"></a>另请参阅

- [过程](./index.md)
- [过程形参和实参](./procedure-parameters-and-arguments.md)
- [如何：将参数传递给过程](./how-to-pass-arguments-to-a-procedure.md)
- [按值和按引用传递参数](./passing-arguments-by-value-and-by-reference.md)
- [可选参数](./optional-parameters.md)
- [参数数组](./parameter-arrays.md)
- [可选](../../../language-reference/modifiers/optional.md)
- [ParamArray](../../../language-reference/modifiers/paramarray.md)
