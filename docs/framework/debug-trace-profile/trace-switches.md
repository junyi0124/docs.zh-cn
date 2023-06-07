---
title: 跟踪开关
description: 探索跟踪开关，用于启用、禁用和筛选跟踪输出。 .NET 提供了 BooleanSwitch、TraceSwitch 和 SourceSwitch 类。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- tracing [.NET Framework], trace switches
- trace switches, about trace switches
- tracing [.NET Framework], level of detail
- switches, trace
- trace switches
- trace switches, creating custom
ms.assetid: 8ab913aa-f400-4406-9436-f45bc6e54fbe
ms.openlocfilehash: dfbff0a78b3c6c1318224ccc9608c1b816f9d5f1
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96238049"
---
# <a name="trace-switches"></a>跟踪开关

跟踪开关用于启用、禁用和筛选跟踪输出。 它们是代码中存在并可通过 .config 文件在外部配置的对象。 .NET Framework 中提供三种跟踪开关类型： <xref:System.Diagnostics.BooleanSwitch> 类、 <xref:System.Diagnostics.TraceSwitch> 类和 <xref:System.Diagnostics.SourceSwitch> 类。 <xref:System.Diagnostics.BooleanSwitch> 类充当切换开关，可启用或禁用各种跟踪语句。 使用 <xref:System.Diagnostics.TraceSwitch> 和 <xref:System.Diagnostics.SourceSwitch> 类，可以启用特定跟踪级别的跟踪开关，以确保出现为该级别以及其下所有级别指定的 <xref:System.Diagnostics.Trace> 或 <xref:System.Diagnostics.TraceSource> 消息。 如果禁用此开关，则不会出现跟踪消息。 所有这些类均派生自抽象 (MustInherit **T:System.Diagnostics.Switch**) 类 **Switch**，用户开发的任何开关也应如此。  
  
 跟踪开关可用于筛选信息。 例如，你可能希望查看数据访问模块中的每条跟踪消息，但只查看应用程序其余部分的错误消息。 在这种情况下，则需要分别对数据访问模块和应用程序的其余部分使用一个跟踪开关。 通过使用 .config 文件将开关配置为相应的设置，便可以控制接收的跟踪消息类型。 有关详细信息，请参阅[如何：创建、初始化和配置跟踪开关](how-to-create-initialize-and-configure-trace-switches.md)。  
  
 通常情况下，会在禁用开关的情况下执行已部署的应用程序，以确保在应用程序运行时用户不会看到屏幕上出现的或填满日志文件的不相关跟踪消息。 如果应用程序执行期间出现了问题，可以停止应用程序，启用开关，然后重新启动应用程序。 然后，将显示跟踪消息。  
  
 若要使用开关，首先必须在 BooleanSwitch **T:System.Diagnostics.Switch** 类、 **TraceSwitch** 类或开发人员定义的开关类中创建一个开关对象。 有关创建开发人员定义的开关的详细信息，请参阅 .NET Framework 引用中的 <xref:System.Diagnostics.Switch> 类。 然后，将设置一个指定何时使用该开关对象的配置值。 然后，可以在各种 Trace **T:System.Diagnostics.Switch** （或 **Debug**）跟踪方法中测试该开关对象的设置。  
  
## <a name="trace-levels"></a>跟踪级别  

 使用 TraceSwitch **T:System.Diagnostics.Switch** 时，存在其他注意事项。 TraceSwitch **T:System.Diagnostics.Switch** 对象具有四个属性，这四个属性返回指示开关是否设置为至少一个特殊级别的 **Boolean** 值：  
  
- <xref:System.Diagnostics.TraceSwitch.TraceError%2A?displayProperty=nameWithType>  
  
- <xref:System.Diagnostics.TraceSwitch.TraceWarning%2A?displayProperty=nameWithType>  
  
- <xref:System.Diagnostics.TraceSwitch.TraceInfo%2A?displayProperty=nameWithType>  
  
