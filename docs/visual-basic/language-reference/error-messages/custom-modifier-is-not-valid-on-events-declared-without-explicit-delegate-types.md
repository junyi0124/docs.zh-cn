---
title: “Custom”修饰符在未用显式委托类型声明的事件上无效
ms.date: 07/20/2015
f1_keywords:
- vbc31122
- bc31122
helpviewer_keywords:
- BC31122
ms.assetid: 6911f0d1-641a-473b-906d-8ee5681194be
ms.openlocfilehash: a2277e3778c2c252fd4b1eaeb033d549f041c15c
ms.sourcegitcommit: ff5a4eb5cffbcac9521bc44a907a118cd7e8638d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2020
ms.locfileid: "92160628"
---
# <a name="bc31122-custom-modifier-is-not-valid-on-events-declared-without-explicit-delegate-types"></a>BC31122： "Custom" 修饰符在没有显式委托类型的声明事件上无效

与非自定义事件不同， `Custom Event` 声明在 `As` 事件名称后需要一个子句，该事件名称显式指定事件的委托类型。

 可使用 `As` 子句和显式委托类型定义非自定义事件，或使用紧跟在事件名称后面的参数列表定义。

 **错误 ID：** BC31122

## <a name="to-correct-this-error"></a>更正此错误

1. 定义与自定义事件具有相同参数列表的委托。

     例如，如果定义了 `Custom Event` `Custom Event Test(ByVal sender As Object, ByVal i As Integer)` ，则相应的委托将如下所示。

     [!code-vb[VbVbalrEventError#18](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrEventError/VB/VbVbalrEventError.vb#18)]

2. 将自定义事件的参数列表替换为 `As` 指定委托类型的子句。

     继续此示例，将 `Custom Event` 按如下所示重写声明。

     [!code-vb[VbVbalrEventError#19](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrEventError/VB/VbVbalrEventError.vb#19)]

## <a name="example"></a>示例

 此示例声明一个 `Custom Event` ，并指定 `As` 具有委托类型的必需子句。

 [!code-vb[VbVbalrEventError#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrEventError/VB/VbVbalrEventError.vb#2)]

## <a name="see-also"></a>另请参阅

- [Event 语句](../statements/event-statement.md)
- [Delegate 语句](../statements/delegate-statement.md)
- [事件](../../programming-guide/language-features/events/index.md)
