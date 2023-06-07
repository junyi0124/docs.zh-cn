---
title: End 语句
ms.date: 07/20/2015
f1_keywords:
- vb.End
- End
helpviewer_keywords:
- execution [Visual Basic], ending
- files [Visual Basic], closing
- End keyword [Visual Basic], End statements
- programs [Visual Basic], quitting
- code, exiting
- program termination
- End statement [Visual Basic]
- execution [Visual Basic], stopping
ms.assetid: 0e64467c-0f34-4aab-9ddd-43f8b9d55d90
ms.openlocfilehash: 0c99b919b50701e93fab7caf5fb5d8b6b976d44b
ms.sourcegitcommit: d2db216e46323f73b32ae312c9e4135258e5d68e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2020
ms.locfileid: "90865857"
---
# <a name="end-statement"></a>End 语句

立即终止执行。  
  
## <a name="syntax"></a>语法  
  
```vb  
End  
```  
  
## <a name="remarks"></a>备注  

 可以将语句放 `End` 在过程中的任意位置，以强制整个应用程序停止运行。 `End` 关闭使用语句打开的所有文件 `Open` ，并清除应用程序的所有变量。 应用程序将立即关闭，因为没有任何其他程序保留对其对象的引用，并且没有任何代码正在运行。  
  
> [!NOTE]
> `End`语句会突然停止代码执行，并且不会调用 `Dispose` 或 `Finalize` 方法或任何其他 Visual Basic 代码。 其他程序持有的对象引用已失效。 如果 `End` 在或块中遇到语句 `Try` `Catch` ，控件不会传递给相应的 `Finally` 块。  
  
 `Stop`语句暂停执行，但与 `End` 此不同，它不会关闭任何文件或清除任何变量，除非在已编译的可执行文件 ( .exe) 文件中出现。  
  
 由于 `End` 终止了你的应用程序而不参与任何可能已打开的资源，因此，在使用该应用程序之前，你应尝试完全关闭。 例如，如果应用程序打开了任何窗体，则在控制到达语句之前应关闭它们 `End` 。  
  
 应 `End` 慎用，只需在需要立即停止时使用。 终止过程 ([返回语句](return-statement.md) 和 [退出语句](exit-statement.md) 的常规方法) 不仅完全关闭过程，而且还向调用代码授予完全关闭的机会。 例如，控制台应用程序可以直接 `Return` 从 `Main` 过程开始。  
  
> [!IMPORTANT]
> `End`语句 <xref:System.Environment.Exit%2A> <xref:System.Environment> 在命名空间中调用类的方法 <xref:System> 。 <xref:System.Environment.Exit%2A> 要求您具有 `UnmanagedCode` 权限。 否则， <xref:System.Security.SecurityException> 会发生错误。  
  
 后跟一个附加的关键字时， [end \<keyword> 语句](end-keyword-statement.md) 指定相应过程或块定义的末尾。 例如， `End Function` 终止过程的定义 `Function` 。  
  
## <a name="example"></a>示例  

 下面的示例使用 `End` 语句在用户请求代码的情况下终止代码执行。  
  
 [!code-vb[VbVersHelp60Controls#64](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVersHelp60Controls/VB/Form1.vb#64)]  
  
## <a name="smart-device-developer-notes"></a>智能设备开发人员说明  

 不支持此语句。  
  
## <a name="see-also"></a>另请参阅

- <xref:System.Security.Permissions.SecurityPermissionFlag>
- [Stop 语句](stop-statement.md)
- [End \<keyword> 语句](end-keyword-statement.md)
