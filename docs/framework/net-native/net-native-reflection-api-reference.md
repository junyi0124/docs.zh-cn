---
title: .NET Native 本机反射 API 参考
ms.date: 03/30/2017
ms.assetid: 0429c049-22a3-4ba1-9cc8-f6ee91e31d9c
ms.openlocfilehash: 4cded310397ffa4dea057899b6f008146d35a03b
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96250874"
---
# <a name="net-native-reflection-api-reference"></a>.NET Native 本机反射 API 参考

.NET Native 包括三个新的异常类型： MissingInteropDataException、MissingMetadataException 和 MissingRuntimeArtifactException。 [system.runtime.compilerservices.](missinginteropdataexception-class-net-native.md)、 [System.Reflection.MissingMetadataException](missingmetadataexception-class-net-native.md)和[System.Reflection.MissingRuntimeArtifactException](missingruntimeartifactexception-class-net-native.md)。 请注意有关所有三种异常类型的以下内容：  
  
 这些类型仅供内部使用。  
 这三种异常类型仅用于 .NET Native 工具链。 当 .NET Native 工具链检测到不允许程序继续执行的缺失数据时，将引发异常。  
  
 不在代码中处理这些异常。  
 这些异常指示缺少应用程序所需的元数据（ [MissingInteropDataException](missinginteropdataexception-class-net-native.md) 和 [MissingMetadataException](missingmetadataexception-class-net-native.md) 异常）或缺少应用程序所需的实现代码（ [MissingRuntimeArtifactException](missingruntimeartifactexception-class-net-native.md) 异常）。 可以通过修改运行时指令 (.rd.xml) 文件，使所需的元数据或实现代码在运行时可用，从而更正这些异常条件。 有关更多信息，请参见 [Runtime Directives (rd.xml) Configuration File Reference](runtime-directives-rd-xml-configuration-file-reference.md)。 有两个故障排除程序可用于为运行时指令文件提供合适的条目，指令文件将消除 [MissingMetadataException](missingmetadataexception-class-net-native.md) 和 [MissingRuntimeArtifactException](missingruntimeartifactexception-class-net-native.md) 异常：  
  
- 类型的 [MissingMetadataException 故障排除程序](https://dotnet.github.io/native/troubleshooter/type.html) 。  
  
- 方法的 [MissingMetadataException 故障排除程序](https://dotnet.github.io/native/troubleshooter/method.html) 。  
  
> [!NOTE]
> 此引用记录了三种对 .NET Native 唯一的异常类型。 有关 .NET Framework 核心反射 API 的参考文档，请参阅 <xref:System.Reflection> <xref:System.Reflection.Context> 和 <xref:System.Reflection.Emit> 命名空间。 要查看 .NET Framework 核心互操作 API 的应用文档，请参阅 <xref:System.Runtime.InteropServices>。  
  
## <a name="systemreflection-namespace"></a>System.Reflection 命名空间  

 <xref:System.Reflection> 命名空间包含用于 .NET Framework 中的反射的核心类型。 对于 .NET Native，它还包括两个新的异常类型：  
  
|实例|描述|  
|-----------|-----------------|  
|[MissingMetadataException](missingmetadataexception-class-net-native.md)|当反射用于检索不存在的元数据时会引起此异常。|  
|[MissingRuntimeArtifactException](missingruntimeartifactexception-class-net-native.md)|当一个类型或类型成员的元数据可用但其实现已遭到删除时会引发此异常。|  
  
 要查看有关此命名空间中其他类型的文档，请参阅 .NET Framework 文档集中的 <xref:System.Reflection> 引用页面。  
  
## <a name="systemruntimecompilerservices-namespace"></a>System.Runtime.CompilerServices 命名空间  

 <xref:System.Runtime.CompilerServices> 命名空间包括通过语言编译器为用户设计的类型。 对于 .NET Native，它还包含一个新的异常类型：  
  
|实例|描述|  
|-----------|-----------------|  
|[MissingInteropDataException](missinginteropdataexception-class-net-native.md)|当手动封送方法被调用但一个类型的元数据无法通过动态分析找到或无法在运行时指令文件中找到时，会引发该异常。|  
  
 要查看有关此命名空间中其他类型的文档，请参阅 .NET Framework 文档集中的 <xref:System.Runtime.CompilerServices> 引用页面。  
  
## <a name="see-also"></a>另请参阅

- [MissingInteropDataException 类](missinginteropdataexception-class-net-native.md)
- [MissingMetadataException 类](missingmetadataexception-class-net-native.md)
- [MissingRuntimeArtifactException 类](missingruntimeartifactexception-class-net-native.md)
- [入门](getting-started-with-net-native.md)
