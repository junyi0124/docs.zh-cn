---
title: .NET Native 入门
ms.date: 03/30/2017
ms.assetid: fc9e04e8-2d05-4870-8cd6-5bd276814afc
ms.openlocfilehash: b6cd4acaa377de7fc172fb12c9fb9ff1b832f88a
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90551205"
---
# <a name="getting-started-with-net-native"></a>.NET Native 入门

无论是在编写适用于 Windows 10 的新 Windows 应用，还是在迁移现有的 Windows 应用商店应用，都可以按照同一套过程操作。 若要创建 .NET Native 应用，请执行以下步骤：

1. [开发面向 Windows 10 的通用 Windows 平台 (UWP) 应用](#Step1)，测试应用的调试版本以确保其正常工作。

2. [处理额外的反射和序列化用法](#Step2)。

3. [部署和测试应用的发布版本](#Step3)。

4. [手动解决丢失的元数据](#Step4)，并重复[步骤 3](#Step3)，直到所有问题都得到解决为止。

> [!NOTE]
> 如果要将现有的 Windows 应用商店应用迁移到 .NET Native，请务必查看将 [windows 应用商店应用迁移到 .NET Native](migrating-your-windows-store-app-to-net-native.md)。

<a name="Step1"></a>

## <a name="step-1-develop-and-test-debug-builds-of-your-uwp-app"></a>步骤 1：开发和测试 UWP 应用的调试版本

不管是开发新的应用还是迁移现有应用，均遵循用于任何 Windows 应用的同一流程进行操作。

1. 通过使用针对 Visual C# 或 Visual Basic 的通用 Windows 应用模板，在 Visual Studio 中创建新的 UWP 项目。 默认情况下，所有 UWP 应用程序均以 CoreCLR 为目标，且其发布版本通过使用 .NET Native 工具链进行编译。

2. 请注意，使用 .NET Native 工具链和不使用它来编译 UWP 应用项目之间存在一些已知的兼容性问题。 有关更多信息，请参阅 [迁移指南](migrating-your-windows-store-app-to-net-native.md) 。

你现在可以针对在本地系统 (或模拟器) 中运行的 .NET Native 外围应用程序，编写 c # 或 Visual Basic 代码。

> [!IMPORTANT]
> 开发你的应用时，在代码中用任何序列化或反射时都应提起注意。

默认情况下，调试版本进行 JIT 编译以启用快速 F5 部署，而发布版本则使用 .NET Native 预编译技术进行编译。 这意味着在使用 .NET Native 工具链来编译应用的调试版本之前，应生成和测试它们以确保其正常工作。

<a name="Step2"></a>

## <a name="step-2-handle-additional-reflection-and-serialization-usage"></a>步骤 2：处理其他反射和序列化用法

创建运行时指令文件 Default.rd.xml 时，会将其自动添加到项目中。 如果在 C# 中进行开发，则该文件位于项目的 **“属性”** 文件夹中。 如果在 Visual Basic 中进行开发，则该文件位于项目的 **“我的项目”** 文件夹中。

> [!NOTE]
> 有关提供为何需要运行时指令文件的背景的 .NET 本机编译过程的概述，请参阅 [.NET 本机和编译](net-native-and-compilation.md)。

运行时指令文件用于定义应用在运行时所需的元数据。 某些情况下，文件的默认版本可能是够用的。 然而，一些依赖于序列化或反射的代码可能需要运行时指令文件中的其他条目。

**序列化**

有两类序列化程序，并且每一类可能都要求在运行时指令文件中有额外的条目：

- 非基于反射的序列化程序。 在 .NET Framework 类库中找到的序列化程序，比如 <xref:System.Runtime.Serialization.DataContractSerializer>\ <xref:System.Runtime.Serialization.Json.DataContractJsonSerializer>和 <xref:System.Xml.Serialization.XmlSerializer> 类，不依赖反射。 然而，它们需要在基于对象的基础生成的代码得到序列化或反序列化。  有关更多信息，请参阅 [Serialization and Metadata](serialization-and-metadata.md)中的“Microsoft 序列化程序”部分。

- 第三方序列化程序。 第三方序列化库（最常见的是 Newtonsoft.json JSON 序列化程序）通常基于反射，并要求.rd.xml 文件中的条目 \* 支持对象序列化和反序列化。 有关更多信息，请参阅 [Serialization and Metadata](serialization-and-metadata.md)中的“第三方序列化程序”部分。

**依赖反射的方法**

在某些情况下，代码中对反射的使用是不明显的。 一些常见的 API 或编程模式不被视为是反射 API 的一部分，但依赖反射成功执行。 这包括以下类型实例化和方法构造方法：

- <xref:System.Type.MakeGenericType%2A?displayProperty=nameWithType> 方法

- <xref:System.Array.CreateInstance%2A?displayProperty=nameWithType> 和 <xref:System.Type.MakeArrayType%2A?displayProperty=nameWithType> 方法

- <xref:System.Reflection.MethodInfo.MakeGenericMethod%2A?displayProperty=nameWithType> 方法。

有关详细信息，请参阅 [APIs That Rely on Reflection](apis-that-rely-on-reflection.md)。

> [!NOTE]
> 运行时指令文件中使用的类型名称必须是完全限定的。 例如，该文件必须指定“System.String”而不是“String”。

<a name="Step3"></a>

## <a name="step-3-deploy-and-test-the-release-builds-of-your-app"></a>步骤 3：部署和测试应用的发布版本

在更新运行时指令文件之后，你可以重新生成和部署应用的发布版本。 .NET Native 二进制文件放置在项目 "**属性**" 对话框的 "**生成输出路径**"**文本框中指定**目录的 ILC 子目录中。未在此文件夹中的二进制文件尚未用 .NET Native 编译。 彻底测试你的应用，并在其各个目标平台上测试所有方案（包括失败方案）。

如果应用无法正常工作（尤其是在运行时引发 [MissingMetadataException](missingmetadataexception-class-net-native.md) 或 [MissingInteropDataException](missinginteropdataexception-class-net-native.md) 异常的情况下），请按照下一部分的指令进行操作，即[步骤 4：手动解决丢失的元数据](#Step4)。 启用最可能的异常可能会帮助发现这些 bug。

如果已测试并调试了应用的调试版本，并且确信已消除 [MissingMetadataException](missingmetadataexception-class-net-native.md) 和 [MissingInteropDataException](missinginteropdataexception-class-net-native.md) 异常，则应将应用作为优化 .NET Native 应用进行测试。 为此，请将活动项目配置从“调试”改为“发布”********。

<a name="Step4"></a>

## <a name="step-4-manually-resolve-missing-metadata"></a>步骤 4：手动解决丢失的元数据

在桌面上不会遇到的 .NET Native 最常见的失败是运行时 [MissingMetadataException](missingmetadataexception-class-net-native.md)、 [MissingInteropDataException](missinginteropdataexception-class-net-native.md)或 [MissingRuntimeArtifactException](missingruntimeartifactexception-class-net-native.md) 异常。 在某些情况下，元数据的丢失在不可预测的行为中或甚至在应用失败的情况下可以自行显露。 此部分讨论了你该如何通过将指令添加到运行时指令文件来调试和解决这些异常。 有关运行时指令的格式信息，请参阅[运行时指令 (rd.xml) 配置文件参考](runtime-directives-rd-xml-configuration-file-reference.md)。 添加运行时指令后，你应该再次 [部署并检测你的应用](#Step3) 并解决任何新的 [丢失元数据异常](missingmetadataexception-class-net-native.md)、 [丢失互操作数据异常](missinginteropdataexception-class-net-native.md)和  [丢失运行时项目异常](missingruntimeartifactexception-class-net-native.md) 异常，直到你停止遇到任何异常。

> [!TIP]
> 在更高级别上指定你的运行时指令，使你的应用能够适应代码变化。  我们推荐在命名空间和类型级别而不在成员级别添加运行时指令。 注意，你可能需要在弹性和编译时间较长的较大二进制代码之间做一个权衡。

当处理一个丢失元数据异常时，请考虑以下问题：

- 应用在发生异常之前正在执行什么操作？

  - 例如，它是正在绑定、序列化或反序列化数据？还是正在直接使用反射 API？

- 这是一个孤立情形吗？或者你是否认为针对其他类型时也会遇到同样的问题？

  - 例如，当在应用的对象模型中序列化一个类型时，引发了 [丢失元数据异常](missingmetadataexception-class-net-native.md) 异常。  如果你知道其他要得到序列化的类型，可以同时为这些类型（或者为它们包含的命名空间，这要视代码的组织情况而定）添加运行指令。

- 你可以重写代码，不让它使用反射吗？

  - 例如，当你知道使用什么类型时代码是否使用了 `dynamic` 关键字？

  - 当有更好的选择时，代码是否调用了一个依赖反射的方法？

> [!NOTE]
> 有关如何处理因反射中的差异以及桌面应用和 .NET Native 中的元数据的可用性而造成的问题的其他信息，请参阅 [依赖于反射的 api](apis-that-rely-on-reflection.md)。

要了解处理在检测应用的过程中发生的异常和其他问题的特定实例，请参阅：

- [示例：处理绑定数据时出现的异常](example-handling-exceptions-when-binding-data.md)

- [示例：动态编程疑难解答](example-troubleshooting-dynamic-programming.md)

- [.NET 本机应用中的运行时异常](runtime-exceptions-in-net-native-apps.md)

## <a name="see-also"></a>请参阅

- [运行时指令 (rd.xml) 配置文件引用](runtime-directives-rd-xml-configuration-file-reference.md)
- [.NET Native 安装和配置](/previous-versions/dn600164(v=vs.110))
- [.NET Native 和编译](net-native-and-compilation.md)
- [反射和 .NET Native](reflection-and-net-native.md)
- [利用反射的 API](apis-that-rely-on-reflection.md)
- [序列化和元数据](serialization-and-metadata.md)
- [将 Windows 应用商店应用迁移到 .NET Native](migrating-your-windows-store-app-to-net-native.md)
