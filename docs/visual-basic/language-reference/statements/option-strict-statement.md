---
title: Option Strict Statement
ms.date: 07/20/2015
f1_keywords:
- vb.Strict
- vb.OptionStrict
helpviewer_keywords:
- Strict keyword [Visual Basic]
- Option Strict statement [Visual Basic]
- conversions [Visual Basic], implicit
- late binding [Visual Basic]
- implicit conversions [Visual Basic]
ms.assetid: 5883e0c1-a920-4274-8e46-b0ff047eaee5
ms.openlocfilehash: ab1094961e2bc3aed0e975e40369a5f5c1ba93eb
ms.sourcegitcommit: d2db216e46323f73b32ae312c9e4135258e5d68e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2020
ms.locfileid: "90873133"
---
# <a name="option-strict-statement"></a>Option Strict Statement

将隐式数据类型转换限制为仅进行扩大转换，不允许后期绑定，并允许导致类型的隐式 `Object` 类型化。  
  
## <a name="syntax"></a>语法  
  
```vb  
Option Strict { On | Off }  
```  
  
## <a name="parts"></a>组成部分  
  
|术语|定义|  
|---|---|  
|`On`|可选。 启用 `Option Strict` 检查。|  
|`Off`|可选。 禁用 `Option Strict` 检查。|  
  
## <a name="remarks"></a>备注  

 当 `Option Strict On` 或 `Option Strict` 出现在文件中时，以下情况会导致编译时错误：  
  
- 隐式收缩转换  
  
- 后期绑定  
  
- 隐式键入会导致 `Object` 类型  
  
