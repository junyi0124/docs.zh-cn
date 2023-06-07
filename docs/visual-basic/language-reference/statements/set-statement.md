---
title: Set 语句
ms.date: 07/20/2015
f1_keywords:
- vb.Set
helpviewer_keywords:
- property procedures [Visual Basic], Set statements
- Set statement [Visual Basic]
- Set statement [Visual Basic], syntax
- write-only properties
- properties [Visual Basic], write-only
ms.assetid: 9ecc27b4-df84-420d-9075-db25455fb3cd
ms.openlocfilehash: b3524769567a56a87184bf916a3e5ccb1fd4fa1c
ms.sourcegitcommit: d2db216e46323f73b32ae312c9e4135258e5d68e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2020
ms.locfileid: "90871759"
---
# <a name="set-statement-visual-basic"></a>Set 语句 (Visual Basic)

声明 `Set` 用于向属性赋值的属性过程。  
  
## <a name="syntax"></a>语法  
  
```vb  
[ <attributelist> ] [ accessmodifier ] Set (ByVal value [ As datatype ])  
    [ statements ]  
End Set  
```  
  
## <a name="parts"></a>组成部分  

 `attributelist`  
 可选。 请参阅 [特性列表](attribute-list.md)。  
  
 `accessmodifier`  
 在 `Get` 此属性中的最多一个和语句上是可选的 `Set` 。 可以是以下值之一：  
  
- [避免](../modifiers/protected.md)  
  
- [Friend](../modifiers/friend.md)  
  
- [专用](../modifiers/private.md)  
  
- `Protected Friend`  
  
 请参阅 [Visual Basic 中的访问级别](../../programming-guide/language-features/declared-elements/access-levels.md)。  
  
 `value`  
 必需。 包含属性新值的参数。  
  
 `datatype`  
 如果 `Option Strict` 为，则为必需 `On` 。 参数的数据类型 `value` 。 指定的数据类型必须与声明此语句的属性的数据类型相同 `Set` 。  
  
 `statements`  
 可选。 调用属性过程时运行的一个或多个语句 `Set` 。  
  
 `End Set`  
 必需。 终止属性过程的定义 `Set` 。  
  
## <a name="remarks"></a>备注  

 除非将属性标记为，否则每个属性都必须具有 `Set` 属性过程 `ReadOnly` 。 此 `Set` 过程用于设置属性的值。  
  
 `Set`当赋值语句提供要存储在属性中的值时，Visual Basic 会自动调用该属性的过程。  
  
 Visual Basic `Set` 在属性分配过程中将参数传递给过程。 如果未提供参数 `Set` ，则集成开发环境 (IDE) 使用名为的隐式参数 `value` 。 参数保留要赋给属性的值。 通常将此值存储在私有本地变量中，并在 `Get` 调用过程时返回。  
  
 属性声明的主体只能在 `Get` `Set` [property 语句](property-statement.md) 和语句之间包含属性的和过程 `End Property` 。 它无法存储这些过程以外的任何内容。 特别是，它无法存储属性的当前值。 您必须将此值存储在属性的外部，因为如果将其存储在任一属性过程中，则其他属性过程将无法访问它。 常见的方法是将值存储在与属性在同一级别上声明的 [私有](../modifiers/private.md) 变量中。 必须在应用的 `Set` 属性中定义一个过程。  
  
 此 `Set` 过程默认为其包含属性的访问级别，除非你 `accessmodifier` 在语句中使用 `Set` 。  
  
## <a name="rules"></a>规则  
  
- **混合访问级别。** 如果要定义读写属性，则可以选择为或过程指定不同的访问级别 `Get` `Set` ，但不能同时指定两者。 如果执行此操作，则过程访问级别必须比属性的访问级别更严格。 例如，如果声明了属性 `Friend` ，则可以声明该 `Set` 过程 `Private` ，而不是 `Public` 。  
  
     如果要定义 `WriteOnly` 属性，则该 `Set` 过程表示整个属性。 不能为声明不同的访问级别 `Set` ，因为这会为属性设置两个访问级别。  
  
## <a name="behavior"></a>行为  
  
- **从属性过程返回。** 当 `Set` 过程返回到调用代码时，执行将在提供要存储的值的语句之后继续执行。  
  
     `Set` 属性过程可以使用 [Return 语句](return-statement.md) 或 [Exit 语句](exit-statement.md)返回。  
  
     `Exit Property`和 `Return` 语句导致直接从属性过程退出。 任意数量的 `Exit Property` 和 `Return` 语句可以出现在过程中的任何位置，并且可以混合使用 `Exit Property` 和 `Return` 语句。  
  
## <a name="example"></a>示例  

 下面的示例使用 `Set` 语句设置属性的值。  
  
 [!code-vb[VbVbalrStatements#55](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#55)]  
  
## <a name="see-also"></a>另请参阅

- [Get 语句](get-statement.md)
- [Property Statement](property-statement.md)
- [Sub 语句](sub-statement.md)
- [Property 过程](../../programming-guide/language-features/procedures/property-procedures.md)
