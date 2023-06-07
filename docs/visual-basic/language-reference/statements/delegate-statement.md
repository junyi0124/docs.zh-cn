---
title: Delegate 语句
ms.date: 07/20/2015
f1_keywords:
- vb.Delegate
helpviewer_keywords:
- delegate keyword [Visual Basic]
- Delegate statement [Visual Basic]
ms.assetid: f799c518-0817-40cc-ad0b-4da846fdba57
ms.openlocfilehash: 29de4c174273c3c6c0d4f0cea1ee6dc254a1339b
ms.sourcegitcommit: d2db216e46323f73b32ae312c9e4135258e5d68e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2020
ms.locfileid: "90866664"
---
# <a name="delegate-statement"></a>Delegate 语句

用于声明委托。 委托是一种引用类型，引用类型的 `Shared` 方法或对象的实例方法。 任何具有匹配参数和返回类型的过程都可以用于创建此委托类的实例。 稍后可以通过委托实例调用该过程。  
  
## <a name="syntax"></a>语法  
  
```vb  
[ <attrlist> ] [ accessmodifier ] _  
[ Shadows ] Delegate [ Sub | Function ] name [( Of typeparamlist )] [([ parameterlist ])] [ As type ]  
```  
  
## <a name="parts"></a>组成部分  
  
|术语|定义|  
|---|---|  
|`attrlist`|可选。 应用于此委托的特性的列表。 用逗号分隔多个属性。 必须将 [属性列表](attribute-list.md) 用尖括号括起来 ( " `<` " 和 " `>` " ) 。|  
|`accessmodifier`|可选。 指定哪些代码可以访问委托。 可以是以下值之一：<br /><br /> - [公共](../modifiers/public.md)。 可以访问声明委托的元素的任何代码都可以访问它。<br />-   [受保护](../modifiers/protected.md)。 只有委托的类或派生类中的代码可以访问它。<br />-   [友元](../modifiers/friend.md)。 只有同一程序集中的代码才能访问该委托。<br />- [私有](../modifiers/private.md)。 只有声明委托的元素中的代码才能访问它。<br /><br /> - [受保护的朋友](../modifiers/protected-friend.md) 只有委托的类、派生类或同一程序集内的代码可以访问该委托。 <br />- [私有受保护](../modifiers/private-protected.md) 只有委托的类中的代码或同一程序集中的派生类中的代码才能访问该委托。 |  
|`Shadows`|可选。 指示此委托重新声明并隐藏基类中具有相同名称的编程元素或重载元素集。 可以与任何其他类型一起隐藏任何类型的已声明元素。<br /><br /> 隐藏的元素不可在隐藏它的派生类中使用（除了从隐藏元素不可访问的位置）。 例如，如果某个 `Private` 元素隐藏了一个基类元素，则没有访问该元素的权限的代码将 `Private` 改为访问该基类元素。|  
|`Sub`|可选，但 `Sub` `Function` 必须出现或。 将此过程声明为不 `Sub` 返回值的委托过程。|  
|`Function`|可选，但 `Sub` `Function` 必须出现或。 将此过程声明为一个 `Function` 返回值的委托过程。|  
|`name`|必需。 委托类型的名称;遵循标准变量命名约定。|  
|`typeparamlist`|可选。 此委托的类型参数列表。 多个类型参数之间用逗号分隔。 根据需要，可以使用 `In` 和泛型修饰符来声明每个类型参数 `Out` 。 必须用括号将 [类型列表](type-list.md) 括起来，并在其中引入 `Of` 关键字。|  
|`parameterlist`|可选。 调用时传递给过程的参数的列表。 必须将 [参数列表](parameter-list.md) 括在括号中。|  
|`type`|如果指定过程，则为必需 `Function` 。 返回值的数据类型。|  
  
## <a name="remarks"></a>备注  

 `Delegate`语句定义委托类的参数和返回类型。 任何具有匹配参数和返回类型的过程都可以用于创建此委托类的实例。 然后，可以通过调用委托的方法，稍后通过委托实例调用该过程 `Invoke` 。  
  
 委托可以在命名空间、模块、类或结构级别进行声明，但不能在过程中声明。  
  
 每个委托类均可定义将对象方法指定传递到的构造函数。 委托构造函数的自变量必须是对方法的引用或 lambda 表达式。  
  
 若要指定对方法的引用，请使用以下语法：  
  
 `AddressOf` [`expression`.]`methodname`  
  
 `expression` 的编译时类型必须是类或接口的名称，此类或接口包含指定名称的方法，其签名与委托类的签名一致。 `methodname` 可以是共享方法，也可以是实例方法。 即使为类的默认方法创建委托，`methodname` 也不是可选的。  
  
 若要指定 lambda 表达式，请使用以下语法：  
  
 `Function` ([`parm` As `type`, `parm2` As `type2`, ...]) `expression`  
  
 函数的签名必须与委托类型的签名一致。 有关 lambda 表达式的详细信息，请参阅 [Lambda 表达式](../../programming-guide/language-features/procedures/lambda-expressions.md)。  
  
 有关委托的详细信息，请参阅[委托](../../programming-guide/language-features/delegates/index.md)。  
  
## <a name="example"></a>示例  

 下面的示例使用 `Delegate` 语句声明一个用于对两个数字进行操作并返回一个数字的委托。 `DelegateTest`方法获取此类型的委托的一个实例，并使用该实例对一对数字进行操作。  
  
 [!code-vb[VbVbalrDelegates#14](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrDelegates/VB/Class1.vb#14)]  
  
## <a name="see-also"></a>另请参阅

- [AddressOf 运算符](../operators/addressof-operator.md)
- [个](of-clause.md)
- [委托](../../programming-guide/language-features/delegates/index.md)
- [如何：使用泛型类](../../programming-guide/language-features/data-types/how-to-use-a-generic-class.md)
- [Generic Types in Visual Basic](../../programming-guide/language-features/data-types/generic-types.md)
- [协变和逆变](../../programming-guide/concepts/covariance-contravariance/index.md)
- [位于](../modifiers/in-generic-modifier.md)
- [弄](../modifiers/out-generic-modifier.md)
