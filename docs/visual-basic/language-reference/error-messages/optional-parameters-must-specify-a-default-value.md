---
title: 可选参数必须指定默认值
ms.date: 07/20/2015
f1_keywords:
- vbc30812
- bc30812
helpviewer_keywords:
- BC30812
ms.assetid: 5091a250-be66-413b-98a3-2a9974c4d600
ms.openlocfilehash: 3718fe5c42c8af0948f3b5cb0d120c6876c6f98f
ms.sourcegitcommit: ff5a4eb5cffbcac9521bc44a907a118cd7e8638d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2020
ms.locfileid: "92162448"
---
# <a name="bc30812-optional-parameters-must-specify-a-default-value"></a>BC30812：可选参数必须指定默认值

可选参数必须提供默认值，如果调用过程未提供任何参数，则可以使用这些默认值。

**错误 ID：** BC30812

## <a name="example"></a>示例

下面的示例生成 BC30812：

```vb
Sub Proc1(x As Integer, Optional y As String)
    Console.WriteLine("Default argument is: " & y)
End Sub
```

## <a name="to-correct-this-error"></a>更正此错误

指定可选参数的默认值：

```vb
Sub Proc1(x As Integer, Optional y As String = "Default Value")
    Console.WriteLine("Default argument is: " & y)
End Sub
```

## <a name="see-also"></a>另请参阅

- [可选](../modifiers/optional.md)
