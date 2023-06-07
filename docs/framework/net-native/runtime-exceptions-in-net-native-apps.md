---
title: .NET 本机应用中的运行时异常
ms.date: 03/30/2017
ms.assetid: 5f050181-8fdd-4a4e-9d16-f84c22a88a97
ms.openlocfilehash: 5c521eed94590e583a761cc2003460875e690fa9
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96287847"
---
# <a name="runtime-exceptions-in-net-native-apps"></a>.NET 本机应用中的运行时异常

请务必在通用 Windows 平台应用程序的目标平台上测试它们的发布版本，因为调试和发布配置完全不同。 默认情况下，调试配置使用 .NET Core 运行时来编译应用程序，但发布配置使用 .NET 本机将应用程序编译为本机代码。  
  
> [!IMPORTANT]
> 有关处理在测试应用的发布版本时可能会遇到的 [MissingMetadataException](missingmetadataexception-class-net-native.md)、 [MissingInteropDataException](missinginteropdataexception-class-net-native.md)和 [MissingRuntimeArtifactException](missingruntimeartifactexception-class-net-native.md) 异常的信息，请参阅 [入门](getting-started-with-net-native.md) 主题中的 "步骤4：手动解析缺少的元数据：" 和 " [反射和 .NET Native](reflection-and-net-native.md) " 和 " [运行时指令" ( # A0) 配置文件引用](runtime-directives-rd-xml-configuration-file-reference.md)。  
  
## <a name="debug-and-release-builds"></a>调试和发布版本  

 当调试版本针对 .NET Core 运行时执行时，它未编译为本机代码。 这使得你的应用可使用通常由运行时提供的所有服务。  
  
 另一方面，发布版本编译为其目标平台的本机代码，删除外部运行时和库的大多数依赖项，并很大程度优化代码以获得最佳性能。  
  
 对使用 .NET 本机编译的发布版本进行调试时：  
  
- 请使用不同于普通的 .NET 调试工具的 .NET 本机调试引擎。  
  
- 尽可能减小了可执行文件的大小。 .NET 本机减小可执行文件的大小的方法之一是大量清理运行时异常消息， [Runtime exception messages](#Messages) 部分中更详细地讨论了这一主题。  
  
- 很大程度优化了你的代码。 这意味着只要有可能就会使用该内联。  (内联将外部例程中的代码移到调用例程中。 ) .NET Native 提供专用运行时并实现主动内联会影响在调试时显示的调用堆栈。  有关详细信息，请参阅 [Runtime call stack](#CallStack) 部分。  
  
> [!NOTE]
> 通过选中或取消选中“使用 .NET 本机工具链进行编译”  框，可以控制是否使用 .NET 本机工具链编译调试和发布版本。   但是，请注意 Windows 应用商店将始终使用 .NET 本机工具链编译你的应用程序的生产版本。  
  
<a name="Messages"></a>

## <a name="runtime-exception-messages"></a>Runtime exception messages  

 为尽量减少应用程序可执行文件的大小，.NET 本机不包含异常消息的完整文本。 因此，在发布版本中引发的运行时异常可能不显示异常消息的完整文本。 相反，该文本可能由一个子字符串和一个链接构成，跟随该链接可获取详细信息。 例如，异常信息可能显示为：  
  
```output
Exception thrown: '$16_System.AggregateException' in Unknown Module.  
  
Additional information: AggregateException_ctor_DefaultMessage  
  
If there is a handler for this exception, the program may be safely continued.  
```  
  
 如果需要完整的异常消息，请改为运行调试版本。 例如，发布版本中的上一个异常信息在调试版本中可能显示如下：  
  
```output
Exception thrown: 'System.AggregateException' in NativeApp.exe.  
  
Additional information: Value does not fall within the expected range.  
```  
  
<a name="CallStack"></a>

## <a name="runtime-call-stack"></a>Runtime call stack  

 由于内联和其他优化，由 .NET 本机工具链编译的应用显示的调用堆栈可能无益于清晰地标识运行时异常的路径。  
  
 若要获取完整的堆栈，请改为运行调试版本。  
  
## <a name="see-also"></a>另请参阅

- [调试 .NET 本机 Windows 通用应用](https://devblogs.microsoft.com/devops/debugging-net-native-windows-universal-apps/)
- [入门](getting-started-with-net-native.md)
