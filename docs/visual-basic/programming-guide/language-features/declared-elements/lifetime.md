---
title: 生存期
ms.date: 07/20/2015
helpviewer_keywords:
- static variables [Visual Basic], lifetime
- static variables [Visual Basic], Visual Basic
- declared elements [Visual Basic], lifetime
- Shared variable lifetime [Visual Basic]
- lifetime [Visual Basic], declared elements
- lifetime [Visual Basic], Visual Basic
- lifetime [Visual Basic]
ms.assetid: bd91e390-690a-469a-9946-8dca70bc14e7
ms.openlocfilehash: 67fe63eecd2aa0c134682708cdeddb21ba06db12
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91087487"
---
# <a name="lifetime-in-visual-basic"></a>Visual Basic 中的生存期

已声明元素的 *生存期* 是可供使用的时间段。 变量是具有生存期的唯一元素。 出于此目的，编译器将过程参数和函数返回视为变量的特殊情况。 变量的生存期表示它可以包含一个值的时间段。 它的值可以在其生存期内更改，但它始终保存某些值。  
  
## <a name="different-lifetimes"></a>不同生存期  

 在模块级别声明的 *成员变量* (在任何过程) 通常与声明它的元素具有相同的生存期。 在类或结构中声明的非共享变量作为为其声明的类或结构的每个实例的单独副本存在。 每个此类变量与实例的生存期相同。 但是， `Shared` 变量只有单个生存期，即应用程序运行时的整个生存期。  
  
  (在过程中声明的 *局部变量*) 仅在其声明过程正在运行时才存在。 这同样适用于该过程的参数和任何函数返回。 但是，如果该过程调用其他过程，则在调用的过程正在运行时，本地变量将保留其值。  
  
## <a name="beginning-of-lifetime"></a>生存期开头  

 当控件进入声明它的过程时，本地变量的生存期开始。 当过程开始运行时，每个本地变量都将被初始化为其数据类型的默认值。 当过程遇到 `Dim` 指定初始值的语句时，它会将这些变量设置为这些值，即使您的代码已经为它们分配了其他值。  
  
 结构变量的每个成员都初始化为一个单独的变量。 同样，数组变量的每个元素都是单独初始化的。  
  
 在过程中声明的块内声明的变量 (如 `For` 循环) 在进入过程时进行初始化。 无论代码是否执行块，这些初始化都将生效。  
  
## <a name="end-of-lifetime"></a>生存期结束  

 过程终止后，不会保留其局部变量的值，Visual Basic 将回收其内存。 下一次调用该过程时，将创建不用重新并重新初始化其所有局部变量。  
  
 当类或结构的实例终止时，其非共享变量将丢失其内存及其值。 类或结构的每个新实例都创建并重新初始化其非共享变量。 但是， `Shared` 会保留变量，直到应用程序停止运行。  
  
## <a name="extension-of-lifetime"></a>生存期扩展  

 如果使用关键字声明局部变量 `Static` ，则其生存期比其过程的执行时间长。 下表显示了过程声明如何确定变量存在的时间长度 `Static` 。  
  
|过程位置和共享|静态变量生存期开始|静态变量生存期结束|  
|------------------------------------|-------------------------------------|-----------------------------------|  
|默认情况下， (共享模块) |第一次调用该过程时|当应用程序停止运行时|  
|在类中， `Shared` (过程不是实例成员) |第一次在特定实例或类或结构名称本身上调用该过程|当应用程序停止运行时|  
|在类的实例中，不 `Shared` (过程是实例成员) |第一次在特定实例上调用该过程时|在释放实例以进行垃圾回收 (GC) |  
  
## <a name="static-variables-of-the-same-name"></a>具有相同名称的静态变量  

 可以在多个过程中声明具有相同名称的静态变量。 如果这样做，Visual Basic 编译器会将每个此类变量视为一个单独的元素。 其中一个变量的初始化不会影响其他变量的值。 如果使用一组重载定义过程，并在每个重载中声明一个具有相同名称的静态变量，则这一点同样适用。  
  
## <a name="containing-elements-for-static-variables"></a>包含静态变量的元素  

 可以在类中声明静态局部变量，即在该类的过程中。 但是，不能在结构中声明静态局部变量，要么作为结构成员，要么在该结构内作为过程的局部变量。  
  
## <a name="example"></a>示例  
  
### <a name="description"></a>描述  

 下面的示例使用 [Static](../../../language-reference/modifiers/static.md) 关键字声明一个变量。  (注意， `Dim` [Dim 语句](../../../language-reference/statements/dim-statement.md) 使用修饰符（如）时不需要关键字 `Static` 。 )   
  
### <a name="code"></a>代码  

 [!code-vb[VbVbalrKeywords#13](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrKeywords/VB/class7.vb#13)]  
  
### <a name="comments"></a>注释  

 在前面的示例中，该变量将在 `applesSold` 该过程 `runningTotal` 返回到调用代码后继续存在。 下次调用时 `runningTotal` ，将 `applesSold` 保留其以前计算的值。  
  
 如果在未 `applesSold` 使用的情况 `Static` 下声明了，则在对的调用中不会保留以前的累计值 `runningTotal` 。 下一次 `runningTotal` 调用时， `applesSold` 将重新创建并将其初始化为0，并 `runningTotal` 只返回与调用该值相同的值。  
  
### <a name="compile-the-code"></a>编译代码  

 您可以将静态局部变量的值初始化为其声明的一部分。 如果将数组声明为 `Static` ，则可以将其级别 (的维度) 、每个维度的长度以及各个元素的值进行初始化。  
  
### <a name="security"></a>安全性  

 在前面的示例中，可以通过在模块级别声明来生成相同的生存期 `applesSold` 。 但是，如果您以这种方式更改了变量的作用域，则该过程将不再具有对它的独占访问权限。 由于其他过程可能会访问 `applesSold` 和更改其值，因此，运行总计可能不可靠，并且代码可能更难以维护。  
  
## <a name="see-also"></a>请参阅

- [共享](../../../language-reference/modifiers/shared.md)
- [没](../../../language-reference/nothing.md)
- [Declared Element Names](declared-element-names.md)
- [References to Declared Elements](references-to-declared-elements.md)
- [Visual Basic 中的范围](scope.md)
- [Visual Basic 中的访问级别](access-levels.md)
- [变量](../variables/index.md)
- [变量声明](../variables/variable-declaration.md)
- [数据类型疑难解答](../data-types/troubleshooting-data-types.md)
- [Static](../../../language-reference/modifiers/static.md)
