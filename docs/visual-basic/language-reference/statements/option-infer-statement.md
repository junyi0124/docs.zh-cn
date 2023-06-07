---
title: Option Infer 语句
ms.date: 07/20/2015
f1_keywords:
- vb.OptionInfer
- vb.Infer
helpviewer_keywords:
- variables [Visual Basic], declaring
- Option Infer statement [Visual Basic]
- Infer keyword [Visual Basic]
- declaring variables [Visual Basic], inferred
- inferred variable declaration
ms.assetid: 4ad3e6e9-8f5b-4209-a248-de22ef6e4652
ms.openlocfilehash: 977e492c1c8ec5040c22169d91268c9c2241f6c4
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84404351"
---
# <a name="option-infer-statement"></a>Option Infer 语句

允许声明变量时使用局部类型推理。

## <a name="syntax"></a>语法

```vb
Option Infer { On | Off }
```

## <a name="parts"></a>组成部分

|术语|定义|
|---|---|
|`On`|可选。 启用局部类型推理。|
|`Off`|可选。 禁用局部类型推理。|

## <a name="remarks"></a>备注

若要在文件中设置 `Option Infer`，请在任何其他源代码之前、文件顶部键入 `Option Infer On` 或 `Option Infer Off`。 如果为文件中 `Option Infer` 设置的值与在 IDE 中或在命令行上设置的值冲突，则文件中的值优先。

将 `Option Infer` 设置为 `On`时，你可以无需显式声明数据类型就声明本地变量。 编译器会从其初始化表达式的类型推断出变量的数据类型。

在下图中，`Option Infer` 处于打开状态。 声明 `Dim someVar = 2` 中的变量由类型推理声明为整数。

以下屏幕截图显示了选项推断为启用时的 IntelliSense：

![当选项推断为 on 时显示 IntelliSense 视图的屏幕截图。](./media/option-infer-statement/option-infer-as-integer-on.png)

在下图中，`Option Infer` 处于关闭状态。 声明 `Dim someVar = 2` 中的变量由类型推理声明为 `Object`。 在此示例中，在 "编译" 页的 "[项目设计器" （Visual Basic）](/visualstudio/ide/reference/compile-page-project-designer-visual-basic)上，" **Option Strict** " 设置设置为 "**关闭**"。

以下屏幕截图显示了选项推断为 off 时的 IntelliSense：

![当选项推理为 off 时显示 IntelliSense 视图的屏幕截图。](./media/option-infer-statement/option-infer-as-object-off.png)

> [!NOTE]
> 变量声明为 `Object` 时，可在程序运行时更改运行时类型。 Visual Basic 执行称为*装箱*和*取消装箱*的操作，以便在 `Object` 和值类型之间进行转换，这会使执行速度变慢。 有关装箱和取消装箱的信息，请参阅[Visual Basic 语言规范](~/_vblang/spec/conversions.md#value-type-conversions)。

类型推理适用于过程级，且在类、结构、模块或接口中的过程之外不适用。

有关其他信息，请参阅[局部类型推理](../../programming-guide/language-features/variables/local-type-inference.md)。

## <a name="when-an-option-infer-statement-is-not-present"></a>当 Option Infer 语句不存在时

如果源代码不包含 `Option Infer` 语句，则使用 "编译" 页上的 "推断设置"**选项**，将使用[项目设计器（Visual Basic）](/visualstudio/ide/reference/compile-page-project-designer-visual-basic) 。 如果使用命令行编译器，则使用[-optioninfer](../../reference/command-line-compiler/optioninfer.md)编译器选项。

#### <a name="to-set-option-infer-in-the-ide"></a>若要在 IDE 中设置 Option Infer

1. 在**解决方案资源管理器**中，选择一个项目。 在“项目”菜单上，单击“属性”   。

2. 单击“编译”  选项卡。

3. 设置 "**选项推断**框" 中的值。

创建新项目时，"**编译**" 选项卡上的 "**选择推断**设置" 设置为 " **VB 默认值**" 对话框中的 "推断设置"**选项**。 若要访问 " **VB 默认值**" 对话框，请在 "**工具**" 菜单上单击 "**选项**"。 在“选项”对话框中，展开“项目和解决方案”，然后单击“VB 默认值”************。 **VB 默认**设置中的初始默认设置为 `On` 。

#### <a name="to-set-option-infer-on-the-command-line"></a>若要设置命令行上的 Option Infer

在**vbc**命令中包含[-optioninfer](../../reference/command-line-compiler/optioninfer.md)编译器选项。

## <a name="default-data-types-and-values"></a>默认数据类型和值

下表描述了指定 `Dim` 语句中数据类型和初始值设定项的各种组合的结果。

|是否指定数据类型？|是否指定初始值设定项？|示例|结果|
|---|---|---|---|
|否|否|`Dim qty`|如果 `Option Strict` 处于关闭状态（默认），则将变量设置为 `Nothing`。<br /><br /> 如果 `Option Strict` 处于打开状态，则发生编译时错误。|
|否|是|`Dim qty = 5`|如果 `Option Infer` 处于打开状态（默认），则变量采用初始值设定项的数据类型。 请参阅[局部类型推理](../../programming-guide/language-features/variables/local-type-inference.md)。<br /><br /> 如果 `Option Infer` 和 `Option Strict` 均处于关闭状态，则变量采用 `Object` 的数据类型。<br /><br /> 如果 `Option Infer` 处于关闭状态但 `Option Strict` 处于打开状态，则发生编译时错误。|
|是|No|`Dim qty As Integer`|将变量初始化为数据类型的默认值。 有关详细信息，请参阅[Dim 语句](dim-statement.md)。|
|是|是|`Dim qty  As Integer = 5`|如果初始值设定项的数据类型不可转换为指定数据类型，则会发生编译时错误。|

## <a name="example"></a>示例

下例演示了 `Option Infer` 语句启用局部类型推理的方式。

[!code-vb[VbVbalrTypeInference#6](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrTypeInference/VB/Class1.vb#6)]

## <a name="example"></a>示例

下例演示了变量被标识为 `Object` 时运行时类型可以改变的情况。

[!code-vb[VbVbalrTypeInference#11](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrTypeInference/VB/Class1.vb#11)]

## <a name="see-also"></a>另请参阅

- [Dim 语句](dim-statement.md)
- [局部类型推理](../../programming-guide/language-features/variables/local-type-inference.md)
- [Option Compare 语句](option-compare-statement.md)
- [Option Explicit 语句](option-explicit-statement.md)
- [Option Strict 语句](option-strict-statement.md)
- [“选项”对话框 ->“项目”->“Visual Basic 默认值”](/visualstudio/ide/reference/visual-basic-defaults-projects-options-dialog-box)
- [-optioninfer](../../reference/command-line-compiler/optioninfer.md)
- [装箱和取消装箱](../../../csharp/programming-guide/types/boxing-and-unboxing.md)
