---
title: AddressOf 运算符
ms.date: 07/20/2015
f1_keywords:
- AddressOf
- vb.AddressOf
helpviewer_keywords:
- AddressOf operator [Visual Basic]
- addresses, passing to API procedures
ms.assetid: 8105a59d-60d8-4ab5-b221-5899cdfacbf4
ms.openlocfilehash: edce7d4a2268bd311045ea4972672fe8fd2600ea
ms.sourcegitcommit: d2db216e46323f73b32ae312c9e4135258e5d68e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2020
ms.locfileid: "90874894"
---
# <a name="addressof-operator-visual-basic"></a>AddressOf 运算符 (Visual Basic)

创建引用特定过程的委托实例。  
  
## <a name="syntax"></a>语法  
  
```vb  
AddressOf procedurename  
```  
  
## <a name="parts"></a>组成部分  

 `procedurename`  
 必需。 指定新创建的委托所引用的过程。  
  
## <a name="remarks"></a>备注  

 `AddressOf`运算符创建指向由指定的 sub 或函数的委托 `procedurename` 。 如果指定的过程是实例方法，则委托同时引用实例和方法。 然后，在调用委托时，将调用指定实例的指定方法。  
  
 `AddressOf`运算符可用作委托构造函数的操作数，或可用于可由编译器确定委托的类型的上下文中。  
  
## <a name="example"></a>示例  

 此示例使用 `AddressOf` 运算符来指定用于处理 `Click` 按钮事件的委托。  
  
 [!code-vb[VbVbalrDelegates#8](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrDelegates/VB/Class1.vb#8)]  
  
## <a name="example"></a>示例  

 下面的示例使用 `AddressOf` 运算符来指定线程的启动函数。  
  
 [!code-vb[VbVbalrDelegates#9](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrDelegates/VB/Class1.vb#9)]  
  
## <a name="see-also"></a>另请参阅

- [Declare Statement](../statements/declare-statement.md)
- [Function 语句](../statements/function-statement.md)
- [Sub 语句](../statements/sub-statement.md)
- [委托](../../programming-guide/language-features/delegates/index.md)
