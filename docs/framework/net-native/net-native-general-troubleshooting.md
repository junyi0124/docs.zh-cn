---
title: .NET Native 一般疑难解答
ms.date: 03/30/2017
ms.assetid: ee8c5e17-35ea-48a1-8767-83298caac1e8
ms.openlocfilehash: 2bea81e380fed6c456898e9883658ef874c8dd97
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/06/2020
ms.locfileid: "73128230"
---
# <a name="net-native-general-troubleshooting"></a>.NET Native 一般疑难解答

本主题介绍如何解决在 .NET Native 开发应用程序时可能遇到的潜在问题。

- 问题：生成输出窗口未正确更新****。

  解决方法：该生成输出窗口在生成完成之前不会更新****。 生成时间可能会长达数分钟，因此在可能看到更新之前可能会有一个延误。

- 问题：应用的 ARM 零售生成时间已经后推****。

  **解决方法：** 将应用部署到 ARM 设备时，将调用 .NET Native 基础结构。 该编译执行了大量的优化，同时还确保反射等非静态语义继续工作。 此外，.NET 框架中应用使用的那部分得到动态链接，以达到最佳性能，并且还必须编译到本机代码之中。 这就是编译花费时间较长的原因。

  然而，对于标准开发机器上的大多数应用而言，标准编译的编译时间仍在一分钟以内。  通常，在标准开发机器上仅为 .NET Framework 生成本机映像就要花上数分钟。  即使所有的优化可改善生成的代码并且包含 .NET 框架，应用版本时间通常要花费一两分钟。

  我们将继续工作，通过调查多线程编译和其他优化方法来提升编译性能。

- **问题：** 您不知道您的应用程序是使用 .NET Native 编译的。

  **解决方法：** 如果调用 .NET Native 编译器，你会注意到生成时间较长，并且任务管理器将显示各种 .NET Native 组件进程，如 ILC 和 nutc_driver。

  成功生成具有 .NET Native 的项目后，可以在 "obj \\ *config* \  *arch* \\ *projectname*ilc\out 目录" 最终的本机包内容可在 bin " \\ *arch* \\ *配置*\AppX." 下找到。 如果已部署应用，则最终的本机包内容低于 \bin 的 \\ *arch* \\ *配置*\AppX。

- 问题：.NET Native 编译的应用正在引发运行时异常（通常为 [MissingMetadataException](missingmetadataexception-class-net-native.md) 或 [MissingRuntimeArtifactException](missingruntimeartifactexception-class-net-native.md) 异常），而应用未使用 .NET Native 进行编译时则不会引发该异常****。

  解决方法：引发异常的原因是 .NET Native 未提供元数据或通过反射可用的实现代码****。 （有关详细信息，请参阅[.NET Native 和编译](net-native-and-compilation.md)。）若要消除此异常，必须将条目添加到[运行时指令（研发）文件](runtime-directives-rd-xml-configuration-file-reference.md)，以便 .NET Native 工具链可以使元数据或实现代码在运行时可用。 有两个可用的故障排除程序，将生成要添加到运行时指令文件的必需条目：

  - 类型的 [MissingMetadataException 故障排除程序](https://dotnet.github.io/native/troubleshooter/type.html) 。

  - 方法的 [MissingMetadataException 故障排除程序](https://dotnet.github.io/native/troubleshooter/method.html) 。

  有关详细信息，请参阅[反射和 .NET Native](reflection-and-net-native.md)。

## <a name="see-also"></a>另请参阅

- [将 Windows 应用商店应用迁移到 .NET Native](migrating-your-windows-store-app-to-net-native.md)
