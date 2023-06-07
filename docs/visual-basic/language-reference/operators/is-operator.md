---
title: Is 运算符
ms.date: 07/20/2015
f1_keywords:
- vb.is
helpviewer_keywords:
- comparison operators [Visual Basic]
- equivalent objects
- TypeOf...Is expression
- Is operator [Visual Basic]
ms.assetid: 8045a6c8-2a83-45b6-ad47-d09a704c656d
ms.openlocfilehash: 1c2d87ef0a8202332c87af552f488d652c400213
ms.sourcegitcommit: 9c45035b781caebc63ec8ecf912dc83fb6723b1f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/25/2020
ms.locfileid: "88812258"
---
# <a name="is-operator-visual-basic"></a>Is 运算符 (Visual Basic) 

比较两个对象引用变量。

## <a name="syntax"></a>语法

```vb
result = object1 Is object2
```

## <a name="parts"></a>组成部分

 `result`  
 必需。 任何 `Boolean` 值。  
  
 `object1`  
 必需。 任意 `Object` 名称。  
  
 `object2`  
 必需。 任意 `Object` 名称。  
  
## <a name="remarks"></a>注解

`Is`运算符确定两个对象引用是否引用同一对象。 但是，它不会执行值比较。 如果 `object1` 和 `object2` 都引用完全相同的对象实例，则 `result` 为 `True` ; 如果不是，则为; 如果不是，则 `result` 为 `False` 。

> [!NOTE]
> `Is`关键字还用于[Select .。。Case 语句](../statements/select-case-statement.md)。
  
## <a name="example"></a>示例

下面的示例使用 `Is` 运算符比较对象引用对。 将结果分配给一个 `Boolean` 值，该值表示两个对象是否相同。

[!code-vb[VbVbalrOperators#27](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#27)]

如前面的示例所示，可以使用 `Is` 运算符来测试早期绑定和晚期绑定对象。

## <a name="use-typeof-operator-with-is-operator"></a>对 Is 运算符使用 TypeOf 运算符

`Is` 运算符还可与关键字一起使用 `TypeOf` ，以生成 `TypeOf` ... `Is` 表达式，该表达式测试对象变量是否与数据类型兼容。 例如：

```vb
If TypeOf sender Is Button Then
```

## <a name="see-also"></a>另请参阅

- [TypeOf 运算符](typeof-operator.md)
- [IsNot 运算符](isnot-operator.md)
- [Comparison Operators in Visual Basic](../../programming-guide/language-features/operators-and-expressions/comparison-operators.md)
- [Visual Basic 中的运算符优先级](operator-precedence.md)
- [按功能列出的运算符](operators-listed-by-functionality.md)
- [运算符和表达式](../../programming-guide/language-features/operators-and-expressions/index.md)