> [!NOTE]
> 在可以在 "编译" 页上设置的警告配置中 [，"项目设计器" (Visual Basic) ](/visualstudio/ide/reference/compile-page-project-designer-visual-basic)，有三个与导致编译时错误的三个条件相对应的设置。 有关如何使用这些设置的详细信息，请参阅本主题后面的将 [警告配置设置到 IDE](option-strict-statement.md#conditions) 中。  
  
 `Option Strict Off`对于所有三个条件，语句将关闭错误和警告检查，即使关联的 IDE 设置指定打开这些错误或警告。 `Option Strict On`即使关联的 IDE 设置指定关闭这些错误或警告，语句也会对所有三个条件启用错误和警告检查。  
  
 如果使用， `Option Strict` 语句必须出现在文件中的任何其他代码语句之前。  
  
 当你将设置 `Option Strict` 为时 `On` ，Visual Basic 将检查是否为所有编程元素指定了数据类型。 可以显式指定数据类型，也可以使用局部类型推理来指定数据类型。 建议为所有编程元素指定数据类型，原因如下：  
  
- 它为你的变量和参数启用 IntelliSense 支持。 这使您可以在键入代码时查看它们的属性和其他成员。  
  
- 它使编译器可以执行类型检查。 类型检查有助于在运行时查找可能因类型转换错误而失败的语句。 它还标识对不支持这些方法的对象上的方法的调用。  
  
- 它可加速代码的执行。 导致这种情况的原因之一是，如果未指定编程元素的数据类型，则 Visual Basic 编译器会为其分配 `Object` 类型。 编译的代码可能需要在 `Object` 和其他数据类型之间来回转换，这会降低性能。  
  
## <a name="implicit-narrowing-conversion-errors"></a>隐式收缩转换错误  

 隐式数据类型转换为收缩转换时，将发生隐式收缩转换错误。  
  
 Visual Basic 可以将多个数据类型转换为其他数据类型。 当一种数据类型的值转换为精度较低或容量较小的数据类型时，可能会发生数据丢失。 如果这种收缩转换失败，则会发生运行时错误。 `Option Strict` 确保这些收缩转换的编译时通知，以便你可以避免这些转换。 有关详细信息，请参阅 [隐式和显式转换](../../programming-guide/language-features/data-types/implicit-and-explicit-conversions.md) 以及 [扩大和收缩转换](../../programming-guide/language-features/data-types/widening-and-narrowing-conversions.md)。  
  
 可能导致错误的转换包括表达式中发生的隐式转换。 有关详细信息，请参阅下列主题：  
  
- [+ 运算符](../operators/addition-operator.md)  
  
- [+ = 运算符](../operators/addition-assignment-operator.md)  
  
- [\ 运算符 (Visual Basic) ](../operators/integer-division-operator.md)  
  
- [/= 运算符 (Visual Basic) ](../operators/floating-point-division-assignment-operator.md)  
  
- [Char 数据类型](../data-types/char-data-type.md)  
  
 通过使用 [& 运算符](../operators/concatenation-operator.md)来串联字符串时，将所有字符串转换都视为扩大。 因此，这些转换不会生成隐式收缩转换错误，即使 `Option Strict` 启用了也是如此。  
  
 如果调用的方法具有的数据类型与相应的参数不同，则收缩转换将在打开时导致编译时错误 `Option Strict` 。 您可以通过使用扩大转换或显式转换来避免编译时错误。  
  
 在编译时，隐式收缩转换错误将从集合中的元素转换 `For Each…Next` 为循环控制变量。 即使处于打开的情况下也会发生这种情况 `Option Strict` 。 有关详细信息，请参阅中的 "收缩转换" 一节 [。下一语句](for-each-next-statement.md)。  
  
## <a name="late-binding-errors"></a>后期绑定错误  

 如果将对象分配给声明为 `Object` 类型的变量，则该对象为晚期绑定。 有关详细信息，请参阅 [早期和后期绑定](../../programming-guide/language-features/early-late-binding/index.md)。  
  
## <a name="implicit-object-type-errors"></a>隐式对象类型错误  

 如果无法为已声明的变量推断出合适的类型，则会发生隐式对象类型错误，因此 `Object` 类型是推断出来的。 这主要是在未使用 `As` 子句的情况下使用 `Dim` 语句声明变量，且 `Option Infer` 为关闭时发生的。 有关详细信息，请参阅 [选项推断语句](option-infer-statement.md) 和 [Visual Basic 语言规范](../../reference/language-specification/index.md)。  
  
 对于方法参数， `As` 如果为 off，则子句是可选的 `Option Strict` 。 但是，如果任何一个参数使用 `As` 子句，则所有参数都必须使用它。 如果 `Option Strict` 为 on，则 `As` 每个参数定义都需要子句。  
  
 如果在不使用子句的情况下声明变量 `As` 并将其设置为 `Nothing` ，则该变量的类型为 `Object` 。 如果 `Option Strict` 为 on，则在此情况下不会发生编译时错误 `Option Infer` 。 这是一个示例 `Dim something = Nothing` 。  
  
### <a name="default-data-types-and-values"></a>默认数据类型和值  

 下表描述了在 [Dim 语句](dim-statement.md)中指定数据类型和初始值设定项的各种组合的结果。  
  
|是否指定数据类型？|是否指定初始值设定项？|示例|结果|  
|---|---|---|---|  
|否|否|`Dim qty`|如果 `Option Strict` 处于关闭状态（默认），则将变量设置为 `Nothing`。<br /><br /> 如果 `Option Strict` 处于打开状态，则发生编译时错误。|  
|否|是|`Dim qty = 5`|如果 `Option Infer` 处于打开状态（默认），则变量采用初始值设定项的数据类型。 请参阅 [局部类型推理](../../programming-guide/language-features/variables/local-type-inference.md)。<br /><br /> 如果 `Option Infer` 和 `Option Strict` 均处于关闭状态，则变量采用 `Object` 的数据类型。<br /><br /> 如果 `Option Infer` 处于关闭状态但 `Option Strict` 处于打开状态，则发生编译时错误。|  
|是|否|`Dim qty As Integer`|将变量初始化为数据类型的默认值。 有关详细信息，请参阅 [Dim 语句](dim-statement.md)。|  
|是|是|`Dim qty  As Integer = 5`|如果初始值设定项的数据类型不可转换为指定数据类型，则会发生编译时错误。|  
  
## <a name="when-an-option-strict-statement-is-not-present"></a>当 Option Strict 语句不存在时  

 如果源代码不包含 `Option Strict` 语句，则使用 "编译" 页上的 " **Option strict** " 设置 [，"项目设计器" (Visual Basic) ](/visualstudio/ide/reference/compile-page-project-designer-visual-basic) 。 " **编译" 页** 中的设置提供对生成错误的条件的更多控制。  
  
 如果你使用的是命令行编译器，则可以使用 [-optionstrict](../../reference/command-line-compiler/optionstrict.md) 编译器选项来指定的设置 `Option Strict` 。  
  
### <a name="to-set-option-strict-in-the-ide"></a>在 IDE 中设置 Option Strict  

[!INCLUDE[note_settings_general](~/includes/note-settings-general-md.md)]  
  
1. 在 **解决方案资源管理器**中，选择一个项目。 在 **“项目”** 菜单上，单击 **“属性”** 。  
  
2. 在 " **编译** " 选项卡上，在 " **选项严格限制** " 框中设置值。  
  
### <a name="to-set-warning-configurations-in-the-ide"></a><a name="conditions"></a> 在 IDE 中设置警告配置  

 使用 "编译" [页时，"项目设计器" (Visual Basic) ](/visualstudio/ide/reference/compile-page-project-designer-visual-basic) 而不是 `Option Strict` 语句，您可以对生成错误的条件进行其他控制。 "**编译" 页**的 "**警告配置**" 部分具有与在上时导致编译时错误的三个条件相对应的设置 `Option Strict` 。 这些设置如下：  
  
- **隐式转换**  
  
- 晚期绑定；调用可能在运行时失败****  
  
- 隐式类型；假定为对象****  
  
 “Option Strict”设置为“开启”时，所有这三个警告配置设置都将被设置为“错误”************。 “Option Strict”设置为“关闭”时，所有这三个设置都将被设置为“无”************。  
  
 可单独将各个警告配置设置更改为“无”、“警告”或“错误”************。 如果所有三个警告配置设置均设置为 " **错误**"， `On` 则会出现在 `Option strict` 框中。 如果所有三个均设置为 " **无**"，则 `Off` 在此框中显示。 对于这些配置的任何其他组合，显示“(自定义)”****。  
  
### <a name="to-set-the-option-strict-default-setting-for-new-projects"></a>为新项目设置选项 Strict 默认设置  

 创建项目时，"**编译**" 选项卡上的 " **option strict** " 设置设置为 "**选项**" 对话框中的 " **option strict** " 设置。  
  
 若要 `Option Strict` 在此对话框中设置，请在 " **工具** " 菜单上单击 " **选项**"。 在“选项”对话框中，展开“项目和解决方案”，然后单击“VB 默认值”************。 **VB 默认**设置中的初始默认设置为 `Off` 。  
  
### <a name="to-set-option-strict-on-the-command-line"></a>在命令行上设置 Option Strict  

 在**vbc**命令中包含[-optionstrict](../../reference/command-line-compiler/optionstrict.md)编译器选项。  
  
## <a name="example"></a>示例  

 下面的示例演示由收缩转换的隐式类型转换导致的编译时错误。 此类别的错误对应于 "**编译" 页**上的**隐式转换**条件。  
  
 [!code-vb[VbVbalrStatements#161](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/class13.vb#161)]  
  
## <a name="example"></a>示例  

 下面的示例演示后期绑定导致的编译时错误。 此类别的错误对应于后期绑定; "**编译" 页**上的 "**调用可能在运行时失败**" 条件。  
  
 [!code-vb[VbVbalrStatements#162](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/class13.vb#162)]  
  
## <a name="example"></a>示例  

 下面的示例演示由使用隐式类型声明的变量导致的错误 `Object` 。 此类别的错误对应于 **隐式类型;** " **编译" 页**上的 "对象假定" 条件。  
  
 [!code-vb[VbVbalrStatements#163](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/class13.vb#163)]  
  
 [!code-vb[VbVbalrStatements#164](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/class13.vb#164)]  
  
## <a name="see-also"></a>另请参阅

- [Widening and Narrowing Conversions](../../programming-guide/language-features/data-types/widening-and-narrowing-conversions.md)
- [隐式转换和显式转换](../../programming-guide/language-features/data-types/implicit-and-explicit-conversions.md)
- [“项目设计器”->“编译”页 (Visual Basic)](/visualstudio/ide/reference/compile-page-project-designer-visual-basic)
- [Option Explicit 语句](option-explicit-statement.md)
- [Type Conversion Functions](../functions/type-conversion-functions.md)
- [如何：访问对象的成员](../../programming-guide/language-features/variables/how-to-access-members-of-an-object.md)
- [XML 中的嵌入式表达式](../../programming-guide/language-features/xml/embedded-expressions-in-xml.md)
- [宽松委托转换](../../programming-guide/language-features/delegates/relaxed-delegate-conversion.md)
- [Late Binding in Office Solutions](/visualstudio/vsto/late-binding-in-office-solutions)
- [-optionstrict](../../reference/command-line-compiler/optionstrict.md)
- [“选项”对话框 ->“项目”->“Visual Basic 默认值”](/visualstudio/ide/reference/visual-basic-defaults-projects-options-dialog-box)
