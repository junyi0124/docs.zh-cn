---
title: 对象变量值
ms.date: 07/20/2015
helpviewer_keywords:
- object variables [Visual Basic], values
- values [Visual Basic], of object variables
- data types [Visual Basic], object variable
- variables [Visual Basic], object
ms.assetid: 31555704-58a3-49f1-9a0a-6421f605664f
ms.openlocfilehash: 800b9754ce27cc6a494dd781d06f4bdca8a10e87
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91080226"
---
# <a name="object-variable-values-visual-basic"></a>对象变量值 (Visual Basic)

[对象数据类型](../../../language-reference/data-types/object-data-type.md)的变量可以引用任何类型的数据。 在变量中存储的值 `Object` 将保留在内存中的其他位置，而变量本身保存指向数据的指针。  
  
## <a name="object-classifier-functions"></a>对象分类器函数  

 Visual Basic 提供返回变量引用信息的函数 `Object` ，如下表所示。  
  
|功能|如果对象变量引用，则返回 True|  
|--------------|---------------------------------------------------|  
|<xref:Microsoft.VisualBasic.Information.IsArray%2A>|值的数组，而不是单个值|  
|<xref:Microsoft.VisualBasic.Information.IsDate%2A>|[日期数据类型](../../../language-reference/data-types/date-data-type.md)值，或可解释为日期和时间值的字符串|  
|<xref:Microsoft.VisualBasic.Information.IsDBNull%2A>|类型为的对象 <xref:System.DBNull> ，表示缺少或不存在的数据|  
|<xref:Microsoft.VisualBasic.Information.IsError%2A>|异常对象，派生自 <xref:System.Exception>|  
|<xref:Microsoft.VisualBasic.Information.IsNothing%2A>|[没有，也就是说](../../../language-reference/nothing.md)，当前没有对象赋给变量|  
|<xref:Microsoft.VisualBasic.Information.IsNumeric%2A>|一个数字，或者一个可解释为数字的字符串。|  
|<xref:Microsoft.VisualBasic.Information.IsReference%2A>|引用类型 (例如字符串、数组、委托或类类型) |  
  
 您可以使用这些函数来避免将无效值提交给操作或过程。  
  
## <a name="typeof-operator"></a>TypeOf 运算符  

 你还可以使用 [TypeOf 运算符](../../../language-reference/operators/typeof-operator.md) 来确定对象变量当前是否引用了特定数据类型。 `TypeOf` `Is` `True` 如果操作数的运行时类型派生自或实现指定的类型，则 ... 表达式的计算结果为。  
  
 下面的示例 `TypeOf` 在引用值和引用类型的对象变量上使用。  
  
```vb  
' The following statement puts a value type (Integer) in an Object variable.  
Dim num As Object = 10  
' The following statement puts a reference type (Form) in an Object variable.  
Dim frm As Object = New Form()  
If TypeOf num Is Long Then Debug.WriteLine("num is Long")  
If TypeOf num Is Integer Then Debug.WriteLine("num is Integer")  
If TypeOf num Is Short Then Debug.WriteLine("num is Short")  
If TypeOf num Is Object Then Debug.WriteLine("num is Object")  
If TypeOf frm Is Form Then Debug.WriteLine("frm is Form")  
If TypeOf frm Is Label Then Debug.WriteLine("frm is Label")  
If TypeOf frm Is Object Then Debug.WriteLine("frm is Object")  
```  
  
 前面的示例将以下行写入 " **调试** " 窗口：  
  
 `num is Integer`  
  
 `num is Object`  
  
 `frm is Form`  
  
 `frm is Object`  
  
 对象变量 `num` 引用类型的数据 `Integer` ，并 `frm` 引用类的对象 <xref:System.Windows.Forms.Form> 。  
  
## <a name="object-arrays"></a>对象数组  

 您可以声明和使用变量的数组 `Object` 。 如果需要处理各种数据类型和对象类，这会很有用。 数组中的所有元素必须具有相同的声明数据类型。 如果将此数据类型声明为，则可以在 `Object` 数组中将对象和类实例与其他数据类型一起存储。  
  
## <a name="see-also"></a>请参阅

- [对象变量](object-variables.md)
- [对象变量声明](object-variable-declaration.md)
- [对象变量赋值](object-variable-assignment.md)
- [如何：引用对象的当前实例](how-to-refer-to-the-current-instance-of-an-object.md)
- [如何：确定对象变量引用的类型](how-to-determine-what-type-an-object-variable-refers-to.md)
- [如何：确定两个对象是否相关](how-to-determine-whether-two-objects-are-related.md)
- [如何：确定两个对象是否相同](how-to-determine-whether-two-objects-are-identical.md)
- [数据类型](../data-types/index.md)
