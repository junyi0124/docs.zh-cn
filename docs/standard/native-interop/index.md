---
title: 本机互操作性 - .NET
description: 了解如何与 .NET 中的本机组件交互。
ms.date: 01/18/2019
ms.openlocfilehash: 330466d74cc268214f74c4f575e6a2961f678972
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2020
ms.locfileid: "75706330"
---
# <a name="native-interoperability"></a>本机互操作性

以下文章介绍在 .NET 中实现“本机互操作性”的各种方法。

调用本机代码的原因有以下几种：

- 操作系统附带的许多 API 并不在托管类库中。 该应用场景最好的例子就是对硬件或操作系统管理功能的访问。
- 与其他具有或可生成 C 式 ABI（本机 ABI）的组件通信，如通过 [Java 本机接口 (JNI)](https://docs.oracle.com/javase/8/docs/technotes/guides/jni/) 公开的 Java 代码或可生成本机组件的任何其他托管语言。
- 在 Windows 上，安装的大部分软件（例如 Microsoft Office 套件）会注册 COM 组件，用于代表软件的程序，并使开发人员能够自动化或者使用它们。 在这种情况下，也需要本机互操作性。

前述列表并未包括开发人员想要/偏向于/需要与本机组件交互的所有可能场合与情境。 例如，.NET 类库使用本机互操作性支持来实现其相当多的 API，如控制台支持和操作、文件系统访问，等等。 但务必应知道，在需要的情况下是有其他选择的。

> [!NOTE]
> 本部分中的大多示例是针对 .NET Core 支持的所有三个平台（Windows、Linux 和 macOS）提供的。 但是，在某些简短的演示性示例中，只显示了一个使用 Windows 文件名和扩展名（即，库的扩展名“dll”）的例子。 这并不意味着这些功能不能在 Linux 或 macOS 上使用，只是出于方便而未提到这些平台。

## <a name="see-also"></a>请参阅

- [平台调用 (P/Invoke)](pinvoke.md)
- [类型封送](type-marshaling.md)
- [本机互操作性最佳做法](best-practices.md)
