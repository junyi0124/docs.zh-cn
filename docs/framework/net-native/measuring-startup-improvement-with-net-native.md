---
title: 使用 .NET Native 衡量启动改善
ms.date: 03/30/2017
ms.assetid: c4d25b24-9c1a-4b3e-9705-97ba0d6c0289
ms.openlocfilehash: 6d89edaff184692eabb11e928f5211f664ff5afa
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96250965"
---
# <a name="measuring-startup-improvement-with-net-native"></a>使用 .NET Native 衡量启动改善

.NET Native 显著改善了应用的启动时间。 这一改善在便携式、低功耗设备上和在使用复杂应用时尤其明显。 该主题将帮助你初步了解衡量这个启动提升所需的基本检测。  
  
 为方便性能调查，.NET Framework 和 Windows 使用一个名为 Windows 事件跟踪 (ETW) 的事件框架，它允许你的应用在事件发生时通知工具。 然后你可以使用一个名为 PerfView 的工具查看和分析 ETW 事件。 该主题解释了如何：  
  
- 使用 <xref:System.Diagnostics.Tracing.EventSource> 类来发出事件。  
  
- 使用 PerfView 来收集这些事件。  
  
- 使用 PerfView 来显示这些事件。  
  
## <a name="using-eventsource-to-emit-events"></a>使用 EventSource 来发出事件  

 <xref:System.Diagnostics.Tracing.EventSource> 提供了一个基类，从该基类可以创建自定义事件提供程序。 通常，你可以创建 <xref:System.Diagnostics.Tracing.EventSource> 的一个子类并使用你自己的事件方法环绕 `Write*` 方法。 一个单独模式通常用于每个 <xref:System.Diagnostics.Tracing.EventSource>。  
  
 例如，以下实例中的类可以用来衡量两个性能特征：  
  
- `App` 类构造函数得到调用所花时间。  
  
