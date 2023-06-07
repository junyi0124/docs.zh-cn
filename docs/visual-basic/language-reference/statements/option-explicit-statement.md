---
title: Option Explicit 语句
ms.date: 07/20/2015
f1_keywords:
- vb.Explicit
- vb.OptionExplicit
helpviewer_keywords:
- declaring variables [Visual Basic], explicit
- forced variable declaration in Option Explicit statement [Visual Basic]
- Explicit keyword
- explicit variable declaration
- Option Explicit statement [Visual Basic]
ms.assetid: e82ac1ad-2cd3-49b2-b985-8bcf016f3fcc
ms.openlocfilehash: 44bf8205ec071710ee3660968ab3c3e9af33f74d
ms.sourcegitcommit: d2db216e46323f73b32ae312c9e4135258e5d68e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2020
ms.locfileid: "90874940"
---
# <a name="option-explicit-statement-visual-basic"></a>Option Explicit 语句 (Visual Basic)

强制显式声明文件中的所有变量，或允许隐式声明变量。  
  
## <a name="syntax"></a>语法  
  
```vb  
Option Explicit { On | Off }  
```  
  
## <a name="parts"></a>组成部分  

 `On`  
 可选。 启用 `Option Explicit` 检查。 如果 `On` `Off` 未指定或，则默认值为 `On` 。  
  
 `Off`  
 可选。 禁用 `Option Explicit` 检查。  
  
## <a name="remarks"></a>备注  

 当 `Option Explicit On` 或 `Option Explicit` 出现在文件中时，必须使用或语句显式声明所有变量 `Dim` `ReDim` 。 如果尝试使用未声明的变量名称，则会在编译时出现错误。 `Option Explicit Off`语句允许隐式声明变量。  
  
 使用时，`Option Explicit` 语句必须在文件中任何其他源代码语句之前。  
  
> [!NOTE]
> 将设置 `Option Explicit` 为 `Off` 通常不是好的做法。 在一个或多个位置拼错变量名称，将会在程序运行时导致意想不到的结果。  
  
## <a name="when-an-option-explicit-statement-is-not-present"></a>当选项显式语句不存在时  

 如果源代码不包含 `Option Explicit` 语句，则使用 "编译" 页上的 " **Option Explicit** " 设置，将使用 " [项目设计器" (Visual Basic) ](/visualstudio/ide/reference/compile-page-project-designer-visual-basic) 。 如果使用命令行编译器，则使用 [-optionexplicit](../../reference/command-line-compiler/optionexplicit.md) 编译器选项。  
  
#### <a name="to-set-option-explicit-in-the-ide"></a>在 IDE 中设置 Option Explicit  
  
1. 在 **解决方案资源管理器**中，选择一个项目。 在“项目”菜单上，单击“属性” 。  
  
2. 单击“编译”选项卡。  
  
3. 在 " **选项** " 框中设置值。  
  
 创建新项目时，"**编译**" 选项卡上的 " **option explicit** " 设置设置为 " **VB 默认值**" 对话框中的 "选项" "**显式**设置"。 若要访问 " **VB 默认值** " 对话框，请在 " **工具** " 菜单上单击 " **选项**"。 在“选项”对话框中，展开“项目和解决方案”，然后单击“VB 默认值”************。 **VB 默认**设置中的初始默认设置为 `On` 。  
  
#### <a name="to-set-option-explicit-on-the-command-line"></a>在命令行上设置 Option Explicit  
  
- 在**vbc**命令中包含[-optionexplicit](../../reference/command-line-compiler/optionexplicit.md)编译器选项。  
  
## <a name="example"></a>示例  

 下面的示例使用 `Option Explicit` 语句强制所有变量的显式声明。 尝试使用未声明的变量会导致编译时错误。  
  
 [!code-vb[VbVbalrStatements#47](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#47)]  
  
 [!code-vb[VbVbalrStatements#48](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class2.vb#48)]  
  
## <a name="see-also"></a>另请参阅

- [Dim 语句](dim-statement.md)
- [ReDim 语句](redim-statement.md)
- [Option Compare 语句](option-compare-statement.md)
- [Option Strict 语句](option-strict-statement.md)
- [-optioncompare](../../reference/command-line-compiler/optioncompare.md)
- [-optionexplicit](../../reference/command-line-compiler/optionexplicit.md)
- [-optionstrict](../../reference/command-line-compiler/optionstrict.md)
- [“选项”对话框 ->“项目”->“Visual Basic 默认值”](/visualstudio/ide/reference/visual-basic-defaults-projects-options-dialog-box)
