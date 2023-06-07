---
title: 故障排除过程
ms.date: 07/20/2015
helpviewer_keywords:
- troubleshooting Visual Basic, procedures
- procedures [Visual Basic], troubleshooting
- Visual Basic code, procedures
- troubleshooting procedures
- procedures [Visual Basic], about procedures
ms.assetid: 525721e8-2e02-4f75-b5d8-6b893462cf2b
ms.openlocfilehash: 8d4c4929e299efb12d283492a101c18ae794110b
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/22/2019
ms.locfileid: "74352519"
---
# <a name="troubleshooting-procedures-visual-basic"></a>故障排除过程（Visual Basic）

此页列出了在使用过程时可能出现的一些常见问题。  
  
## <a name="returning-an-array-type-from-a-function-procedure"></a>从函数过程返回数组类型

如果 `Function` 过程返回数组数据类型，则不能使用 `Function` 名称将值存储在数组的元素中。 如果尝试执行此操作，编译器会将其解释为对 `Function`的调用。 下面的示例生成编译器错误：
  
```vb
Function AllOnes(n As Integer) As Integer()
   For i As Integer = 1 To n - 1  
      ' The following statement generates a COMPILER ERROR.  
      AllOnes(i) = 1  
   Next  

   ' The following statement generates a COMPILER ERROR.  
   Return AllOnes()  
End Function
```

