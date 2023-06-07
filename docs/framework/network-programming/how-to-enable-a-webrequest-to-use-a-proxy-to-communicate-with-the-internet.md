---
title: 如何：使 WebRequest 能够使用代理以与 Internet 通信
description: 了解如何创建全局代理实例，使任何 WebRequest 可以在 .NET Framework 中使用代理与 Internet 通信。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 63c0ef2c-44b5-4c54-9804-ba0b9b001ac7
ms.openlocfilehash: 0fc33cea3f5a7fe4669b110e53e71afdb9561c23
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/08/2020
ms.locfileid: "84502530"
---
# <a name="how-to-enable-a-webrequest-to-use-a-proxy-to-communicate-with-the-internet"></a>如何：使 WebRequest 能够使用代理以与 Internet 通信

此示例将创建一个全局代理实例，该实例将启用任何 <xref:System.Net.WebRequest> 以使用代理与 Internet 进行通信。 该示例假定代理服务器名为 `webproxy`，且在端口 80（标准 HTTP 端口）上进行通信。

## <a name="example"></a>示例

```csharp
var proxyObject = new WebProxy("http://webproxy:80/");
GlobalProxySelection.Select = proxyObject;
```

```vb
Dim proxyObject As New WebProxy("http://webproxy:80/")
GlobalProxySelection.Select = proxyObject
```

## <a name="compiling-the-code"></a>编译代码

此示例需要：

- System.Net 命名空间的 C# [`using` 指令](../../csharp/language-reference/keywords/using-directive.md)。
- System.Net 命名空间的 Visual Basic [`Imports` 语句](../../visual-basic/language-reference/statements/imports-statement-net-namespace-and-type.md)。

## <a name="see-also"></a>请参阅

- [使用应用程序协议](using-application-protocols.md)
- [通过代理访问 Internet](accessing-the-internet-through-a-proxy.md)
