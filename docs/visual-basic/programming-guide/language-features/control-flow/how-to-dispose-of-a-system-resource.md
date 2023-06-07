---
title: 如何：释放系统资源
ms.date: 07/20/2015
helpviewer_keywords:
- Using statement [Visual Basic], disposing of system resources
- Visual Basic code, control flow
- system resources, disposing of
- resources [Visual Basic], disposing of system
- system resources
- Using statement [Visual Basic], Using...End Using
- Using block
ms.assetid: 8be2b239-8090-419b-8e7e-bcaa75b0ecc8
ms.openlocfilehash: c430bc7744f5aefaa65f2a86f3e5e22743ffed57
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91077197"
---
# <a name="how-to-dispose-of-a-system-resource-visual-basic"></a>如何：释放系统资源 (Visual Basic)

您可以使用 `Using` 块来保证系统在代码退出块时释放资源。 如果你使用的系统资源占用大量内存，或者其他组件也想要使用，则此方法很有用。  
  
### <a name="to-dispose-of-a-database-connection-when-your-code-is-finished-with-it"></a>使用数据库连接完成代码时释放数据库连接  
  
1. 在这种情况下，请确保将相应的 [Imports 语句 ( .Net 命名空间和类型) ](../../../language-reference/statements/imports-statement-net-namespace-and-type.md) 用于您的源文件的开头的数据库连接 (<xref:System.Data.SqlClient>) 。  
  
2. `Using`使用和语句创建块 `Using` `End Using` 。 在块中，放置用于处理数据库连接的代码。  
  
3. 声明连接并在语句中创建其实例 `Using` 。  
  
    ```vb  
    ' Insert the following line at the beginning of your source file.  
    Imports System.Data.SqlClient  
    Public Sub AccessSql(ByVal s As String)  
        Using sqc As New System.Data.SqlClient.SqlConnection(s)  
            MsgBox("Connected with string """ & sqc.ConnectionString & """")  
        End Using  
    End Sub  
    ```  
  
     无论退出块的方式如何（包括未经处理的异常的情况），系统都将释放资源。  
  
     请注意，不能 `sqc` 从块外部访问 `Using` ，因为它的作用域限制为块。  
  
     您可以对系统资源（如文件句柄或 COM 包装）使用这种方法。 `Using`当你希望在退出块后，请使用块来确保其他组件可以使用该资源 `Using` 。  
  
## <a name="see-also"></a>请参阅

- <xref:System.Data.SqlClient.SqlConnection>
- [控制流](index.md)
- [决策结构](decision-structures.md)
- [循环结构](loop-structures.md)
- [其他控件结构](other-control-structures.md)
- [嵌套的控件结构](nested-control-structures.md)
- [Using 语句](../../../language-reference/statements/using-statement.md)
