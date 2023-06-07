---
ms.openlocfilehash: 519de92ca4201d199941afe6382ddf9fc480fbbd
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/01/2020
ms.locfileid: "85614370"
---
### <a name="servicebase-doesnt-propagate-onstart-exceptions"></a>ServiceBase 不传播 OnStart 异常

#### <a name="details"></a>详细信息

在 .NET Framework 4.7 和更早版本中，服务启动时引发的异常不会传播到 <xref:System.ServiceProcess.ServiceBase.Run%2A?displayProperty=nameWithType> 的调用方。<br/>从面向.NET Framework 4.7.1 的应用程序开始，针对启动失败的服务，运行时会将异常传播到 <xref:System.ServiceProcess.ServiceBase.Run%2A?displayProperty=nameWithType>。

#### <a name="suggestion"></a>建议

服务启动时，如果出现异常，将传播该异常。 这有助于诊断服务启动失败的事例。 <br/>如果不需要此行为，可在应用程序配置文件的 &lt;runtime&gt; 部分中添加以下 &lt;AppContextSwitchOverrides&gt; 元素，从而选择弃用此行为：

```xml
<AppContextSwitchOverrides value="Switch.System.ServiceProcess.DontThrowExceptionsOnStart=true" />
```

如果应用程序面向早于 4.7.1 的版本，但你需要此行为，请将以下 &lt;AppContextSwitchOverrides&gt; 元素添加到应用程序配置文件的 &lt;runtime&gt; 部分：

```xml
<AppContextSwitchOverrides value="Switch.System.ServiceProcess.DontThrowExceptionsOnStart=false" />
```

| “属性”    | “值”       |
|:--------|:------------|
| 范围   | 次要       |
| Version | 4.7.1       |
| 类型    | 重定目标 |

#### <a name="affected-apis"></a>受影响的 API

- <xref:System.ServiceProcess.ServiceBase.Run(System.ServiceProcess.ServiceBase)?displayProperty=nameWithType>
- <xref:System.ServiceProcess.ServiceBase.Run(System.ServiceProcess.ServiceBase[])?displayProperty=nameWithType>
