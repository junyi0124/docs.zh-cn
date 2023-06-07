---
title: 解释网络跟踪
description: 了解如何使用跟踪来捕获应用程序对 .NET Framework 中各 System.Net 类成员的调用。
ms.date: 03/30/2017
helpviewer_keywords:
- TraceMode attribute
- hexidecimal data, network tracing output
- network tracing, analyzing
- protocolonly
- text, network tracing output
- includehex
ms.assetid: ad22b4b8-00af-4778-9cca-cb609ce1f8ff
ms.openlocfilehash: 63d7e36bb95054303fc4f26b0fd14dc3d10dbb7d
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96241559"
---
# <a name="interpreting-network-tracing"></a>解释网络跟踪

启用网络跟踪之后，可以通过跟踪捕获应用程序对各种 <xref:System.Net> 类成员的调用。 这些调用的输出可能类似于以下示例。  
  
```output
[588]   (4357)   Entering Socket#33574638::Send()  
[588]   (4387)   Exiting Socket#33574638::Send()-> 61#61
```  
  
 在上一示例中，[588] 是当前线程的唯一标识符。 (4357) 和 (4387) 是一种时间戳，它表示应用程序启动以来已经经过的毫秒数。 时间戳之后的数据显示了进入和退出“Socket.Send”方法的应用程序。 执行“Send”方法的对象使用 33574638 作为其唯一标识符。 退出跟踪方法包括返回值（上一示例中为 61）。  
  
 网络跟踪可以使用诸如 Hypertext Transfer Protocol (HTTP) 应用程序级协议捕获由应用程序发送或接收的网络流量。 此数据可捕获为文本或十六进制数据。 如果将“includehex”指定为“tracemode”属性的值，可使用十六进制数据 。 （有关此属性的详细信息，请参阅[如何：配置网络跟踪](how-to-configure-network-tracing.md)。）下例通过“includehex”生成跟踪。  
  
 `[1692]   (1142)   00000000 : 47 45 54 20 2F 77 70 61-64 2E 64 61 74 20 48 54 : GET /wpad.dat HT`  
  
 `[1692]   (1142)   00000010 : 54 50 2F 31 2E 31 0D 0A-48 6F 73 74 3A 20 69 74 : TP/1.1..Host: it`  
  
 `[1692]   (1142)   00000020 : 67 70 72 6F 78 79 0D 0A-43 6F 6E 6E 65 63 74 69 : gproxy..Connecti`  
  
 `[1692]   (1142)   00000030 : 6F 6E 3A 20 43 6C 6F 73-65 0D 0A 0D 0A     : on: Close....`  
  
 若要省略十六进制数据，请将“protocolonly”指定为“tracemode”属性的值 。 下例演示了指定“protocolonly”时的跟踪。  
  
 `[2444]   (594)   Data from ConnectStream#33574638::WriteHeaders<<GET /wpad.dat HTTP/1.1`  
  
 `Host: itgproxy`  
  
 `Connection: Close`  
  
## <a name="see-also"></a>请参阅

- [启用网络跟踪](enabling-network-tracing.md)
- [如何：配置网络跟踪](how-to-configure-network-tracing.md)
- [.NET Framework 中的网络跟踪](network-tracing.md)
