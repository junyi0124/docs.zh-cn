---
title: 重载决策
ms.date: 07/20/2015
helpviewer_keywords:
- Visual Basic code, procedures
- overload resolution
- procedures [Visual Basic], overloading
- procedures [Visual Basic], calling
- procedure overloading [Visual Basic], overload resolution
- signatures [Visual Basic], procedure
- overloads [Visual Basic], resolution
ms.assetid: 766115d1-4352-45fb-859f-6063e0de0ec0
ms.openlocfilehash: 9b83eba8efc8dfe14b6ec1cbab270984977198e5
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91071359"
---
# <a name="overload-resolution-visual-basic"></a>重载决策 (Visual Basic)

当 Visual Basic 编译器遇到多个重载版本中定义的过程时，编译器必须确定要调用的重载。 它通过执行以下步骤来执行此操作：  
  
1. **辅助功能。** 它消除了具有访问级别的任何重载，使调用代码无法调用它。  
  
2. **参数的数目。** 它消除了任何定义与调用中提供的参数数量不同的参数的重载。  
  
3. **参数数据类型。** 编译器使实例方法优先于扩展方法。 如果找到了只需要扩大转换才能匹配过程调用的任何实例方法，则将删除所有扩展方法，并且编译器仅以实例方法候选继续。 如果未找到此类实例方法，则它将继续执行实例和扩展方法。  
  
     在此步骤中，它将消除调用参数的数据类型无法转换为重载中定义的参数类型的任何重载。  
  
4. **收缩转换。** 它消除了任何需要从调用参数类型到已定义参数类型的收缩转换的重载。 无论类型检查开关 ([Option Strict 语句](../../../language-reference/statements/option-strict-statement.md)) 为或，都是 `On` 如此 `Off` 。  
  
5. **最小扩大。** 编译器将其余重载视为成对。 对于每个对，它将比较已定义参数的数据类型。 如果其中一个重载中的类型扩大到另一个重载中的相应类型，则编译器将消除后者。 也就是说，它会保留要求最小扩大量的重载。  
  
6. **单个候选项。** 它继续考虑成对重载，直到只保留一个重载，并解析对该重载的调用。 如果编译器无法将重载减少到单个候选项，则会生成错误。  
  
 下图显示了确定要调用的一组重载版本的过程。  
  
 ![重载解析过程的流程图](./media/overload-resolution/determine-overloaded-version.gif "在重载版本之间进行解析")
  
 下面的示例演示了此重载决策过程。  
  
 [!code-vb[VbVbcnProcedures#62](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#62)]  
  
 [!code-vb[VbVbcnProcedures#63](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#63)]  
  
 在第一次调用中，编译器将消除第一次重载，因为)  (第一个参数的类型会 `Short` 缩小到相应参数的类型 (`Byte`) 。 然后，它将消除第三个重载，因为第二个重载中的每个参数类型都将 (`Short` 并 `Single`) 扩大到第三个重载 (和) 中的相应类型 `Integer` `Single` 。 第二个重载需要更少的扩展，因此编译器将其用于调用。  
  
 在第二次调用中，编译器无法根据收缩消除任何重载。 它消除第三个重载的原因与第一次调用中的相同原因，因为它可以通过更少的参数类型来调用第二个重载。 但编译器无法在第一个和第二个重载之间解析。 每个都有一个定义的参数类型，该类型扩大到其他 (中的相应类型 `Byte` `Short` ，但 `Single` 要 `Double`) 。 因此，编译器将生成重载决策错误。  
  
## <a name="overloaded-optional-and-paramarray-arguments"></a>重载的可选参数和 ParamArray 参数  

 如果过程的两个重载具有相同的签名，则除了最后一个参数在另[一个中声明](../../../language-reference/modifiers/paramarray.md)为[可选](../../../language-reference/modifiers/optional.md)外，编译器将解析对该过程的调用，如下所示：  
  
|如果调用提供了最后一个参数|编译器解析对声明最后一个参数的重载的调用|  
|---|---|  
|未省略值 (参数) |`Optional`|  
|单个值|`Optional`|  
|以逗号分隔的列表中的两个或多个值|`ParamArray`|  
|包含空数组 (任意长度的数组) |`ParamArray`|  
  
## <a name="see-also"></a>请参阅

- [可选参数](./optional-parameters.md)
- [参数数组](./parameter-arrays.md)
- [过程重载](./procedure-overloading.md)
- [过程疑难解答](./troubleshooting-procedures.md)
- [如何：定义一个过程的多个版本](./how-to-define-multiple-versions-of-a-procedure.md)
- [如何：调用重载过程](./how-to-call-an-overloaded-procedure.md)
- [如何：重载带有可选参数的过程](./how-to-overload-a-procedure-that-takes-optional-parameters.md)
- [如何：重载参数数量不确定的过程](./how-to-overload-a-procedure-that-takes-an-indefinite-number-of-parameters.md)
- [重载过程注意事项](./considerations-in-overloading-procedures.md)
- [重载](../../../language-reference/modifiers/overloads.md)
- [扩展方法](./extension-methods.md)
