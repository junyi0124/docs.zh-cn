---
title: 如何：控制变量的范围
ms.date: 07/20/2015
helpviewer_keywords:
- variables [Visual Basic], scope
- declared elements [Visual Basic], scope
- visibility [Visual Basic], declared elements
- variables [Visual Basic], visibility
- scope [Visual Basic], declared elements
- scope [Visual Basic], variables
- scope [Visual Basic], Visual Basic
- declared elements [Visual Basic], visibility
- visibility [Visual Basic], variables
ms.assetid: 44b7f62a-cb5c-4d50-bce9-60ae68f87072
ms.openlocfilehash: 2ce7c1700eec54542719e6e0880466ca136e86f6
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91095427"
---
# <a name="how-to-control-the-scope-of-a-variable-visual-basic"></a>如何：控制变量的范围 (Visual Basic)

通常，变量在声明它的整个区域内处于 *范围*内，或者在引用中可见。 在某些情况下，变量的 *访问级别* 会影响其作用域。  
  
 有关详细信息，请参阅 [Scope in Visual Basic](scope.md)。  
  
## <a name="scope-at-block-or-procedure-level"></a>块或过程级别的作用域  
  
#### <a name="to-make-a-variable-visible-only-within-a-block"></a>使变量仅在块中可见  
  
- 将变量的 [Dim 语句](../../../language-reference/statements/dim-statement.md) 置于该块的起始和终止声明语句之间，例如，在 `For` 循环的和语句之间 `Next` `For` 。  
  
     只能从块内引用该变量。  
  
#### <a name="to-make-a-variable-visible-only-within-a-procedure"></a>使变量仅在过程中可见  
  
- 将变量的语句放置在 `Dim` 过程中，但在任何块 (（例如 `With` ... `End With` 块) ）的外部。  
  
     您只能从过程内引用变量，包括过程中包含的任何块。  
  
## <a name="scope-at-module-or-namespace-level"></a>模块或命名空间级别的作用域  

 为方便起见，单术语 *模块级别* 同样适用于模块、类和结构。 模块级别变量的访问级别确定其作用域。 包含模块、类或结构的命名空间还会影响范围。  
  
#### <a name="to-make-a-variable-visible-throughout-a-module-class-or-structure"></a>使变量在整个模块、类或结构中可见  
  
1. 将变量的语句放入 `Dim` 模块、类或结构中，但放在任何过程之外。  
  
2. 在语句中包含 [Private](../../../language-reference/modifiers/private.md) 关键字 `Dim` 。  
  
3. 可以从模块、类或结构中的任何位置引用该变量，而不能从外部引用。  
  
#### <a name="to-make-a-variable-visible-throughout-a-namespace"></a>使变量在整个命名空间中可见  
  
1. 将变量的语句放入 `Dim` 模块、类或结构中，但放在任何过程之外。  
  
2. 在语句中包含 [Friend](../../../language-reference/modifiers/friend.md) 或 [Public](../../../language-reference/modifiers/public.md) 关键字 `Dim` 。  
  
3. 可以从包含模块、类或结构的命名空间中的任何位置引用该变量。  
  
## <a name="example"></a>示例  

 下面的示例在模块级别声明一个变量，并将其可见性限制为模块中的代码。  
  
```vb  
Module demonstrateScope  
    Private strMsg As String  
    Sub initializePrivateVariable()  
        strMsg = "This variable cannot be used outside this module."  
    End Sub  
    Sub usePrivateVariable()  
        MsgBox(strMsg)  
    End Sub  
End Module  
```  
  
 在前面的示例中，模块中定义的所有过程 `demonstrateScope` 都可以引用 `String` 变量 `strMsg` 。 调用此 `usePrivateVariable` 过程时，它将在对话框中显示字符串变量的内容 `strMsg` 。  
  
 对于前面的示例的以下更改，字符串变量 `strMsg` 可以在其声明的命名空间中的任何位置通过代码引用。  
  
```vb  
Public strMsg As String  
```  
  
## <a name="robust-programming"></a>可靠编程  

 变量的作用域越窄，使用同一名称的另一个变量无意引用它就越少机会。 你还可以最大程度地减少引用匹配的问题。  
  
## <a name="net-framework-security"></a>.NET Framework 安全性  

 变量的范围越窄，恶意代码使用它的机会就越小。  
  
## <a name="see-also"></a>请参阅

- [Visual Basic 中的范围](scope.md)
- [Visual Basic 中的生存期](lifetime.md)
- [Visual Basic 中的访问级别](access-levels.md)
- [变量](../variables/index.md)
- [变量声明](../variables/variable-declaration.md)
- [Dim 语句](../../../language-reference/statements/dim-statement.md)
