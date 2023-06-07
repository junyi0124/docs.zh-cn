---
title: 如何：创建、初始化和配置跟踪开关
description: 使用 .NET 中的 BooleanSwitch 和 TraceSwitch 类创建、初始化和配置跟踪开关。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- trace switches, configuring
- tracing [.NET Framework], trace switches
- switches, trace
- tracing [.NET Framework], enabling or disabling
- Web.config configuration file, trace switches
ms.assetid: 5a0e41bf-f99c-4692-8799-f89617f5bcf9
ms.openlocfilehash: 4e8c076645f19e197ea820c052b20d5dde5e7565
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96272249"
---
# <a name="how-to-create-initialize-and-configure-trace-switches"></a>如何：创建、初始化和配置跟踪开关

跟踪开关用于启用、禁用和筛选跟踪输出。  
  
<a name="create"></a>

## <a name="creating-and-initializing-a-trace-switch"></a>创建和初始化跟踪开关  

 为了使用跟踪开关，必须首先创建跟踪开关并将其放置在代码中。 有两种预定义的类可用于创建开关对象：<xref:System.Diagnostics.BooleanSwitch?displayProperty=nameWithType> 类和 <xref:System.Diagnostics.TraceSwitch?displayProperty=nameWithType> 类。 如果您只关心跟踪消息是否出现，应使用 <xref:System.Diagnostics.BooleanSwitch>；如果您需要区别不同的跟踪级别，应使用 <xref:System.Diagnostics.TraceSwitch>。 如果使用 <xref:System.Diagnostics.TraceSwitch>，则可以定义您自己的调试消息并将其与不同的跟踪级别相关联。 您可以将这两种开关用于跟踪或调试。 默认情况下，将禁用 <xref:System.Diagnostics.BooleanSwitch>，并将 <xref:System.Diagnostics.TraceSwitch> 设置为 <xref:System.Diagnostics.TraceLevel.Off?displayProperty=nameWithType> 级别。 可在任何需要使用跟踪开关的位置创建和放置跟踪开关。  
  
 虽然可以在代码中设置跟踪级别和其他配置选项，但最好使用配置文件来管理开关的状态。 这是因为，通过配置系统管理开关配置可获得较大的灵活性，您无需重新编译应用程序，就可以打开和关闭各个开关并更改级别。  
  
#### <a name="to-create-and-initialize-a-trace-switch"></a>若要创建和初始化跟踪开关  
  
1. 将开关定义为 <xref:System.Diagnostics.BooleanSwitch?displayProperty=nameWithType> 类型或 <xref:System.Diagnostics.TraceSwitch?displayProperty=nameWithType> 类型并设置开关的名称和说明。  
  
