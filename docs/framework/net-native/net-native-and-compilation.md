---
title: .NET Native 和编译
ms.date: 03/30/2017
ms.assetid: e38ae4f3-3e3d-42c3-a4b8-db1aa9d84f85
ms.openlocfilehash: cf5c9f05b2f2cb4ca15e4add5b53bc9bdca757a3
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/06/2020
ms.locfileid: "73128245"
---
# <a name="net-native-and-compilation"></a>.NET Native 和编译

面向 .Net Framework 的 Windows 8.1 应用程序和 Windows 桌面应用程序由特定的编程语言进行编写并编译为中间语言 (IL)。 在运行时，实时 (JIT) 编译器负责恰好在首次执行方法前为本地计算机将 IL 编译到本机代码中。 与此相反，.NET 本机工具链在编译时将源代码转换为本机代码。 本主题将 .NET 本机与其他可用于 .NET Framework 应用程序的编译技术进行比较，还提供了 .NET 本机如何生成本机代码的实用概述，可帮助用户了解使用 .NET 本机编译的代码中发生的异常为什么不会出现在 JIT 编译的代码中。

## <a name="net-native-generating-native-binaries"></a>.NET 本机：生成本机二进制文件

面向 .NET Framework 且未使用 .NET 本机工具链编译的应用程序包含应用程序的程序集，其中包括以下内容：

- 用于描述程序集、其依赖项、其包含的类型和其中成员的[元数据](../../standard/metadata-and-self-describing-components.md)。 元数据用于反射和后期绑定访问，在一些情况下也可用于编译器和生成工具。

- 实现代码。 这包括中间语言 (IL) 操作码。 在运行时，实时 (JIT) 编译器将它转换为目标平台的本机代码。

 除了主应用程序集，应用程序还需要存在以下内容：

- 应用程序所需的任何其他类库或第三方程序集。 这些程序集同样包括描述程序集、其类型和其中成员的元数据，以及实现所有类型成员的 IL。

- .NET Framework 类库。 这是与 .NET Framework 一同安装在本地系统上的程序集的集合。 .NET Framework 类库中附带的程序集包括整套元数据和实现代码。

- 公共语言运行时。 这是动态链接库的集合，执行程序集上载、内存管理和垃圾回收、异常处理、实时编译、远程处理和互操作等服务。 和类库一样，运行时与 .NET Framework 一同安装在本地系统中。

请注意，若要应用程序成功执行，必须存在整个公共语言运行时、应用程序特定的程序集中的元数据和 IL、第三方程序集以及系统程序集。

## <a name="net-native-and-just-in-time-compilation"></a>.NET 本机和实时编译

.NET 本机工具链的输入是指 C# 或 Visual Basic 编译器生成的 Windows 应用商店应用。 换言之，.NET 本机工具链在语言编译器完成 Windows 应用商店应用的编译时开始执行。

> [!TIP]
> 由于 .NET Native 的输入是写入托管程序集的 IL 和元数据，因此仍然可以通过使用预生成（或后期生成）的事件或通过修改 MSBuild 项目文件执行自定义代码生成或其他自定义操作。
>
> 然而，不支持修改 IL 进而阻止 .NET 工具链分析应用 IL 的工具的类别。 模糊处理程序是此类型中最值得注意的工具。

在将应用程序从 IL 转换为本机代码的过程中，.NET Native 工具链执行如下所示的操作：

- 对于某些代码路径，它将依靠反射和元数据的代码替换为静态本机代码。 例如，如果值类型未重写 <xref:System.ValueType.Equals%2A?displayProperty=nameWithType> 方法，默认的相等性测试将使用反射来检索表示值类型字段的 <xref:System.Reflection.FieldInfo> 对象，然后将两个实例的字段值进行比较。 编译为本机代码时，.NET Native 工具链将反射代码和元数据替换为字段值的静态比较。

- 如果可能，它会尝试消除所有元数据。

- 它只将实际由应用程序调用的实现代码包含在最终应用程序集中。 这尤其会对第三方库和 .NET Framework 类库中的代码产生影响。 因此，应用程序不再依赖第三方库或完整的 .NET Framework 类库；相反，对应用程序而言，当前第三方和 .NET Framework 类库中的代码都是本地的。

- 它将完整的 CLR 替换为主要包含垃圾回收器的重构运行时。 重构运行时位于应用程序中名为 mrt100_app.dll 本地库，且其大小仅为几百千字节。 这可能是因为静态链接不再需要公共语言运行时执行多个服务。

  > [!NOTE]
  > .NET 本机使用的垃圾回收器与标准公共语言运行时使用的相同。 在 .NET 本机垃圾回收器中，默认启用后台垃圾回收。 有关垃圾回收的详细信息，请参阅[垃圾回收的基础知识](../../standard/garbage-collection/fundamentals.md)。