- <xref:System.Diagnostics.TraceSwitch.TraceVerbose%2A?displayProperty=nameWithType>  
  
 通过设置级别，可以将接收的跟踪信息数量限制为仅解决问题所需的信息。 通过将跟踪开关设置和配置为相应的跟踪级别，可以指定跟踪输出的详细级别。 可以选择接收错误消息、警告消息、信息性消息、详细跟踪消息，也可以选择不接收任何消息。  
  
 这完全取决于你自身确定与每个级别关联的消息类型。 通常，跟踪消息的内容取决于每个级别关联的内容，但你可以确定级别之间的差异。 例如，你可能希望在第 3 级别 (Info **T:System.Diagnostics.Switch**) 提供问题的详细说明，但在第 1 级别 (**Error**) 仅提供错误参考号。 这完全取决于你自身确定的最适合应用程序的方案。  
  
 这些属性与 TraceLevel **T:System.Diagnostics.Switch** 枚举的值 1-4 相对应。 下表列出 TraceLevel **T:System.Diagnostics.Switch** 枚举的级别与对应的值。  
  
|枚举值|整数值|所显示的（或写入指定输出目标）的消息类型|  
|----------------------|-------------------|---------------------------------------------------------------------------|  
|关闭|0|无|  
|错误|1|仅错误消息|  
|警告|2|警告消息和错误消息|  
|信息|3|信息性消息、警告消息和错误消息|  
|“详细”|4|详细消息、信息性消息、警告消息和错误消息|  
  
 TraceSwitch **T:System.Diagnostics.Switch** 属性指示开关的最大跟踪级别。 也就是说，将在指定的级别以及其下的所有级别写入跟踪信息。 例如，如果 TraceInfo **T:System.Diagnostics.Switch** 为 **true**，则 **TraceError** 和 **TraceWarning** 也为 **true** ，但 **TraceVerbose** 可能为 **false**。  
  
 这些属性是只读的。 当设置了 **TraceLevel** 属性时，TraceSwitch **T:System.Diagnostics.Switch** 对象会自动对这些属性进行设置。 例如：  
  
```vb  
Dim myTraceSwitch As New TraceSwitch("SwitchOne", "The first switch")  
myTraceSwitch.Level = TraceLevel.Info  
' This message box displays true, because setting the level to  
' TraceLevel.Info sets all lower levels to true as well.  
MessageBox.Show(myTraceSwitch.TraceWarning.ToString())  
' This messagebox displays false.  
MessageBox.Show(myTraceSwitch.TraceVerbose.ToString())  
```  
  
```csharp  
System.Diagnostics.TraceSwitch myTraceSwitch =
   new System.Diagnostics.TraceSwitch("SwitchOne", "The first switch");  
myTraceSwitch.Level = System.Diagnostics.TraceLevel.Info;  
// This message box displays true, because setting the level to
// TraceLevel.Info sets all lower levels to true as well.  
MessageBox.Show(myTraceSwitch.TraceWarning.ToString());  
// This message box displays false.  
MessageBox.Show(myTraceSwitch.TraceVerbose.ToString());  
```  
  
## <a name="developer-defined-switches"></a>开发人员定义的开关  

 除了提供 BooleanSwitch **T:System.Diagnostics.Switch** 和 **TraceSwitch** 之外，可以通过从 **Switch** 类继承以及将基类方法重写为自定义的方法定义自己的开关。 有关创建开发人员定义的开关的详细信息，请参阅 .NET Framework 引用中的 <xref:System.Diagnostics.Switch> 类。  
  
## <a name="see-also"></a>另请参阅

- [跟踪侦听器](trace-listeners.md)
- [如何：向应用程序代码添加跟踪语句](how-to-add-trace-statements-to-application-code.md)
- [跟踪应用程序和在应用程序中插入检测点](tracing-and-instrumenting-applications.md)
