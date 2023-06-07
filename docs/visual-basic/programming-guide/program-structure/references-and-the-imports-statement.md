---
title: 引用和 Imports 语句
ms.date: 07/20/2015
helpviewer_keywords:
- assemblies [Visual Basic], namespaces
- references [Visual Basic], assembly
- namespaces [Visual Basic], assemblies
- referencing assemblies
- Imports statement [Visual Basic], referencing assemblies
- assemblies [Visual Basic], references
ms.assetid: 38149bd4-0a6f-4b31-b5f8-94a8c33f1600
ms.openlocfilehash: 13f250e1b015e5a821da83e557033bc878a8a3b8
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91097899"
---
# <a name="references-and-the-imports-statement-visual-basic"></a>引用和 Imports 语句 (Visual Basic)

通过选择 "**项目**" 菜单上的 "**添加引用**" 命令，可以使外部对象对项目可用。 Visual Basic 中的引用可指向程序集，这些程序集类似于类型库，但包含更多信息。  
  
## <a name="the-imports-statement"></a>Imports 语句  

 程序集包括一个或多个命名空间。 添加对程序集的引用时，还可以向模块添加一个 `Imports` 语句，该模块控制该程序集的命名空间在模块中的可见性。 `Imports`语句提供了一个范围上下文，该上下文允许只使用提供唯一引用所需的命名空间部分。  
  
 `Imports`语句具有以下语法：  
  
 `Imports [Aliasname =] Namespace`  
  
 `Aliasname` 引用可以在代码中用来引用导入的命名空间的短名称。 `Namespace` 是通过项目引用、项目中的定义或通过上一条语句提供的命名空间 `Imports` 。  
  
 模块可以包含任意多个 `Imports` 语句。 它们必须出现在任何 `Option` 语句之后（如果存在），但在任何其他代码之前。  
  
> [!NOTE]
> 不要将项目引用与 `Imports` 语句或 `Declare` 语句混淆。 项目引用使外部对象（如程序集中的对象）可用于 Visual Basic 项目。 `Imports`语句用于简化对项目引用的访问，但不提供对这些对象的访问权限。 `Declare`语句用于声明对动态链接库中的外部过程的引用 (DLL) 。  
  
## <a name="using-aliases-with-the-imports-statement"></a>在 Imports 语句中使用别名  

 使用 `Imports` 语句，无需显式键入引用的完全限定名，就能更轻松地访问类的方法。 使用别名可以为一个命名空间的一部分分配更友好的名称。 例如，在多行上显示一段文本的回车/换行符序列是 <xref:Microsoft.VisualBasic.ControlChars> 命名空间中的模块的一部分 <xref:Microsoft.VisualBasic?displayProperty=nameWithType> 。 若要在不带别名的程序中使用此常量，需要键入以下代码：  
  
 [!code-vb[VbVbalrApplication#3](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrApplication/VB/Class1.vb#3)]  
  
 `Imports` 语句必须始终是紧跟在模块中的任何语句之后的第一行 `Option` 。 下面的代码段演示了如何导入和分配模块的别名 <xref:Microsoft.VisualBasic.ControlChars?displayProperty=nameWithType> ：  
  
 [!code-vb[VbVbalrApplication#4](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrApplication/VB/Class1.vb#4)]  
  
 以后引用此命名空间可能会大大缩短：  
  
 [!code-vb[VbVbalrApplication#5](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrApplication/VB/Class1.vb#5)]  
  
 如果 `Imports` 语句不包含别名，则在导入的命名空间中定义的元素无需限定即可在模块中使用。 如果指定了别名，则必须将其用作该命名空间中包含的名称的限定符。  
  
## <a name="see-also"></a>请参阅

- <xref:Microsoft.VisualBasic.ControlChars>
- <xref:Microsoft.VisualBasic>
- [Visual Basic 中的命名空间](namespaces.md)
- [.NET 中的程序集](../../../standard/assembly/index.md)
- [Imports 语句（.NET 命名空间和类型）](../../language-reference/statements/imports-statement-net-namespace-and-type.md)
