---
title: 'How to: Write Event Information to a Text File'
ms.date: 07/20/2015
helpviewer_keywords:
- event logs [Visual Studio], writing event information
- text files [Visual Basic], writing event information to a text file
- events [Visual Basic], writing event information to a text file
ms.assetid: 9ca7cc03-bf99-4933-9e5e-61ee28e9a6b4
ms.openlocfilehash: 6e83f8450ca7be8a2dcd5ff43eab3dd2ec0d2f1b
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84410057"
---
# <a name="how-to-write-event-information-to-a-text-file-visual-basic"></a>如何：将事件信息写入文本文件 (Visual Basic)

可以使用 `My.Application.Log` 和 `My.Log` 对象来记录有关应用程序中所发生事件的信息。 本示例演示如何使用 `My.Application.Log.WriteEntry` 方法将跟踪信息记录到日志文件中。

### <a name="to-add-and-configure-the-file-log-listener"></a>添加和配置文件日志侦听器

1. 在“解决方案资源管理器”  中右键单击 app.config，然后选择“打开”  。

     \- 或 -

     如果其中没有 app.config 文件：

    1. 在 **“项目”** 菜单上选择 **“添加新项”** 。

    2. 在“添加新项”  对话框中，选择“应用程序配置文件”  。

    3. 单击 **添加**。

2. 在应用程序配置文件中找到 `<listeners>` 部分。

     \<listeners> 部分位于 name 属性为“DefaultSource”的 \<source> 部分当中，后者又嵌套在 \<system.diagnostics> 部分当中，位于顶级 \<configuration> 部分之下。

3. 将此元素添加到该 `<listeners>` 部分：

    ```xml
    <add name="FileLogListener" />
    ```

4. 找到 `<sharedListeners>` 部分，该部分位于 `<system.diagnostics>` 部分中，后者嵌套在顶级 `<configuration>` 部分之下。

5. 将此元素添加到该 `<sharedListeners>` 部分：

    ```xml
    <add name="FileLogListener"
        type="Microsoft.VisualBasic.Logging.FileLogTraceListener,
              Microsoft.VisualBasic, Version=8.0.0.0, Culture=neutral,
              PublicKeyToken=b03f5f7f11d50a3a"
        initializeData="FileLogListenerWriter"
        location="Custom"
        customlocation="c:\temp\" />
    ```

     将 `customlocation` 属性的值更改为日志目录。

    > [!NOTE]
    > 若要设置侦听器属性的值，请使用与该属性具有相同名称的特性，名称中的所有字母都为小写。 例如，`location` 和 `customlocation` 属性设置 <xref:Microsoft.VisualBasic.Logging.FileLogTraceListener.Location%2A> 和 <xref:Microsoft.VisualBasic.Logging.FileLogTraceListener.CustomLocation%2A> 属性的值。

### <a name="to-write-event-information-to-the-file-log"></a>将事件信息写入文件日志

可以使用 `My.Application.Log.WriteEntry` 或 `My.Application.Log.WriteException` 方法将信息写入文件日志。 有关详细信息，请参阅[如何：编写日志消息](how-to-write-log-messages.md)和[如何：记录异常](how-to-log-exceptions.md)。

为程序集配置文件日志侦听器后，它将接收该程序集写入 `My.Application.Log` 的所有消息。

## <a name="see-also"></a>另请参阅

- <xref:Microsoft.VisualBasic.Logging.Log?displayProperty=nameWithType>
- <xref:Microsoft.VisualBasic.Logging.Log.WriteEntry%2A>
- <xref:Microsoft.VisualBasic.Logging.Log.WriteException%2A>
- [使用应用程序日志](working-with-application-logs.md)
- [如何：日志异常](how-to-log-exceptions.md)
