---
title: 结构和其他编程元素
ms.date: 07/20/2015
helpviewer_keywords:
- structures [Visual Basic], arrays
- procedures [Visual Basic], structures as arguments to
- objects [Visual Basic], structure elements
- arrays [Visual Basic], structure elements
- nested structures [Visual Basic]
ms.assetid: 0f849313-ccd2-4c9a-acb9-69de6751c088
ms.openlocfilehash: 26c98adda7305783b0220141db35b08285b21554
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91084081"
---
# <a name="structures-and-other-programming-elements-visual-basic"></a>结构和其他编程元素 (Visual Basic)

您可以将结构与数组、对象和过程以及彼此一起使用。 交互使用的语法与这些元素分别使用的语法相同。  
  
> [!NOTE]
> 不能在结构声明中初始化任何结构元素。 只能为已声明为结构类型的变量的元素赋值。  
  
## <a name="structures-and-arrays"></a>结构和数组  

 结构可以包含数组作为其一个或多个元素。 下面的示例对此进行了演示。  
  
```vb  
Public Structure systemInfo  
    Public cPU As String  
    Public memory As Long  
    Public diskDrives() As String  
    Public purchaseDate As Date  
End Structure
```  
  
 访问结构中数组的值的方式与访问对象上的属性的方式相同。 下面的示例对此进行了演示。  
  
```vb  
Dim mySystem As systemInfo  
ReDim mySystem.diskDrives(3)  
mySystem.diskDrives(0) = "1.44 MB"  
```  
  
 还可以声明结构的数组。 下面的示例对此进行了演示。  
  
```vb  
Dim allSystems(100) As systemInfo  
```  
  
 遵循相同的规则来访问此数据体系结构的组件。 下面的示例对此进行了演示。  
  
```vb  
ReDim allSystems(5).diskDrives(3)  
allSystems(5).CPU = "386SX"  
allSystems(5).diskDrives(2) = "100M SCSI"  
```  
  
## <a name="structures-and-objects"></a>结构和对象  

 结构可以包含对象作为其一个或多个元素。 下面的示例对此进行了演示。  
  
```vb  
Protected Structure userInput  
    Public userName As String  
    Public inputForm As System.Windows.Forms.Form  
    Public userFileNumber As Integer  
End Structure  
```  
  
 在此类声明中应使用特定对象类，而不是 `Object` 。  
  
## <a name="structures-and-procedures"></a>结构和过程  

 可以将结构作为过程自变量传递。 下面的示例对此进行了演示。  
  
```vb  
Public currentCPUName As String = "700MHz Pentium compatible"  
Public currentMemorySize As Long = 256  
Public Sub fillSystem(ByRef someSystem As systemInfo)  
    someSystem.cPU = currentCPUName  
    someSystem.memory = currentMemorySize  
    someSystem.purchaseDate = Now  
End Sub  
```  
  
 前面的示例 *通过引用*传递结构，这允许过程修改其元素，以使所做的更改在调用代码中生效。 如果要针对此类修改保护结构，请按值传递它。  
  
 还可以从过程返回结构 `Function` 。 下面的示例对此进行了演示。  
  
```vb  
Dim allSystems(100) As systemInfo  
Function findByDate(ByVal searchDate As Date) As systemInfo  
    Dim i As Integer  
    For i = 1 To 100  
        If allSystems(i).purchaseDate = searchDate Then Return allSystems(i)  
    Next i  
   ' Process error: system with desired purchase date not found.  
End Function  
```  
  
## <a name="structures-within-structures"></a>结构内的结构  

 结构可以包含其他结构。 下面的示例对此进行了演示。  
  
```vb  
Public Structure driveInfo  
    Public type As String  
    Public size As Long  
End Structure  
Public Structure systemInfo  
    Public cPU As String  
    Public memory As Long  
    Public diskDrives() As driveInfo  
    Public purchaseDate As Date  
End Structure  
```  
  
```vb  
Dim allSystems(100) As systemInfo  
ReDim allSystems(1).diskDrives(3)  
allSystems(1).diskDrives(0).type = "Floppy"  
```  
  
 你还可以使用此方法将一个模块中定义的结构封装在另一个模块中定义的结构内。  
  
 结构可以包含任意深度的其他结构。  
  
## <a name="see-also"></a>请参阅

- [数据类型](index.md)
- [基本数据类型](elementary-data-types.md)
- [复合数据类型](composite-data-types.md)
- [Value Types and Reference Types](value-types-and-reference-types.md)
- [结构](structures.md)
- [数据类型疑难解答](troubleshooting-data-types.md)
- [如何：声明结构](how-to-declare-a-structure.md)
- [结构变量](structure-variables.md)
- [结构和类](structures-and-classes.md)
- [Structure 语句](../../../language-reference/statements/structure-statement.md)
