---
title: .NET Framework 中的端口操作
ms.date: 07/20/2015
helpviewer_keywords:
- ports, Visual Basic
ms.assetid: 1eba223b-7bd3-401a-b097-982bce96df1b
ms.openlocfilehash: 0ef0b8a7aec40603185d227d972cea655fd238c1
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84360132"
---
# <a name="port-operations-in-the-net-framework-with-visual-basic"></a>使用 Visual Basic 在 .NET Framework 中执行的端口操作

可以通过 <xref:System.IO.Ports?displayProperty=nameWithType> 命名空间中的 .NET Framework 类访问计算机的串行端口。 最重要的类 <xref:System.IO.Ports.SerialPort> 为同步和事件驱动 I/O 提供框架，提供对插针和中断状态的访问，以及对串行驱动程序属性的访问。 它可以包装在可通过 <xref:System.IO.Ports.SerialPort.BaseStream> 属性访问的 <xref:System.IO.Stream> 对象中。 将 <xref:System.IO.Ports.SerialPort> 包装在 <xref:System.IO.Stream> 对象中，以便允许使用流的类访问串行端口。 该命名空间包含可以简化对串行端口的控制的枚举。

创建 <xref:System.IO.Ports.SerialPort> 对象的最简单的方法是通过 <xref:Microsoft.VisualBasic.Devices.Ports.OpenSerialPort%2A> 方法。

> [!NOTE]
> 不能使用 .NET Framework 类直接访问并行端口、USB 端口等其他类型的端口。

## <a name="enumerations"></a>枚举

此表列出并描述了用于访问串行端口的主要枚举：

|枚举|说明|
|---|---|
|<xref:System.IO.Ports.Handshake>|指定在为 <xref:System.IO.Ports.SerialPort> 对象建立串行端口通信时使用的控制协议。|
|<xref:System.IO.Ports.Parity>|指定 <xref:System.IO.Ports.SerialPort> 对象的奇偶校验位。|
|<xref:System.IO.Ports.SerialData>|指定在 <xref:System.IO.Ports.SerialPort> 对象的串行端口上收到的字符的类型。|
|<xref:System.IO.Ports.SerialError>|指定在 <xref:System.IO.Ports.SerialPort> 对象上发生的错误|
|<xref:System.IO.Ports.SerialPinChange>|指定在 <xref:System.IO.Ports.SerialPort> 对象上发生的更改的类型。|
|<xref:System.IO.Ports.StopBits>|指定在 <xref:System.IO.Ports.SerialPort> 对象上使用的停止位的数目。|

## <a name="see-also"></a>另请参阅

- <xref:Microsoft.VisualBasic.Devices.Ports>
- [访问计算机的端口](accessing-the-computer-s-ports.md)
