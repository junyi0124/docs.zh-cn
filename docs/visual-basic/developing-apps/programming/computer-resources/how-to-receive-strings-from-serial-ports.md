---
title: 如何：从串行端口接收字符串
ms.date: 07/20/2015
helpviewer_keywords:
- serial ports, retrieving strings
- strings [Visual Basic], retrieving from serial ports
- My.Resources object
ms.assetid: 8371ce2c-e1c7-476b-a86d-9afc2614b6b7
ms.openlocfilehash: 93b6b47d89d05331c85a6459bba7d6fd5e2e3377
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84401831"
---
# <a name="how-to-receive-strings-from-serial-ports-in-visual-basic"></a>如何：在 Visual Basic 中从串行端口接收字符串

本主题介绍在 Visual Basic 中如何使用 `My.Computer.Ports` 从计算机的串行端口接收字符串。  
  
### <a name="to-receive-strings-from-the-serial-port"></a>从串行端口接收字符串  
  
1. 初始化返回字符串。  
  
     [!code-vb[VbVbalrMyComputer#38](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrMyComputer/VB/Class2.vb#38)]  
  
2. 确定应提供字符串的串行端口。 此示例假定它是 `COM1`。  
  
3. 使用 `My.Computer.Ports.OpenSerialPort` 方法获取对端口的引用。 有关详细信息，请参阅 <xref:Microsoft.VisualBasic.Devices.Ports.OpenSerialPort%2A>。  
  
     `Try...Catch...Finally` 块允许应用程序在即使会生成异常的情况下也关闭串行端口。 操作串行端口的所有代码都应出现在此块中。  
  
     [!code-vb[VbVbalrMyComputer#39](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrMyComputer/VB/Class2.vb#39)]  
  
4. 创建 `Do` 循环，用于读取文本行，直到没有更多行可用。  
  
     [!code-vb[VbVbalrMyComputer#40](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrMyComputer/VB/Class2.vb#40)]  
  
5. 使用 <xref:System.IO.Ports.SerialPort.ReadLine> 方法来从串行端口读取下一个可用的文本行。  
  
     [!code-vb[VbVbalrMyComputer#41](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrMyComputer/VB/Class2.vb#41)]  
  
6. 使用 `If` 语句可确定 <xref:System.IO.Ports.SerialPort.ReadLine> 方法是否返回 `Nothing`（这意味着没有更多文本可用）。 如果它未返回 `Nothing`，则退出 `Do` 循环。  
  
     [!code-vb[VbVbalrMyComputer#42](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrMyComputer/VB/Class2.vb#42)]  
  
7. 向 `If` 语句添加 `Else` 块以处理实际读取字符串的情况。 该块将来自串行端口的字符串追加到返回字符串。  
  
     [!code-vb[VbVbalrMyComputer#43](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrMyComputer/VB/Class2.vb#43)]  
  
8. 返回字符串。  
  
     [!code-vb[VbVbalrMyComputer#44](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrMyComputer/VB/Class2.vb#44)]  
  
## <a name="example"></a>示例  

 [!code-vb[VbVbalrMyComputer#37](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrMyComputer/VB/Class2.vb#37)]  
  
 此代码示例也可作为 IntelliSense 代码片段。 它位于代码片段选取器的“连接和网络”中。  有关详细信息，请参阅[代码片段](/visualstudio/ide/code-snippets)。  
  
## <a name="compiling-the-code"></a>编译代码  

 本示例假定计算机正在使用 `COM1`。  
  
## <a name="robust-programming"></a>可靠编程  

 本示例假定计算机正在使用 `COM1`。 为了获得更大的灵活性，代码应允许用户从可用端口列表中选择所需的串行端口。 有关详细信息，请参阅[如何：显示可用的串行端口](how-to-show-available-serial-ports.md)。  
  
 此示例使用 `Try...Catch...Finally` 块确保应用程序关闭端口以及捕获任何超时异常。 有关详细信息，请参阅 [Try...Catch...Finally 语句](../../../language-reference/statements/try-catch-finally-statement.md)。  
  
## <a name="see-also"></a>另请参阅

- <xref:Microsoft.VisualBasic.Devices.Ports>
- <xref:System.IO.Ports.SerialPort?displayProperty=nameWithType>
- [如何：使用连接到串行端口的调制解调器拨号](how-to-dial-modems-attached-to-serial-ports.md)
- [如何：将字符串发送到串行端口](how-to-send-strings-to-serial-ports.md)
- [如何：显示可用的串行端口](how-to-show-available-serial-ports.md)