2. 配置跟踪开关。 有关详细信息，请参阅 [配置跟踪开关](#configure)。  
  
     以下代码创建两个开关，每种类型一个开关：  
  
    ```vb  
    Dim dataSwitch As New BooleanSwitch("Data", "DataAccess module")  
    Dim generalSwitch As New TraceSwitch("General", "Entire application")  
    ```  
  
    ```csharp  
    System.Diagnostics.BooleanSwitch dataSwitch =
       new System.Diagnostics.BooleanSwitch("Data", "DataAccess module");  
    System.Diagnostics.TraceSwitch generalSwitch =
       new System.Diagnostics.TraceSwitch("General",
       "Entire application");  
    ```  
  
<a name="configure"></a>

## <a name="configuring-trace-switches"></a>配置跟踪开关  

 分发应用程序后，你仍可以通过在应用程序中配置跟踪开关来启用或禁用跟踪输出。 配置一个开关意味着在其初始化后，从外部源中更改其值。 可以使用配置文件更改开关对象的值。 配置跟踪开关，可以将其打开和关闭，或设置其级别，从而确定它传递到侦听器的消息量和消息类型。  
  
 使用 .config 文件配置开关。 对于 Web 应用程序，这是与项目关联的 Web.config 文件。 在 Windows 应用程序中，此文件命名 (应用程序名称) # A0。在已部署的应用程序中，此文件必须与可执行文件位于同一文件夹中。  
  
 应用程序执行首次创建交换机的实例的代码时，它会检查有关命名开关跟踪级别信息的配置文件。 跟踪系统仅为任一特定的开关检查一次配置文件 — 即应用程序首次创建开关时。  
  
 在已部署的应用程序中，应用程序未运行时重新配置开关对象可启用跟踪代码。 通常，这涉及到打开和关闭开关对象或更改跟踪级别，然后重新启动应用程序。  
  
 创建开关实例时，还可以通过指定 displayName 和 description 这两个参数来初始化此实例。 构造函数的 displayName 参数设置 <xref:System.Diagnostics.Switch> 类实例的 <xref:System.Diagnostics.Switch.DisplayName%2A?displayProperty=nameWithType> 属性。 displayName 是用于在 .config 文件中配置开关的名称，description 参数应返回开关及其所控制的消息的简要说明 。  
  
 除了指定要配置的开关名称，还必须指定此开关的值。 此值是一个整数。 对于 <xref:System.Diagnostics.BooleanSwitch>，0 值对应“关闭”，而任何非零值对应“打开”。 对于 <xref:System.Diagnostics.TraceSwitch>，0、1、2、3 和 4 分别对应“关闭”、“错误”、“警告”、“信息”和“详细”。 任何大于 4 的数字将被视为“详细”，而任何小于零的数字将被视为“关闭”。  
  
> [!NOTE]
> 在 .NET Framework 2.0 版中，你可以使用文本指定开关值。 例如，为 <xref:System.Diagnostics.BooleanSwitch> 指定 `true` 或表示枚举值的文本，例如为 <xref:System.Diagnostics.TraceSwitch> 指定 `Error`。 行 `<add name="myTraceSwitch" value="Error" />` 等于 `<add name="myTraceSwitch" value="1" />`。  
  
 为了使最终用户能够配置应用程序的跟踪开关，你必须提供关于应用程序中开关的详细文档。 应该详细说明哪些开关控制哪些内容以及如何将其打开和关闭。 应向你的最终用户提供 .config 文件，此文件在注释中具有适当的帮助。  
  
#### <a name="to-configure-trace-switches"></a>配置跟踪开关  
  
1. 要使用跟踪开关，必须根据[创建和初始化跟踪开关](#create)中所述的内容首先创建跟踪开关，然后将它们放入代码中。  
  
2. 如果项目不包含配置文件（app.config 或 Web.config），则从“项目”菜单中选择“添加新项”。  
  
    - **Visual Basic：** 在“添加新项”对话框中，选择“应用程序配置文件”。  
  
         创建并打开应用程序配置文件。 这是一个根元素为 `<configuration>.` 的 XML 文档  
  
    - **Visual C#：** 在“添加新项”对话框中，选择“XML 文件”。 将此文件命名 **app.config**。在 XML 编辑器中，在 XML 声明的后面添加以下 XML：  
  
        ```xml  
        <configuration>  
        </configuration>  
        ```  
  
         编译项目后，app.config 文件会被复制到项目输出文件夹中，并重命名为 applicationname.exe.config。  
  
3. 在 `<configuration>` 标记之后但在 `</configuration>` 标记前面，添加适当的 XML 以配置开关。 以下示例演示了一个 DisplayName 属性为 `DataMessageSwitch` 的 BooleanSwitch，以及一个 DisplayName 属性为 `TraceLevelSwitch` 的 TraceSwitch。  
  
    ```xml  
    <system.diagnostics>  
       <switches>  
          <add name="DataMessagesSwitch" value="0" />  
          <add name="TraceLevelSwitch" value="0" />  
       </switches>  
    </system.diagnostics>  
    ```  
  
     在此配置中，两个开关均为关闭。  
  
4. 如果需要开启 BooleanSwitch，如前面示例中所示的 `DataMessagesSwitch`，请将“Value”更改为 0 以外的任何整数。  
  
5. 如果需要开启 TraceSwitch，如前面示例中所示的 `TraceLevelSwitch`，请将“Value”更改为适当的级别设置 (1-4)。  
  
6. 将注释添加到 .config 文件，以便最终用户清楚地了解适当地配置开关所需更改的值。  
  
     以下示例显示最终代码（包括注释）：  
  
    ```xml  
    <system.diagnostics>  
       <switches>  
          <!-- This switch controls data messages. In order to receive data   
             trace messages, change value="0" to value="1" -->  
          <add name="DataMessagesSwitch" value="0" />  
          <!-- This switch controls general messages. In order to   
             receive general trace messages change the value to the   
             appropriate level. "1" gives error messages, "2" gives errors   
             and warnings, "3" gives more detailed error information, and   
             "4" gives verbose trace information -->  
          <add name="TraceLevelSwitch" value="0" />  
       </switches>  
    </system.diagnostics>  
    ```  
  
## <a name="see-also"></a>另请参阅

- [跟踪应用程序和在应用程序中插入检测点](tracing-and-instrumenting-applications.md)
- [如何：向应用程序代码添加跟踪语句](how-to-add-trace-statements-to-application-code.md)
- [跟踪开关](trace-switches.md)
- [跟踪和调试设置架构](../configure-apps/file-schema/trace-debug/index.md)
