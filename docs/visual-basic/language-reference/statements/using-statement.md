---
title: Using 语句
ms.date: 07/20/2015
f1_keywords:
- vb.using
helpviewer_keywords:
- resource disposal
- Try...Catch...Finally statements, equivalent to Using statement
- resources [Visual Basic], disposing
- Using statement [Visual Basic]
ms.assetid: 665d1580-dd54-4e96-a9a9-6be2a68948f1
ms.openlocfilehash: 6ec0e228b3898f66f27e322b5db2dd7f3bf3d7d6
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/22/2019
ms.locfileid: "74352762"
---
# <a name="using-statement-visual-basic"></a>Using 语句 (Visual Basic)

声明 `Using` 块的开头，并根据需要获取块所控制的系统资源。

## <a name="syntax"></a>语法

```vb
Using { resourcelist | resourceexpression }
    [ statements ]
End Using
```

## <a name="parts"></a>部件

|术语|Definition|  
|---|---|  
|`resourcelist`|如果未提供 `resourceexpression`，则为必需。 此 `Using` 阻止控件的一个或多个系统资源的列表（用逗号分隔）。|  
|`resourceexpression`|如果未提供 `resourcelist`，则为必需。 引用由该 `Using` 块控制的系统资源的引用变量或表达式。|  
|`statements`|可选。 `Using` 块运行的语句块。|  
|`End Using`|必需。 终止 `Using` 块的定义，并释放它所控制的所有资源。|  

 `resourcelist` 部分中的每个资源都具有以下语法和部分：

 `resourcename As New resourcetype [ ( [ arglist ] ) ]`

 \- 或 -

 `resourcename As resourcetype = resourceexpression`

## <a name="resourcelist-parts"></a>resourcelist 部件

|术语|Definition|  
|---|---|  
|`resourcename`|必需。 引用变量，引用 `Using` 块控制的系统资源。|  
|`New`|如果 `Using` 语句获取资源，则为必需。 如果已获取资源，请使用第二种语法替代方法。|  
|`resourcetype`|必需。 资源的类。 类必须实现 <xref:System.IDisposable> 接口。|  
|`arglist`|可选。 要传递给构造函数以创建 `resourcetype`实例的参数的列表。 请参阅[参数列表](parameter-list.md)。|  
|`resourceexpression`|必需。 引用满足 `resourcetype`要求的系统资源的变量或表达式。 如果使用第二种语法替代方法，则必须先获取资源，然后才能将控制传递给 `Using` 语句。|  
  
## <a name="remarks"></a>备注

 有时，您的代码需要非托管资源，如文件句柄、COM 包装或 SQL 连接。 当你的代码完成了一个或多个此类资源时，`Using` 块可保证其释放。 这使它们可供其他代码使用。

 .NET Framework 垃圾回收器（GC）释放托管资源，而不会对你的部分进行任何额外编码。 托管资源不需要 `Using` 块。 但是，您仍然可以使用 `Using` 块强制处置托管资源，而不是等待垃圾回收器。

 `Using` 块有三个部分：获取、使用和处理。

- *获取*是指创建一个变量并将其初始化，以引用系统资源。 `Using` 语句可以获取一个或多个资源，也可以在输入块之前获取一个资源，并将其提供给 `Using` 语句。 如果提供 `resourceexpression`，则必须在将控制权传递给 `Using` 语句之前获取资源。

- *用法*是指访问资源并对其执行操作。 `Using` 和 `End Using` 之间的语句表示资源的使用情况。

- *处置*意味着对 `resourcename`中的对象调用 <xref:System.IDisposable.Dispose%2A> 方法。 这使对象可以干净地终止其资源。 `End Using` 语句将释放 `Using` 块的控件下的资源。

## <a name="behavior"></a>行为

 `Using` 块的行为类似于 `Try`...`Finally` 构造，其中 `Try` 块使用资源，而 `Finally` 块处置它们。 因此，无论退出块的方式如何，`Using` 块均可保证资源的释放。 即使在发生未经处理的异常的情况下也是如此（<xref:System.StackOverflowException>除外）。

 `Using` 语句获取的每个资源变量的作用域仅限于 `Using` 块。

 如果在 `Using` 语句中指定多个系统资源，则效果与嵌套 `Using` 块在另一个中的情况相同。

 如果 `Nothing``resourcename`，则不会调用 <xref:System.IDisposable.Dispose%2A>，也不会引发异常。

## <a name="structured-exception-handling-within-a-using-block"></a>Using 块中的结构化异常处理

 如果需要处理在 `Using` 块中可能发生的异常，可以向其添加完整的 `Try`...`Finally` 构造。 如果需要处理 `Using` 语句获取资源不成功的情况，可以测试是否 `Nothing`了 `resourcename`。

## <a name="structured-exception-handling-instead-of-a-using-block"></a>结构化异常处理而不是 Using 块

 如果需要更好地控制资源的获取，或在 `Finally` 块中需要其他代码，则可以将 `Using` 块重写为 `Try``Finally` 构造。 下面的示例演示主干 `Try` 和 `Using` 构造，在 `resource`的获取和处置中等效。

```vb
Using resource As New resourceType
    ' Insert code to work with resource.
End Using

' For the acquisition and disposal of resource, the following  
' Try construction is equivalent to the Using block.
Dim resource As New resourceType
Try
    ' Insert code to work with resource.
Finally
    If resource IsNot Nothing Then
        resource.Dispose()
    End If
End Try
```

> [!NOTE]
> `Using` 块内的代码不应将 `resourcename` 中的对象分配给另一个变量。 退出 `Using` 块后，资源会被释放，另一个变量将无法访问它所指向的资源。

## <a name="example"></a>示例

 下面的示例创建一个名为 ".log" 的文件，并向该文件写入两行文本。 该示例还读取同一文件并显示文本行：

 由于 <xref:System.IO.TextWriter> 和 <xref:System.IO.TextReader> 类实现了 <xref:System.IDisposable> 接口，因此代码可以使用 `Using` 语句，以确保在执行写入和读取操作后正确关闭文件。

 [!code-vb[VbVbalrStatements#50](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#50)]

## <a name="see-also"></a>另请参阅

- <xref:System.IDisposable>
- [Try...Catch...Finally 语句](try-catch-finally-statement.md)
- [如何：释放系统资源](../../programming-guide/language-features/control-flow/how-to-dispose-of-a-system-resource.md)
