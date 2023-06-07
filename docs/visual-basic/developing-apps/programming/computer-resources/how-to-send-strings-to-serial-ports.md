---
title: 如何：将字符串发送到串行端口
ms.date: 07/20/2015
helpviewer_keywords:
- ports, sending strings to
- strings [Visual Basic], sending to serial ports
- My.Computer.Ports object
- serial ports, sending strings to
ms.assetid: 6ebf46cd-b2d0-4b2c-9a1f-be177b22ad52
ms.openlocfilehash: f78df9cf1bd75432ea645c4dcc06498915ceee49
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84360288"
---
# <a name="how-to-send-strings-to-serial-ports-in-visual-basic"></a>如何：在 Visual Basic 中将字符串发送到串行端口

本主题介绍在 Visual Basic 中如何使用 `My.Computer.Ports` 将字符串发送到计算机的串行端口。  
  
## <a name="example"></a>示例  

 本示例将字符串发送到 COM1 串行端口。 你可能需要使用计算机上的其他串行端口。  
  
 使用 `My.Computer.Ports.OpenSerialPort` 方法获取对端口的引用。 有关详细信息，请参阅 <xref:Microsoft.VisualBasic.Devices.Ports.OpenSerialPort%2A>。  
  
 `Using` 块允许应用程序在即使会生成异常的情况下也关闭串行端口。 操作串行端口的所有代码都应出现在此块中，或者出现在 `Try...Catch...Finally` 块中。  
  
 <xref:System.IO.Ports.SerialPort.WriteLine%2A> 方法将数据发送到串行端口。  
  
 [!code-vb[VbVbalrMyComputer#33](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrMyComputer/VB/Class2.vb#33)]  
  
## <a name="compiling-the-code"></a>编译代码  
  
- 本示例假定计算机正在使用 `COM1`。  
  
## <a name="robust-programming"></a>可靠编程  

 本示例假定计算机正在使用 `COM1`；为了获得更大的灵活性，代码应允许用户从可用端口列表中选择所需的串行端口。 有关详细信息，请参阅[如何：显示可用的串行端口](how-to-show-available-serial-ports.md)。  
  
 本示例使用 `Using` 块来确保应用程序在即使会引发异常的情况下也关闭端口。 有关详细信息，请参阅 [Using 语句](../../../language-reference/statements/using-statement.md)。  
  
## <a name="see-also"></a>另请参阅

- <xref:Microsoft.VisualBasic.Devices.Ports>
- <xref:System.IO.Ports.SerialPort?displayProperty=nameWithType>
- [如何：使用连接到串行端口的调制解调器拨号](how-to-dial-modems-attached-to-serial-ports.md)
- [如何：显示可用的串行端口](how-to-show-available-serial-ports.md)