语句 `AllOnes(i) = 1` 会生成编译器错误，因为该语句似乎调用了具有错误数据类型的参数 `AllOnes` （标量 `Integer`，而不是 `Integer` 数组）。 语句 `Return AllOnes()` 会生成编译器错误，因为它似乎调用不带参数的 `AllOnes`。  
  
 **正确的方法：** 为了能够修改要返回的数组元素，请将内部数组定义为局部变量。 下面的示例在编译时不会出现错误：

 [!code-vb[VbVbcnProcedures#66](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#66)]

## <a name="argument-not-modified-by-procedure-call"></a>过程调用未修改的参数

如果打算允许过程更改调用代码中的参数基础的编程元素，则必须通过引用传递该元素。 但过程可以访问引用类型参数的元素，即使你按值传递它。

- **基础变量**。 若要允许该过程替换基础变量元素本身的值，该过程必须声明参数[ByRef](../../../language-reference/modifiers/byref.md)。 此外，调用代码不能将参数括在括号中，因为这会重写 `ByRef` 传递机制。

- **引用类型元素**。 如果声明参数[ByVal](../../../language-reference/modifiers/byval.md)，则该过程将无法修改基础变量元素本身。 但是，如果参数是引用类型，则该过程可以修改它所指向的对象的成员，即使它不能替换变量的值。 例如，如果参数是一个数组变量，则该过程无法向其分配新数组，但它可以更改其一个或多个元素。 更改的元素将反映在调用代码的基础数组变量中。

下面的示例定义了两个过程，这些过程按值获取数组变量并对其元素进行操作。 过程 `increase` 只是向每个元素添加一个。 过程 `replace` 向参数 `a()` 分配新数组，然后将一个数组添加到每个元素。 但是，重新分配不会影响调用代码中的基础数组变量，因为 `a()` `ByVal`声明。

[!code-vb[VbVbcnProcedures#35](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#35)]

[!code-vb[VbVbcnProcedures#38](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#38)]

下面的示例调用 `increase` 和 `replace`：

[!code-vb[VbVbcnProcedures#37](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#37)]
  
第一个 `MsgBox` 调用显示 "增加（n）：11，21，31，41" 之后的 "。 由于 `n` 是引用类型，因此 `increase` 可以更改其成员，即使它是 `ByVal`传递的。

第二个 `MsgBox` 调用显示 "replace （n）：11，21，31，41" 之后的。 由于 `n` 传递 `ByVal`，`replace` 无法通过为其分配新数组来修改变量 `n`。 如果 `replace` 创建新的数组实例 `k` 并将其分配给本地变量 `a`，则它将失去调用代码传入的 `n` 的引用。 递增 `a`的成员时，只会影响 `k` 的本地数组。

**正确的方法：** 若要能够修改基础变量元素本身，请按引用传递它。 下面的示例演示 `replace` 声明中的更改，这些更改允许它在调用代码中将一个数组替换为另一个数组：

[!code-vb[VbVbcnProcedures#64](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#64)]

## <a name="unable-to-define-an-overload"></a>无法定义重载

如果要定义过程的重载版本，则必须使用相同的名称，但不能使用其他签名。 如果编译器无法将声明与具有相同签名的重载区分开来，则会生成错误。

过程的*签名*由过程名称和参数列表确定。 每个重载必须与所有其他重载具有相同的名称，但必须在签名的其他至少一个组件中与所有重载的名称不同。 有关更多信息，请参见 [Procedure Overloading](./procedure-overloading.md)。

以下项（即使它们属于参数列表）不是过程签名的组成部分：

- 过程修饰符关键字，如 `Public`、`Shared`和 `Static`。
- 参数名称。
- 参数修饰符关键字，如 `ByRef` 和 `Optional`。
- 返回值的数据类型（转换运算符除外）。

不能通过仅改变一个或多个前述项来重载过程。

**正确的方法：** 若要定义过程重载，必须更改签名。 由于必须使用相同的名称，因此必须更改参数的数目、顺序或数据类型。 在泛型过程中，可以改变类型参数的数目。 在转换运算符（[CType 函数](../../../language-reference/functions/ctype-function.md)）中，可以改变返回类型。

### <a name="overload-resolution-with-optional-and-paramarray-arguments"></a>具有可选参数和 ParamArray 参数的重载决策

如果要重载具有一个或多个[可选](../../../language-reference/modifiers/optional.md)参数或[ParamArray](../../../language-reference/modifiers/paramarray.md)参数的过程，则必须避免复制任何*隐式重载*。 有关信息，请参阅[重载过程中的注意事项](./considerations-in-overloading-procedures.md)。

## <a name="calling-the-wrong-version-of-an-overloaded-procedure"></a>调用错误的重载过程版本

如果过程具有多个重载版本，你应该熟悉其所有参数列表，并了解 Visual Basic 如何在重载之间解析调用。 否则，可以调用非预期重载。

确定要调用的重载时，请注意遵守以下规则：

- 提供正确的参数数目，并按正确的顺序排列。  
- 理想情况下，自变量的数据类型应与对应参数的数据类型完全相同。 在任何情况下，每个参数的数据类型必须扩大到其对应参数的数据类型。 即使[Option Strict 语句](../../../language-reference/statements/option-strict-statement.md)设置为 `Off`也是如此。 如果重载需要自变量列表进行任何收缩转换，则不能调用该重载。
- 如果提供需要扩大的参数，请将其数据类型尽可能靠近相应的参数数据类型。 如果两个或更多重载接受您的参数数据类型，则编译器会将调用解析为对最小扩大量进行调用的重载。

在准备参数时，可以使用[CType 函数](../../../language-reference/functions/ctype-function.md)转换关键字来减少数据类型不匹配的几率。

### <a name="overload-resolution-failure"></a>重载决策失败

调用重载过程时，编译器会尝试消除除一个重载以外的所有重载。 如果成功，它将解析对该重载的调用。 如果它消除了所有重载，或者无法将符合条件的重载减少到单个候选项，则会生成错误。

下面的示例演示了重载决策过程：

[!code-vb[VbVbcnProcedures#62](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#62)]

[!code-vb[VbVbcnProcedures#63](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#63)]
  
在第一次调用中，编译器将消除第一个重载，因为第一个参数的类型（`Short`）会缩小到相应参数的类型（`Byte`）。 然后，它将消除第三个重载，因为第二个重载（`Short` 和 `Single`）中的每个参数类型扩大为第三个重载（`Integer` 和 `Single`）中的相应类型。 第二个重载需要更少的扩展，因此编译器将其用于调用。

在第二次调用中，编译器无法根据收缩消除任何重载。 它消除第三个重载的原因与第一次调用中的相同原因，因为它可以通过更少的参数类型来调用第二个重载。 但编译器无法在第一个和第二个重载之间解析。 每个都有一个已定义的参数类型，该类型扩大到另一个中的相应类型（`Byte` 到 `Short`，但 `Single` `Double`）。 因此，编译器将生成重载决策错误。

**正确的方法：** 若要能够无歧义地调用重载过程，请使用[CType 函数](../../../language-reference/functions/ctype-function.md)将参数数据类型与参数类型相匹配。 下面的示例演示对 `z` 的调用，该调用强制解析为第二个重载。

[!code-vb[VbVbcnProcedures#65](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#65)]

### <a name="overload-resolution-with-optional-and-paramarray-arguments"></a>具有可选参数和 ParamArray 参数的重载决策

如果过程的两个重载具有相同的签名，但最后一个参数在另[一个中声明](../../../language-reference/modifiers/paramarray.md)为[可选](../../../language-reference/modifiers/optional.md)，则编译器将根据最接近的匹配项解析对该过程的调用。 有关更多信息，请参见 [Overload Resolution](./overload-resolution.md)。

## <a name="see-also"></a>另请参阅

- [过程](index.md)
- [Sub 过程](sub-procedures.md)
- [Function 过程](function-procedures.md)
- [属性过程](property-procedures.md)
- [运算符过程](operator-procedures.md)
- [过程参数和自变量](procedure-parameters-and-arguments.md)
- [过程重载](procedure-overloading.md)
- [重载过程注意事项](considerations-in-overloading-procedures.md)
- [重载决策](overload-resolution.md)
