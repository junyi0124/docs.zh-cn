---
title: 数组疑难解答
ms.date: 07/20/2015
helpviewer_keywords:
- troubleshooting arrays
- arrays [Visual Basic], initialization errors
- troubleshooting Visual Basic, arrays
- arrays [Visual Basic], compilation errors
- arrays [Visual Basic], declaration errors
- arrays [Visual Basic], troubleshooting
ms.assetid: f4e971c7-c0a4-4ed7-a77a-8d71039f266f
ms.openlocfilehash: e0cb1008f4182331b7380db81d7a92a0fd45f2f4
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91086356"
---
# <a name="troubleshooting-arrays-visual-basic"></a>数组疑难解答 (Visual Basic)

此页列出了在使用数组时可能出现的一些常见问题。  
  
## <a name="compilation-errors-declaring-and-initializing-an-array"></a>声明和初始化数组的编译错误  

 声明、创建和初始化数组的规则误解可能会引发编译错误。 错误的最常见原因如下：  
  
- 在数组变量声明中指定维度长度后，提供一个 [新的运算符](../../../language-reference/operators/new-operator.md) 子句。 下面的代码行显示了此类型的无效声明。  
  
     `Dim INVALIDsingleDimByteArray(2) As Byte = New Byte()`  
  
     `Dim INVALIDtwoDimShortArray(1, 1) As Short = New Short(,)`  
  
     `Dim INVALIDjaggedByteArray(1)() As Byte = New Byte()()`  
  
- 指定超出交错数组顶级数组的维度长度。 下面的代码行显示了此类型的无效声明。  
  
     `Dim INVALIDjaggedByteArray(1)(1) As Byte`  
  
- `New`指定元素值时省略关键字。 下面的代码行显示了此类型的无效声明。  
  
     `Dim INVALIDoneDimShortArray() As Short = Short() {0, 1, 2, 3}`  
  
- ) 提供 `New` 不带大括号的子句 (`{}` 。 下面的代码行显示了此类型的无效声明。  
  
     `Dim INVALIDsingleDimByteArray() As Byte = New Byte()`  
  
     `Dim INVALIDsingleDimByteArray() As Byte = New Byte(2)`  
  
     `Dim INVALIDtwoDimShortArray(,) As Short = New Short(,)`  
  
     `Dim INVALIDtwoDimShortArray(,) As Short = New Short(1, 1)`  
  
## <a name="accessing-an-array-out-of-bounds"></a>访问超出界限的数组  

 初始化数组的过程为每个维度分配上限和下限。 每次访问数组的元素时，必须为每个维度指定有效的索引或下标。 如果任何索引低于其下限或高于其上限，则会引发 <xref:System.IndexOutOfRangeException> 异常。 编译器无法检测到此类错误，因此在运行时出现错误。  
  
### <a name="determining-bounds"></a>确定边界  

 如果另一个组件将数组传递给您的代码（例如，作为过程参数），则您不知道该数组的大小或其维度的长度。 在尝试访问任何元素之前，应始终确定数组的每个维度的上限。 如果数组是通过 Visual Basic 子句之外的其他方法创建的 `New` ，则下限可能不是0，并且最安全的做法是确定下限。  
  
### <a name="specifying-the-dimension"></a>指定维度  

 确定多维数组的边界时，请注意如何指定维度。 `dimension`和方法的参数 <xref:System.Array.GetLowerBound%2A> <xref:System.Array.GetUpperBound%2A> 是从0开始的，而 `Rank` Visual Basic <xref:Microsoft.VisualBasic.Information.LBound%2A> 和函数的参数 <xref:Microsoft.VisualBasic.Information.UBound%2A> 是从1开始的。  
  
## <a name="see-also"></a>请参阅

- [数组](index.md)
- [如何：在 Visual Basic 中初始化数组变量](how-to-initialize-an-array-variable.md)