> [!IMPORTANT]
> .NET 本机将整个应用程序编译到本机应用程序。 它不允许将包含类库的单个程序集编译为本机代码，所以可独立于托管代码进行调用。

.NET 本机工具链生成的应用程序的写入位置如下：项目目录下 Debug 或 Release 目录中名为 ilc.out 的目录。 它包含以下文件：

- *\<appName>*.exe，一种存根可执行文件，它只是将控制转移到 `Main` .dll 中的特殊导出 *\<appName>* 。

- *\<appName>*.dll：包含所有应用程序代码的 Windows 动态链接库，以及来自 .NET Framework 类库和你依赖的任何第三方库的代码。  还包含支持代码，例如与 Windows 互操作和序列化应用程序中的对象的必要代码。

- mrt100_app.dll，指提供运行时服务（如垃圾回收）的重构运行时。

 所有依赖项均由应用程序的 APPX 清单捕获。  除了在 appx 包中直接绑定的应用程序 exe、dll 和 mrt100_app.dll，还包括以下两个文件：

- mrt100_app.dll 使用的 C 运行时 (CRT) 库 msvcr140_app.dll。 它由架构引用包含在程序包内。

- mrt100.dll。 此库包含可提高 mrt100_app.dll 性能的函数，但没有此库，mrt100_app.dll 也可正常运行。 如果此库存在，可从本地计算机上的 system32 目录加载它。

因为 .NET Native 工具链只在获知应用程序实际调用了实现代码时才会将它链接到应用程序中，所以应用程序中可能不包含以下方案中所需的元数据或实现代码：

- 反射。

- 动态或后期绑定调用。

- 序列化和反序列化。

- COM 互操作。

如果在运行时缺少必需的元数据或实现代码，.NET Native 运行时将引发异常。 可通过使用[运行时指令文件](runtime-directives-rd-xml-configuration-file-reference.md)（即，可指定其元数据或实现代码必须在运行时可用的程序元素并向其分配运行时策略的 XML 文件），防止这些异常并确保 .NET Native 工具链包含所需的元数据和实现代码。 下面是添加到 .NET 本机工具链编译的 Windows 应用商店项目中的默认运行时指令文件：

```xml
<Directives xmlns="http://schemas.microsoft.com/netfx/2013/01/metadata">
  <Application>
    <Assembly Name="*Application*" Dynamic="Required All" />
  </Application>
</Directives>
```

通过它，应用程序包中所有程序集的所有类型和所有成员都可执行反射和动态调用。 然而，.NET Framework 类库程序集中的类型无法借此执行反射或动态激活。 在很多情况下，这已足够。

## <a name="net-native-and-ngen"></a>.NET 本机和 NGEN

[本机映像生成器](../tools/ngen-exe-native-image-generator.md) (NGEN) 将程序集编译为本机代码，并将其安装在本地计算机的本机映像缓存中。 然而，尽管 NGEN 与 .NET Native 一样都生成本机代码，但在一些重要方面它又与 .NET 本机不同：

- 如果特定方法中没有可用的本机映像，NGEN 将转而使用 JITing 代码。 这意味着本机映像必须继续在 NGEN 需要回退到 JIT 编译的事件中包括元数据和 IL。 与此相反，.NET 本机仅生成本机映像并且不会回退到 JIT 编译。 因此，仅必须保留某些反射、序列化和互操作方案所需的元数据。

- NGEN 继续依赖完整公共语言运行时来执行程序集加载、远程处理、互操作、内存管理、垃圾回收以及 JIT 编译（若必要）等服务。 在 .NET 本机中，其中很多服务都是不必要的（如 JIT 编译），或者已在生成时进行解析并纳入应用程序集。 其余服务（其中最重要的是垃圾回收）均包含在名为 mrt100_app.dll 的较小重构运行时中。

- NGEN 映像往往非常脆弱。 例如，如果修补或更改了依赖项，通常需要使用它的程序集也重新执行 NGEN 操作。 对于 .NET Framework 类库中的系统程序集尤其如此。 相反，.NET 本机允许独立提供应用程序。

## <a name="see-also"></a>另请参阅

- [元数据和自描述组件](../../standard/metadata-and-self-describing-components.md)
- [内部 .NET Native（第 9 频道视频）](https://channel9.msdn.com/Shows/Going+Deep/Inside-NET-Native)
- [反射和 .NET Native](reflection-and-net-native.md)
- [.NET Native 一般疑难解答](net-native-general-troubleshooting.md)
