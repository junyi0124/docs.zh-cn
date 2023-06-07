---
title: 如何：控制变量的可用性
ms.date: 07/20/2015
helpviewer_keywords:
- access levels, declared elements
- Private keyword [Visual Basic], accessing variables
- access levels, variables
- Public keyword [Visual Basic], accessing variables
- Friend keyword [Visual Basic], accessing variables
- variables [Visual Basic], access level
- declared elements [Visual Basic], access level
- Protected keyword [Visual Basic], accessing variables
ms.assetid: eaf4f073-7922-43ce-ae1e-90ff376ae947
ms.openlocfilehash: e6173a0eaa0bf84abb1979711c6df932533c5ce9
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91086109"
---
# <a name="how-to-control-the-availability-of-a-variable-visual-basic"></a>如何：控制变量的可用性 (Visual Basic)

可以通过指定变量的 *访问级别*来控制变量的可用性。 访问级别确定哪些代码有权读取或写入变量。  
  
- *成员变量* (在模块级和任何过程外部定义) 默认为公共访问权限，这意味着可查看它们的任何代码都可以访问这些变量。 可以通过指定访问修饰符来更改此。  
  
-  (在过程中定义的*本地变量*) 通常具有公共访问权限，但只有其过程中的代码可以访问它们。 不能更改本地变量的访问级别，但可以更改包含它的过程的访问级别。  
  
 有关详细信息，请参阅 [Visual Basic 中的访问级别](access-levels.md)。  
  
## <a name="private-and-public-access"></a>私有和公共访问  
  
#### <a name="to-make-a-variable-accessible-only-from-within-its-module-class-or-structure"></a>仅允许从其模块、类或结构内部访问变量  
  
1. 将变量的 [Dim 语句](../../../language-reference/statements/dim-statement.md) 放入模块、类或结构中，但放在任何过程之外。  
  
2. 在语句中包含 [Private](../../../language-reference/modifiers/private.md) 关键字 `Dim` 。  
  
     可以从模块、类或结构中的任何位置读取或写入变量，但不能从外部读取或写入。  
  
#### <a name="to-make-a-variable-accessible-from-any-code-that-can-see-it"></a>使变量可从可查看它的任何代码进行访问  
  
1. 对于成员变量，请将 `Dim` 变量的语句放置在模块、类或结构中，但放在任何过程之外。  
  
2. 在语句中包含 [Public](../../../language-reference/modifiers/public.md) 关键字 `Dim` 。  
  
     可以从与程序集互操作的任何代码读取或写入变量。  
  
 \- 或 -  
  
1. 对于局部变量，请将变量的 `Dim` 语句放置到过程内。  
  
2. 不要 `Public` 在语句中包含关键字 `Dim` 。  
  
     可以从过程中的任何位置读取或写入变量，但不能从外部读取或写入。  
  
## <a name="protected-and-friend-access"></a>受保护和友元访问  

 可以将变量的访问级别限制为其类和派生类，或限制为其程序集。 您还可以指定这些限制的联合，这允许从任何派生类中的代码或同一程序集中的任何其他位置进行访问。 通过组合 `Protected` 同一声明中的和关键字来指定此联合 `Friend` 。  
  
#### <a name="to-make-a-variable-accessible-only-from-within-its-class-and-any-derived-classes"></a>使变量只能从其类和任何派生类中访问  
  
1. 将 `Dim` 变量的语句放置在类中，但将其放置在任何过程之外。  
  
2. 在语句中包含 [Protected](../../../language-reference/modifiers/protected.md) 关键字 `Dim` 。  
  
     可以从类中的任何位置以及从派生的任何类（而不是从派生链中的任何类）读取或写入变量。  
  
#### <a name="to-make-a-variable-accessible-only-from-within-the-same-assembly"></a>使变量只能从同一程序集内访问  
  
1. 将变量的语句放入 `Dim` 模块、类或结构中，但放在任何过程之外。  
  
2. 在语句中包含 [Friend](../../../language-reference/modifiers/friend.md) 关键字 `Dim` 。  
  
     你可以从模块、类或结构中的任何位置读取或写入变量，也可以从同一程序集中的任何代码读取或写入到程序集之外的任何代码。  
  
## <a name="example"></a>示例  

 下面的示例演示具有 `Public` 、 `Protected` 、、 `Friend` `Protected Friend` 和 `Private` 访问级别的变量的声明。 请注意，如果 `Dim` 语句指定了访问级别，则不需要包括 `Dim` 关键字。  
  
```vb  
Public Class classForEverybody  
Protected Class classForMyHeirs  
Friend stringForThisProject As String  
Protected Friend stringForProjectAndHeirs As String  
Private numberForMeOnly As Integer  
```  
  
## <a name="net-framework-security"></a>.NET Framework 安全性  

 变量访问级别的限制性越多，恶意代码使用它的可能性就越小。  
  
## <a name="see-also"></a>请参阅

- [Visual Basic 中的访问级别](access-levels.md)
- [Dim 语句](../../../language-reference/statements/dim-statement.md)
- [公共](../../../language-reference/modifiers/public.md)
- [避免](../../../language-reference/modifiers/protected.md)
- [Friend](../../../language-reference/modifiers/friend.md)
- [专用](../../../language-reference/modifiers/private.md)
