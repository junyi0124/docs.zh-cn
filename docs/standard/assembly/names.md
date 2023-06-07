---
title: 程序集名称
description: 了解 .NET 程序集名称及其对程序集范围的影响并将其用于应用程序，并了解 FullName 属性。
ms.date: 08/19/2019
helpviewer_keywords:
- names [.NET], assemblies
- assemblies [.NET], names
ms.assetid: 8f8c2c90-f15d-400e-87e7-a757e4f04d0e
ms.openlocfilehash: 9aa94b4ee54c0a663c9f38392d37369af9f27e48
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95731444"
---
# <a name="assembly-names"></a>程序集名称

程序集的名称存储在元数据中，它对程序集的范围及应用程序对程序集的使用有重要影响。 强名称程序集有一个完全限定的名称，由程序集的名称、区域性、公钥、版本号以及（可选）处理器体系结构组成。 使用 <xref:System.Reflection.Assembly.FullName%2A> 属性来获取已加载程序集的完全限定名称，该名称通常称为显示名称。

运行时使用此名称信息来定位程序集并将其同其他同名的程序集区分开。 例如，名为 `myTypes` 的强名称程序集可以具有下列完全限定名：

```
myTypes, Version=1.0.1234.0, Culture=en-US, PublicKeyToken=b77a5c561934e089c, ProcessorArchitecture=msil
```

在此例中，完全限定名称表明 `myTypes` 程序集的强名称具有公钥标记、区域性值为美国英语、版本号为 1.0.1234.0。 它的处理器体系结构为 `msil`，表示程序集将以实时 (JIT) 方式编译为 32 位代码或 64 位代码（具体取决于操作系统和处理器）。

> [!TIP]
> `ProcessorArchitecture` 信息允许特定于处理器的程序集版本。 可以创建某程序集的多个版本，其标识的差异仅在于处理器体系结构不同，例如特定于 32 位和 64 位处理器的版本。 处理器体系结构对于强名称不是必需的。 有关详细信息，请参阅 <xref:System.Reflection.AssemblyName.ProcessorArchitecture%2A?displayProperty=nameWithType>。

 请求程序集中的类型的代码必须使用完全限定的程序集名称。 这称为完全限定绑定。 在 .NET Framework 中引用程序集时不允许使用部分绑定，因为它只指定一个程序集名称。

 对组成 .NET Framework 的程序集的所有程序集引用也必须包含程序集的完全限定名称。 例如，若要引用 1.0 版的 System.Data .NET Framework 程序集，需要包括：

```
System.data, version=1.0.3300.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
```

该版本与随 .NET Framework 1.0 版一起提供的所有 .NET Framework 程序集的版本号相对应。 对于 .NET Framework 程序集，区域性值始终不是特定的，公钥与上例中所示公钥相同。

 例如，若要在配置文件中添加程序集引用以设置跟踪侦听器，需要包括系统 .NET Framework 程序集的完全限定名：

```xml
<add name="myListener" type="System.Diagnostics.TextWriterTraceListener, System, Version=1.0.3300.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" initializeData="c:\myListener.log" />
```

> [!NOTE]
> 绑定到程序集时，运行时不区分程序集名称的大小写，但会保留程序集名称中使用的大小写。 Windows SDK 中的几个工具会区分程序集名称的大小写。 为获得最佳效果，管理程序集名称时请按区分大小写的方式来处理。

## <a name="name-application-components"></a>命名应用程序组件

 运行时在确定程序集的标识时不考虑文件名。 程序集标识（由程序集名称、版本、区域性和强名称组成）对运行时必须清楚明了。

 例如，如果一个名为 myAssembly.exe 的程序集引用一个名为 myAssembly.dll 的程序集，则在执行 myAssembly.exe 时会正确进行绑定  。 但是，如果另一个应用程序使用 <xref:System.AppDomain.ExecuteAssembly%2A?displayProperty=nameWithType> 方法执行 myAssembly.exe，则当 myAssembly.exe 请求绑定到 `myAssembly` 时，运行时会确定 `myAssembly` 已经加载 。 在这种情况下，不会加载 myAssembly.dll。 由于 myAssembly.exe 不包含请求的类型，因此会发生 <xref:System.TypeLoadException>。

 为避免这个问题，请确保组成应用程序的程序集具有不同的程序集名称，或者将名称相同的程序集放在不同的目录中。

> [!NOTE]
> 在 .NET Framework 中，如果将强名称程序集置于全局程序集缓存中，则程序集的文件名必须与程序集名称相匹配，不包括文件扩展名，如 .exe 或 .dll 。 例如，如果程序集的文件名为 myAssembly.dll，则程序集名称必须为 `myAssembly`。 只有在根应用程序目录中部署的专用程序集的程序集名称可以不同于文件名。

## <a name="see-also"></a>请参阅

- [如何：确定程序集的完全限定名称](find-fully-qualified-name.md)
- [创建程序集](create.md)
- [具有强名称的程序集](strong-named.md)
- [全局程序集缓存](../../framework/app-domains/gac.md)
- [运行时如何定位程序集](../../framework/deployment/how-the-runtime-locates-assemblies.md)
