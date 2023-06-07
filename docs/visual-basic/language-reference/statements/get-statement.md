---
title: Get 语句
ms.date: 07/20/2015
f1_keywords:
- vb.Get
helpviewer_keywords:
- Get statement [Visual Basic], syntax
- Get statement [Visual Basic]
- properties [Visual Basic], read-only
- read-only properties
- Get keyword [Visual Basic]
- property procedures [Visual Basic], Get statements
ms.assetid: 56b05cdc-bd64-4dfd-bb12-824eacec6f94
ms.openlocfilehash: 3da6c099b3f43a144484eaddf58605609eb0bbfe
ms.sourcegitcommit: d2db216e46323f73b32ae312c9e4135258e5d68e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2020
ms.locfileid: "90866208"
---
# <a name="get-statement"></a>Get 语句

声明 `Get` 用于检索属性值的属性过程。  
  
## <a name="syntax"></a>语法  
  
```vb  
[ <attributelist> ] [ accessmodifier ] Get()  
    [ statements ]  
End Get  
```  
  
## <a name="parts"></a>组成部分  
  
|术语|定义|  
|---|---|  
|`attributelist`|可选。 请参阅 [特性列表](attribute-list.md)。|  
|`accessmodifier`|在 `Get` 此属性中的最多一个和语句上是可选的 `Set` 。 可以是以下值之一：<br /><br /> -   [避免](../modifiers/protected.md)<br />-   [友好](../modifiers/friend.md)<br />-   [专有](../modifiers/private.md)<br />-   `Protected Friend`<br /><br /> 请参阅 [Visual Basic 中的访问级别](../../programming-guide/language-features/declared-elements/access-levels.md)。|  
|`statements`|可选。 调用属性过程时运行的一个或多个语句 `Get` 。|  
|`End Get`|必需。 终止属性过程的定义 `Get` 。|  
  
## <a name="remarks"></a>备注  

 除非将属性标记为，否则每个属性都必须具有 `Get` 属性过程 `WriteOnly` 。 此 `Get` 过程用于返回属性的当前值。  
  
 当表达式请求属性值时，Visual Basic 会自动调用该属性的 `Get` 过程。  
  
 属性声明的主体只能在 `Get` `Set` [property 语句](property-statement.md) 和语句之间包含属性的和过程 `End Property` 。 它无法存储这些过程以外的任何内容。 特别是，它无法存储属性的当前值。 您必须将此值存储在属性的外部，因为如果将其存储在任一属性过程中，则其他属性过程将无法访问它。 常见的方法是将值存储在与属性在同一级别上声明的 [私有](../modifiers/private.md) 变量中。 必须在应用的 `Get` 属性中定义一个过程。  
  
 此 `Get` 过程默认为其包含属性的访问级别，除非你 `accessmodifier` 在语句中使用 `Get` 。  
  
## <a name="rules"></a>规则  
  
- **混合访问级别。** 如果要定义读写属性，则可以选择为或过程指定不同的访问级别 `Get` `Set` ，但不能同时指定两者。 如果执行此操作，则过程访问级别必须比属性的访问级别更严格。 例如，如果声明了属性 `Friend` ，则可以声明该 `Get` 过程 `Private` ，而不是 `Public` 。  
  
     如果要定义 `ReadOnly` 属性，则该 `Get` 过程表示整个属性。 不能为声明不同的访问级别 `Get` ，因为这会为属性设置两个访问级别。  
  
- **返回类型。** [Property 语句](property-statement.md)可以声明其返回的值的数据类型。 `Get`过程会自动返回该数据类型。 您可以指定任何数据类型或枚举、结构、类或接口的名称。  
  
     如果 `Property` 语句未指定 `returntype` ，则该过程返回 `Object` 。  
  
## <a name="behavior"></a>行为  
  
- **从过程返回。** 当 `Get` 过程返回到调用代码时，执行将在请求属性值的语句中继续执行。  
  
     `Get` 属性过程可以使用 [Return 语句](return-statement.md) 或通过将返回值分配给属性名称来返回值。 有关详细信息，请参阅 [Function 语句](function-statement.md)中的 "返回值"。  
  
     `Exit Property`和 `Return` 语句导致直接从属性过程退出。 任意数量的 `Exit Property` 和 `Return` 语句可以出现在过程中的任何位置，并且可以混合使用 `Exit Property` 和 `Return` 语句。  
  
- **返回值。** 若要从过程返回值 `Get` ，可将值分配给属性名称，或将其包含在 [return 语句](return-statement.md)中。 `Return`语句同时分配 `Get` 过程返回值并退出该过程。  
  
     如果使用时 `Exit Property` 没有为属性名称赋值，则该过程将 `Get` 返回属性数据类型的默认值。 有关详细信息，请参阅 [Function 语句](function-statement.md)中的 "返回值"。  
  
     下面的示例演示了只读属性 `quoteForTheDay` 可以返回保存在私有变量中的值的两种方法 `quoteValue` 。  
  
     [!code-vb[VbVbalrStatements#27](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#27)]  
  
     [!code-vb[VbVbalrStatements#28](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#28)]  
  
     [!code-vb[VbVbalrStatements#29](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#29)]  
  
## <a name="example"></a>示例  

 下面的示例使用 `Get` 语句返回属性的值。  
  
 [!code-vb[VbVbalrStatements#30](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#30)]  
  
## <a name="see-also"></a>另请参阅

- [Set 语句](set-statement.md)
- [Property Statement](property-statement.md)
- [Exit 语句](exit-statement.md)
- [对象和类](../../programming-guide/language-features/objects-and-classes/index.md)
- [演练：定义类](../../programming-guide/language-features/objects-and-classes/walkthrough-defining-classes.md)
