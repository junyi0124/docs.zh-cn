---
title: 如何：启动应用程序并向其发送击键 - Visual Basic
ms.date: 10/23/2019
helpviewer_keywords:
- keystrokes, sending
- Shell command example [Visual Basic]
- processes, starting and sending keystrokes
- SendKeys.SendWait examples
ms.assetid: f1303184-fce4-44fb-88b4-aac5f42d5d77
ms.openlocfilehash: 033999c07bb5839a264122b2ca330916bdf844b8
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2020
ms.locfileid: "72919393"
---
# <a name="how-to-start-an-application-and-send-it-keystrokes-visual-basic"></a>如何：启动应用程序并向其发送击键 (Visual Basic)

本示例使用 <xref:Microsoft.VisualBasic.Interaction.Shell%2A> 方法启动记事本应用程序，然后使用 [My.Computer.Keyboard.SendKeys](xref:Microsoft.VisualBasic.Devices.Keyboard.SendKeys%2A) 方法发送击键来打印句子。

## <a name="example"></a>示例

[!code-vb[VbVbalrMyComputer#25](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrMyComputer/VB/Class2.vb#25)]

## <a name="robust-programming"></a>可靠编程

如果找不到具有请求的进程标识符的应用程序，则会引发 <xref:System.ArgumentException> 异常。  
  
## <a name="net-framework-security"></a>.NET Framework 安全性

对 `Shell` 函数的调用需要完全信任（<xref:System.Security.SecurityException> 类）。

## <a name="see-also"></a>另请参阅

- <xref:Microsoft.VisualBasic.Devices.Keyboard.SendKeys%2A>
- <xref:Microsoft.VisualBasic.Interaction.Shell%2A>
- <xref:Microsoft.VisualBasic.Interaction.AppActivate%2A>
