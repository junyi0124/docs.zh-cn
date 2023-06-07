---
title: 反射和 .NET Native
ms.date: 03/30/2017
ms.assetid: 91c9eae4-c641-476c-a06e-d7ce39709763
ms.openlocfilehash: c38070ec4afe0a7311133e0ef7b5b24eb2fe4fb5
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96287834"
---
# <a name="reflection-and-net-native"></a>反射和 .NET Native

在 .NET Framework 中，托管开发通过反射 API 支持元编程。 反射允许你检查应用中的对象，调用通过检查发现的对象上的方法，在运行时间生成性类型，并支持所有其他动态代码方案。 它还支持序列化和反序列化，这允许一个对象的字段值被持久化并在后来得到还原。 这些方案都要求 .NET Framework 及时生成 (JIT) 编译器以可用的元数据为基础生成本机代码。  
  
 .NET Native 运行时不包括 JIT 编译器。 因此，所有所需的本机代码必需提前生成。 一组启发可用于确定应生成哪些代码，但是这些启发不能涵盖所有可能的元编程方案。  因此，必须通过使用[运行时指令](runtime-directives-rd-xml-configuration-file-reference.md)向这些元编程方案提供提示。 如果所需的元数据或实现代码在运行时不可用，应用将引发 [MissingMetadataException](missingmetadataexception-class-net-native.md)、[MissingRuntimeArtifactException](missingruntimeartifactexception-class-net-native.md) 或 [MissingInteropDataException](missinginteropdataexception-class-net-native.md) 异常。 有两个疑难解答程序可用于为运行时指令文件提供合适的条目，指令文件将消除以下异常：  
  
- 类型的 [MissingMetadataException 故障排除程序](https://dotnet.github.io/native/troubleshooter/type.html) 。  
  
- 方法的 [MissingMetadataException 故障排除程序](https://dotnet.github.io/native/troubleshooter/method.html) 。  
  
> [!NOTE]
> 有关提供为何需要运行时指令文件的背景的 .NET 本机编译过程的概述，请参阅 [.NET 本机和编译](net-native-and-compilation.md)。  
  
 此外，.NET Native 不允许反映 .NET Framework 类库的私有成员。 例如，调用 <xref:System.Reflection.TypeInfo.DeclaredFields%2A?displayProperty=nameWithType> 属性来检索一个 .NET Framework 类库类型的字段仅返回公共或受保护的字段。  
  
 以下主题提供了你需要在自己的应用中支持反射和序列化的概念和引用文档：  
  
- [利用反射的 API](apis-that-rely-on-reflection.md)  
  
- [反射 API 引用](net-native-reflection-api-reference.md)  
  
- [运行时指令 (rd.xml) 配置文件引用](runtime-directives-rd-xml-configuration-file-reference.md)  
  
## <a name="see-also"></a>另请参阅

- [使用 .NET Native 编译应用程序](index.md)
- [.NET Native 和编译](net-native-and-compilation.md)
