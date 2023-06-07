---
title: 条件编译
ms.date: 07/20/2015
helpviewer_keywords:
- conditional compilation [Visual Basic], about conditional compilation
- compilation [Visual Basic], conditional
ms.assetid: 9c35e55e-7eee-44fb-a586-dad1f1884848
ms.openlocfilehash: e59296882edc018259816c73b6ae861b3b296783
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91098964"
---
# <a name="conditional-compilation-in-visual-basic"></a>Visual Basic 中的条件编译

在 *条件编译*中，程序中的特定代码块会有选择地编译，而其他代码块会被忽略。  
  
 例如，你可能想要编写调试语句来比较不同方法对相同编程任务的速度，或者可能想要针对多种语言对应用程序进行本地化。 条件编译语句设计为在编译时运行，而不是在运行时运行。  
  
 您可以用指令指示要有条件地编译的代码块 `#If...Then...#Else` 。 例如，若要从相同的源代码创建法语和德语版本的同一应用程序，可以 `#If...Then` 使用预定义的常量和将平台特定的代码段嵌入到语句中 `FrenchVersion` `GermanVersion` 。 下面的示例演示如何执行以下操作：  
  
 [!code-vb[VbVbalrConditionalComp#5](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrConditionalComp/VB/Class1.vb#5)]  
  
 如果在 `FrenchVersion` 编译时将条件编译常量的值设置为 `True` ，则编译法语版本的条件代码。 如果将常量的值设置 `GermanVersion` 为 `True` ，则编译器将使用德语版本。 如果两者均未设置为 `True` ，则最后一个块中的代码将 `Else` 运行。  
  
> [!NOTE]
> 当编辑代码时，自动完成功能将无法正常工作，如果代码不是当前分支的一部分，则使用条件编译指令。  
  
## <a name="declaring-conditional-compilation-constants"></a>声明条件编译常量  

 可以通过以下三种方式之一来设置条件编译常量：  
  
- 在**项目设计器**中  
  
- 使用命令行编译器时在命令行上  
  
- 在代码中  
  
 条件编译常量具有特殊作用域，不能从标准代码进行访问。 条件编译常量的作用域取决于其设置方式。 下表列出了使用上述三种方法声明的常量的作用域。  
  
|如何设置常量|常量范围|  
|---|---|  
|**项目设计器**|公共到项目中的所有文件|  
|命令行|向传递到命令行编译器的所有文件公开|  
|`#Const` 代码中的语句|专用于声明它的文件|  
  
|在项目设计器中设置常量|  
|---|  
|-在创建可执行文件之前，请按照[管理项目和解决方案属性](/visualstudio/ide/managing-project-and-solution-properties)中提供的步骤在 "**项目设计器**" 中设置常量。|  
  
|在命令行中设置常量|  
|---|  
|-使用 **-d** 开关输入条件编译常量，如以下示例中所示：<br />     `vbc MyProj.vb /d:conFrenchVersion=–1:conANSI=0`<br />     **-D**开关和第一个常数之间不需要空格。 有关详细信息，请参阅 [-定义 (Visual Basic) ](../../reference/command-line-compiler/define.md)。<br />     命令行声明会重写在 **项目设计器**中输入的声明，但不会将其删除。 在 **项目设计器** 中设置的参数对于后续编译仍有效。<br />     在代码本身中编写常量时，没有严格的规则与它们的位置相关，因为它们的作用域是在其中声明它们的整个模块。|  
  
|若要在代码中设置常量|  
|---|  
|-将常量放置在使用它们的模块的声明块中。 这有助于保持代码的组织和可读性。|  
  
## <a name="related-topics"></a>相关主题  
  
|Title|描述|  
|---|---|  
|[程序结构和代码约定](program-structure-and-code-conventions.md)|提供使代码易于阅读和维护的建议。|  
  
## <a name="reference"></a>参考  

 [#Const 指令](../../language-reference/directives/const-directive.md)  
  
 [#If...Then...#Else 指令](../../language-reference/directives/if-then-else-directives.md)  
  
 [-define (Visual Basic)](../../reference/command-line-compiler/define.md)
