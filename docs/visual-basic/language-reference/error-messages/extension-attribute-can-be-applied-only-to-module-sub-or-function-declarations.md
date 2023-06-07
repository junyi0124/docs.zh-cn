---
title: “Extension”特性只能应用于“Module”、“Sub”或“Function”声明。
ms.date: 07/20/2015
f1_keywords:
- bc36550
- vbc36550
helpviewer_keywords:
- BC36550
ms.assetid: 4387a51f-733c-45d7-abdb-eb64d4f51078
ms.openlocfilehash: bd4d14721b93800831dbce897535b4f5956fe9c7
ms.sourcegitcommit: ff5a4eb5cffbcac9521bc44a907a118cd7e8638d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2020
ms.locfileid: "92160745"
---
# <a name="bc36550-extension-attribute-can-be-applied-only-to-module-sub-or-function-declarations"></a>BC36550： "Extension" 特性只能应用于 "Module"、"Sub" 或 "Function" 声明

在 Visual Basic 中扩展数据类型的唯一方法是在标准模块内定义扩展方法。 扩展方法可以是 `Sub` 过程或 `Function` 过程。 必须用 `<Extension()>` 命名空间中的扩展属性标记所有扩展方法 <xref:System.Runtime.CompilerServices?displayProperty=nameWithType> 。 或者，包含扩展方法的模块可以用相同的方式进行标记。 扩展属性的其他使用无效。

**错误 ID：** BC36550

## <a name="to-correct-this-error"></a>更正此错误

- 删除扩展属性。

- 将扩展重新设计为封闭模块中定义的方法。

## <a name="example"></a>示例

下面的示例定义了 `Print` `String` 数据类型的方法。

```vb
Imports StringUtility
Imports System.Runtime.CompilerServices
Namespace StringUtility
    <Extension()>
    Module StringExtensions
        <Extension()>
        Public Sub Print (ByVal str As String)
            Console.WriteLine(str)
        End Sub
    End Module
End Namespace
```

## <a name="see-also"></a>另请参阅

- [属性概述](../../programming-guide/concepts/attributes/index.md)
- [扩展方法](../../programming-guide/language-features/procedures/extension-methods.md)
- [Module 语句](../statements/module-statement.md)
