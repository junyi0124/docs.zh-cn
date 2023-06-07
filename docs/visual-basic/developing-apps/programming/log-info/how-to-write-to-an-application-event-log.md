---
title: 如何：写入应用程序事件日志
ms.date: 07/20/2015
helpviewer_keywords:
- Computer.EventLog element
- WriteEntry method [Visual Basic]
- My.Computer.EventLog element
- event logs, writing to
ms.assetid: cadbc8c1-87af-4746-934e-55b79a4f6e2b
ms.openlocfilehash: 298d6d85f8b21176b72db8e676617577eb03fada
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84410031"
---
# <a name="how-to-write-to-an-application-event-log-visual-basic"></a>如何：写入应用程序事件日志 (Visual Basic)

可以使用 `My.Application.Log` 和 `My.Log` 对象来写入有关应用程序中所发生事件的信息。 本示例将演示如何配置事件日志侦听器，以便 `My.Application.Log` 将跟踪信息写入应用程序事件日志。

不能将信息写入安全日志。 只有 LocalSystem 或 Administrator 帐户的成员可以将信息写入系统日志。

若要查看事件日志，可以使用“服务器资源管理器”  或“Windows 事件查看器”。   有关详细信息，请参阅 [ETW Events in the .NET Framework](../../../../framework/performance/etw-events.md)。

## <a name="to-add-and-configure-the-event-log-listener"></a>添加和配置事件日志侦听器

1. 在“解决方案资源管理器”  中右键单击 app.config，然后选择“打开”  。

    \- 或 -

    如果其中没有 app.config 文件，

    1. 在 **“项目”** 菜单上选择 **“添加新项”** 。

    2. 在“添加新项”  对话框中，选择“应用程序配置文件”  。

    3. 单击 **添加**。

2. 在应用程序配置文件中找到 `<listeners>` 部分。

    `<listeners>` 部分位于 name 属性为“DefaultSource”的 `<source>` 部分当中，后者又嵌套在 `<system.diagnostics>` 部分当中，位于顶级 `<configuration>` 部分之下。

3. 将此元素添加到该 `<listeners>` 部分：

    ```xml
    <add name="EventLog"/>
    ```

4. 找到 `<sharedListeners>` 部分，该部分位于 `<system.diagnostics>` 部分当中，后者又位于顶级 `<configuration>` 部分之下。

5. 将此元素添加到该 `<sharedListeners>` 部分：

    ```xml
    <add name="EventLog"
        type="System.Diagnostics.EventLogTraceListener, System, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089"
         initializeData="APPLICATION_NAME"/>
    ```

    将 `APPLICATION_NAME` 替换为应用程序的名称。

    > [!NOTE]
    > 通常情况下，应用程序只将错误信息写入事件日志。 有关筛选日志输出的信息，请参阅 [Walkthrough: Filtering My.Application.Log Output](walkthrough-filtering-my-application-log-output.md)。

## <a name="to-write-event-information-to-the-event-log"></a>将事件信息写入事件日志

使用 `My.Application.Log.WriteEntry` 或 `My.Application.Log.WriteException` 方法可以将信息写入事件日志。 有关详细信息，请参阅[如何：编写日志消息](how-to-write-log-messages.md)和[如何：记录异常](how-to-log-exceptions.md)。

为程序集配置事件日志侦听器后，它将接收该程序集写入 `My.Application.Log` 的所有消息。

## <a name="see-also"></a>另请参阅

- <xref:Microsoft.VisualBasic.Logging.Log?displayProperty=nameWithType>
- <xref:Microsoft.VisualBasic.Logging.Log.WriteEntry%2A>
- <xref:Microsoft.VisualBasic.Logging.Log.WriteException%2A>
- [使用应用程序日志](working-with-application-logs.md)
- [如何：日志异常](how-to-log-exceptions.md)
- [演练：确定 My.Application.Log 写入信息的位置](walkthrough-determining-where-my-application-log-writes-information.md)
