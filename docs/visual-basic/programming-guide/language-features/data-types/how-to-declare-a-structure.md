---
title: 如何：声明结构
ms.date: 07/20/2015
helpviewer_keywords:
- declarations [Visual Basic], structures
- structure statements [Visual Basic]
- statements [Visual Basic], structure
- structures [Visual Basic], declaring
ms.assetid: d5e98381-eb81-47d4-af83-48cc534a2572
ms.openlocfilehash: bffdc5974eff6b71e0abc4780a61aa300769eed6
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91058542"
---
# <a name="how-to-declare-a-structure-visual-basic"></a>如何：声明结构 (Visual Basic)

使用 [Structure 语句](../../../language-reference/statements/structure-statement.md)开始结构声明，并使用 `End Structure` 语句结束。 在这两个语句之间必须至少声明一个 *元素*。 元素可以是任何数据类型，但必须至少有一个是非共享变量或非共享、非自定义事件。  
  
 不能在结构声明中初始化任何结构元素。 将变量声明为结构类型时，可以通过变量访问元素，为这些元素赋值。  
  
 有关结构和类之间的差异的讨论，请参阅 [结构和类](structures-and-classes.md)。  
  
 出于演示目的，请考虑要跟踪员工姓名、电话分机和薪金的情况。 结构允许在单个变量中执行此操作。  
  
### <a name="to-declare-a-structure"></a>声明结构  
  
1. 为结构创建开始语句和结束语句。  
  
     您可以使用 [Public](../../../language-reference/modifiers/public.md)、 [Protected](../../../language-reference/modifiers/protected.md)、 [Friend](../../../language-reference/modifiers/friend.md)或 [Private](../../../language-reference/modifiers/private.md) 关键字指定结构的访问级别，也可以让它默认为 `Public` 。  
  
    ```vb  
    Private Structure employee  
    End Structure  
    ```  
  
2. 向结构的主体中添加元素。  
  
     结构中必须至少有一个元素。 必须声明每个元素并为其指定访问级别。 如果使用不带任何关键字的 [Dim 语句](../../../language-reference/statements/dim-statement.md) ，可访问性默认为 `Public` 。  
  
    ```vb  
    Private Structure employee  
        Public givenName As String  
        Public familyName As String  
        Public phoneExtension As Long  
        Private salary As Decimal  
        Public Sub giveRaise(raise As Double)  
            salary *= raise  
        End Sub  
        Public Event salaryReviewTime()  
    End Structure  
    ```  
  
     `salary`前面的示例中的字段是 `Private` ，这意味着它在结构外部不可访问，甚至是从包含类中访问。 但是，该 `giveRaise` 过程是 `Public` ，因此可以从结构外部调用它。 同样，您可以 `salaryReviewTime` 从结构外部引发事件。  
  
     除了变量、 `Sub` 过程和事件外，还可以在结构中定义常量、 `Function` 过程和属性。 最多可以将一个属性指定为 *默认属性*，前提是它至少采用一个参数。 您可以使用共享过程处理事件[Shared](../../../language-reference/modifiers/shared.md) `Sub` 。 有关详细信息，请参阅 [如何：在 Visual Basic 中声明和调用默认属性](../procedures/how-to-declare-and-call-a-default-property.md)。  
  
## <a name="see-also"></a>请参阅

- [数据类型](index.md)
- [基本数据类型](elementary-data-types.md)
- [复合数据类型](composite-data-types.md)
- [Value Types and Reference Types](value-types-and-reference-types.md)
- [结构](structures.md)
- [数据类型疑难解答](troubleshooting-data-types.md)
- [结构变量](structure-variables.md)
- [结构和其他编程元素](structures-and-other-programming-elements.md)
- [结构和类](structures-and-classes.md)
- [用户定义的数据类型](../../../language-reference/data-types/user-defined-data-type.md)