- `MainPage` 构造函数得到调用所花时间。  
  
 [!code-csharp[ProjectN_ETW#1](../../../samples/snippets/csharp/VS_Snippets_CLR/projectn_etw/cs/etw1.cs#1)]  
  
 有几个需要记住的要点。 首先，在 `AppEventSource.Log` 中创建一个单独实例。 该实例将用于所有记录。 其次，每个事件方法都有一个 <xref:System.Diagnostics.Tracing.EventAttribute>。 这将帮助工具将 <xref:System.Diagnostics.Tracing.EventSource.WriteEvent%2A> 方法的索引与在 `AppEventSource` 上调用的方法关联起来。  
  
 注意，这些事件纯粹是说明性的。 大多数应用代码会在这些事件之后运行。 你应该了解代码中的哪些事件对应于用户交互，衡量它们并提高这些基准。 并且，这些事件本身每次只记录一个实例。 通常让成对的实例为每一步操作开始和停止事件是有用的。 检查应用启动时，开始事件通常是操作系统发出的“Process/Star”事件。  
  
 例如，假设你正在创建一个 RSS 阅读器。 一些记录事件的有趣的时间点是：  
  
- 当主页首次呈现时。  
  
- 当旧的 RSS 故事从本地存储中遭到反序列化时。  
  
- 当你的应用开始同步新故事时。  
  
- 当你的应用已完成同步新故事时。  
  
 检测应用程序非常简单：只需在派生类上调用适当的方法。 通过使用前面实例中的 `AppEventSource`，你可以按照如下所示检测一个应用：  
  
 [!code-csharp[ProjectN_ETW#2](../../../samples/snippets/csharp/VS_Snippets_CLR/projectn_etw/cs/etw2.cs#2)]  
  
 当应用得到检测后，你就可以收集事件。  
  
## <a name="gathering-events-with-perfview"></a>使用 PerfView 收集事件  

 PerfView 使用 ETW 事件帮助你在设备上进行各种性能研究。 它还包括一个配置 GUI，这允许你打开或关闭事件的不同类型的记录。 PerfView 是一个免费的工具，可从 [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=28567) 下载。 有关详细信息，请观看 [PerfView 教程视频](https://channel9.msdn.com/Series/PerfView-Tutorial).  
  
> [!NOTE]
> PerfView 无法用于收集 ARM 系统上的事件。 有收集 ARM 系统上的事件，请使用 Windows 性能记录器 (WPR)。 有关详细信息，请参阅 [Vance Morrison 的博客文章](/archive/blogs/vancem/collecting-etwperfview-data-on-an-windows-rt-winrt-arm-surface-device)。  
  
 你也可以从命令行调用 PerfView。 如仅要记录来自你的提供程序的事件，请打开“命令提示符”窗口并输入以下命令：  
  
```console
perfview -KernelEvents:Process -OnlyProviders:*MyCompany-MyApp collect outputFile
```  
  
 其中：  
  
 `-KernelEvents:Process`  
 显示你想知道程序何时启动和停止。 你需要为你的应用设置“进程/开始”事件，才能使其从其他事件时间被扣除。  
  
 `-OnlyProviders:*MyCompany-MyApp`  
 关闭 PerfView 默认开启的其他提供程序，并开启 MyCompany-MyApp 提供程序。  （此处的星号表示 <xref:System.Diagnostics.Tracing.EventSource>.）  
  
 `collect outputFile`  
 显示你想开始收集数据并将其保存到 outputFile.etl.zip。  
  
 在开启 PerfView 后运行你的应用。 运行你的程序时有几个需要谨记的要点：  
  
- 使用发布版本，而不是调试版本。 调试版本通常包含额外的错误检查和错误处理代码，会导致你的应用运行的速度比预期的要慢。  
  
- 运行附带调试程序的应用会影响应用性能。  
  
- Windows 使用多种缓存策略来缩短应用启用时间。 如果你的应用目前缓存在内存中并且不需要从磁盘中加载，它的启动速度会更快。 要确保一致性，在衡量你的应用之前先将其启动和关闭数次。  
  
 在已运行应用以便 PerfView 可以收集发出的事件时，选择“停止收集”按钮。 通常，你应该在关闭应用之前停止收集，这样就不会收集到无关的事件。 然而，如果你正在衡量关机或暂停性能，就应该继续收集。  
  
## <a name="displaying-the-events"></a>正在显示事件  

 要查看已经收集到的事件，请使用 PerfView 打开所创建的 .etl 或 .etl.zip 文件并选择“事件”。 ETW 将会收集有关大量事件的信息，包括来自其他进程的事件。 要专注于你的调查，完成以下事件视图中的文本框：  
  
- 在“进程筛选器”文本框中，指定应用名称（不要包含“.exe”）。  
  
- 在“事件类型筛选器”框中，指定 `Process/Start | MyCompany-MyApp`。 这为来自 MyCompany-MyApp 的事件和“Windows 内核/进程/开始”事件设置了一个筛选器。  
  
 选中左窗格中列出的所有事件 (Ctrl-A) 并选择“Enter”键。 现在你能够查看每个事件的时间戳了。 这些时间戳是从跟踪开始算起的，所以你必须用进程的开始时间减去每个事件的时间，才能确定启动花费的时间。 如果你使用“Ctrl+单击”选中了两个时间戳，你会在页面底部看到在状态栏中显示的他们之间的区别。 这使得要在显示中查看任何两个事件之间的时间间隔变得简单（包括进程开始）。 你可以打开快捷菜单试图并在一些有用的选项中进行选择，比如导出到 CSV 文件或打开 Microsoft Excel 保存或处理这些数据。  
  
 通过对原始应用和使用 .NET Native 工具链生成的版本重复此过程，可以比较性能差异。   .NET Native 应用通常比 non-.NET 本机应用启动更快。 如果你有兴趣更深入了解，PerfView 也可以识别你的应用代码中花费时间最多的部分。 有关详细信息，请观看 [PerfView 教程](https://channel9.msdn.com/Series/PerfView-Tutorial)或读取 [Vance Morrison 的博客文章](/archive/blogs/vancem/publication-of-the-perfview-performance-analysis-tool)。  
  
## <a name="see-also"></a>另请参阅

- <xref:System.Diagnostics.Tracing.EventSource>
