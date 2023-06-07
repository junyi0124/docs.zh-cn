---
title: 确定 My.Application.Log 写入信息的位置
ms.date: 07/20/2015
helpviewer_keywords:
- My.Log object, output location
- output, application log location
- My.Application.Log object, output location
- event logs, determining output location
- application event logs, output location
- applications [Visual Basic], output location
ms.assetid: 5b70143a-7741-45f2-ae1d-03324a3a4189
ms.openlocfilehash: 00b543dbe96ca99446f6797a13b66ee62c422b93
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84398274"
---
# <a name="walkthrough-determining-where-myapplicationlog-writes-information-visual-basic"></a>演练：确定 My.Application.Log 写入信息的位置 (Visual Basic)

`My.Application.Log` 对象可以将信息写入多个日志侦听器。 日志侦听器由计算机的配置文件配置，并且可以通过应用程序的配置文件重写。 本主题介绍默认设置以及如何确定应用程序的设置。

有关默认输出位置的详细信息，请参阅[使用应用程序日志](working-with-application-logs.md)。

### <a name="to-determine-the-listeners-for-myapplicationlog"></a>确定 My.Application.Log 的侦听器

1. 找到程序集的配置文件。 如果正在开发程序集，则可通过“解决方案资源管理器”访问 Visual Studio 中的 app.config  。 否则，配置文件名称即为程序集的名称附加“.config”，并且与程序集位于相同的目录中。

    > [!NOTE]
    > 不是每个程序集都有配置文件。

    配置文件是一个 XML 文件。

2. 找到 `<listeners>` 部分，该部分位于 `<source>` 属性为“DefaultSource”的 `name` 部分当中，后者又位于 `<sources>` 部分之下。 `<sources>` 部分位于 `<system.diagnostics>` 部分当中，后者又位于顶级 `<configuration>` 部分之下。

    如果这些部分不存在，则计算机的配置文件可能会配置 `My.Application.Log` 日志侦听器。 以下步骤介绍如何确定计算机配置文件定义的内容：

    1. 找到计算机的 machine.config 文件。 通常情况下，该文件位于 *SystemRoot\Microsoft.NET\Framework\frameworkVersion\CONFIG* 目录，其中 `SystemRoot` 是操作系统目录，`frameworkVersion` 是 .NET Framework 的版本。

        machine.config 中的设置可以通过应用程序的配置文件重写。

        如果如下所列的可选元素不存在，可以创建它们。

    2. 找到 `<listeners>` 部分，该部分位于 `<source>` 属性为“DefaultSource”的 `name` 部分当中，后者又位于 `<sources>` 部分当中，这部分位于 `<system.diagnostics>` 部分当中，位于顶级 `<configuration>` 部分之下。

        如果这些部分不存在，则 `My.Application.Log` 将只有默认的日志侦听器。

3. 在 <`listeners>` 部分找到 <`add>` 元素。

     这些元素会将命名的日志侦听器添加到 `My.Application.Log` 源。

4. 在 `<add>` 部分找到具有日志侦听器名称的 `<sharedListeners>` 元素，该部分位于 `<system.diagnostics>` 部分当中，后者又位于顶级 `<configuration>` 部分之下。

5. 对于许多类型的共享侦听器，该侦听器的初始化数据说明了侦听器写入数据的位置：

    - <xref:Microsoft.VisualBasic.Logging.FileLogTraceListener?displayProperty=nameWithType> 侦听器将信息写入文件日志，如简介中所述。

    - <xref:System.Diagnostics.EventLogTraceListener?displayProperty=nameWithType> 侦听器将信息写入 `initializeData` 参数指定的计算机事件日志。 若要查看事件日志，可以使用“服务器资源管理器”  或“Windows 事件查看器”。   有关详细信息，请参阅 [ETW Events in the .NET Framework](../../../../framework/performance/etw-events.md)。

    - <xref:System.Diagnostics.DelimitedListTraceListener?displayProperty=nameWithType> 和 <xref:System.Diagnostics.XmlWriterTraceListener?displayProperty=nameWithType> 侦听器将信息写入 `initializeData` 参数指定的文件。

    - <xref:System.Diagnostics.ConsoleTraceListener?displayProperty=nameWithType> 侦听器将信息写入命令行控制台。

    - 有关其他类型的日志侦听器写入信息的位置的信息，请参阅该类型的文档。

## <a name="see-also"></a>另请参阅

- <xref:Microsoft.VisualBasic.Logging.Log?displayProperty=nameWithType>
- <xref:System.Diagnostics.DefaultTraceListener>
- <xref:System.Diagnostics.EventLogTraceListener>
- <xref:System.Diagnostics.DelimitedListTraceListener>
- <xref:System.Diagnostics.XmlWriterTraceListener>
- <xref:System.Diagnostics.ConsoleTraceListener>
- <xref:System.Diagnostics>
- [使用应用程序日志](working-with-application-logs.md)
- [如何：日志异常](how-to-log-exceptions.md)
- [如何：编写日志消息](how-to-write-log-messages.md)
- [演练：更改 My.Application.Log 写入信息的位置](walkthrough-changing-where-my-application-log-writes-information.md)
- [.NET Framework 中的 ETW 事件](../../../../framework/performance/etw-events.md)
- [疑难解答：日志侦听器](troubleshooting-log-listeners.md)
