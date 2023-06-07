---
ms.openlocfilehash: e66a5c2a33a4ab5cf35013c1843936ffd01f90a2
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/01/2020
ms.locfileid: "85614359"
---
### <a name="serialport-background-thread-exceptions"></a>SerialPort 后台线程异常

#### <a name="details"></a>详细信息

使用 <xref:System.IO.Ports.SerialPort> 流创建的后台线程不再在引发 OS 异常时终止进程。 <br/>在面向 .NET Framework 4.7 及更早版本的应用程序中，使用 <xref:System.IO.Ports.SerialPort> 流创建的后台线程上引发操作系统异常时，会终止进程。 <br/>在面向 .NET Framework 4.7.1 或更高版本的应用程序中，后台线程等待与活动串行端口相关的 OS 事件，在某些情况下（例如突然删除串行端口）也可能崩溃。

#### <a name="suggestion"></a>建议

对于面向 .NET Framework 4.7.1 的应用，如果无需异常处理，可通过将以下内容添加到 `app.config` 文件的 `<runtime>` 部分，从而选择不用异常处理：

```xml
<runtime>
  <AppContextSwitchOverrides value="Switch.System.IO.Ports.DoNotCatchSerialStreamThreadExceptions=true" />
</runtime>
```

对于面向旧版 .NET Framework，但在 .NET Framework 4.7.1 或更高版本上运行的应用，可通过将以下内容添加到 `app.config` 文件的 `<runtime>` 部分来选择使用异常处理：

```xml
<runtime>
  <AppContextSwitchOverrides value="Switch.System.IO.Ports.DoNotCatchSerialStreamThreadExceptions=false" />
</runtime>
```

| “属性”    | “值”       |
|:--------|:------------|
| 范围   | 次要       |
| Version | 4.7.1       |
| 类型    | 重定目标 |

#### <a name="affected-apis"></a>受影响的 API

- <xref:System.IO.Ports.SerialPort?displayProperty=nameWithType>
