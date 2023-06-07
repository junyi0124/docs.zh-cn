---
title: 程序集版本控制
description: 了解 .NET 程序集的版本控制。 使用 CLR 的程序集的所有版本控制都在程序集级别上进行。
ms.date: 08/20/2019
helpviewer_keywords:
- informational versions
- version numbers, assemblies
- assemblies [.NET], versioning
- resolving assembly binding requests
- versioning, assemblies
ms.assetid: 775ad4fb-914f-453c-98ef-ce1089b6f903
ms.openlocfilehash: c94e0c74b8beed29537b53d7476715e2cacb7b80
ms.sourcegitcommit: 279fb6e8d515df51676528a7424a1df2f0917116
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2020
ms.locfileid: "92687636"
---
# <a name="assembly-versioning"></a>程序集版本控制

使用公共语言运行时的程序集的所有版本控制都在程序集级别上进行。 一个程序集的特定版本和依赖程序集的版本在该程序集的清单中记录下来。 除非被配置文件（应用程序配置文件、发行者策略文件和计算机的管理员配置文件）中的显式版本策略重写，否则运行时的默认版本策略是，应用程序只与它们生成和测试时所用的程序集版本一起运行。  
  
> [!NOTE]
> 仅对具有强名称的程序集进行版本控制。  
  
运行时执行以下几步来解析程序集绑定请求：  
  
1. 检查原程序集引用，以确定该程序集的版本是否被绑定。  
  
2. 检查所有适用的配置文件以应用版本策略。  
  
3. 通过原程序集引用和配置文件中指定的任何重定向来确定正确的程序集，并且确定应绑定到调用程序集的版本。  
  
4. 检查全局程序集缓存和在配置文件中指定的基本代码，然后使用在[运行时如何定位程序集](../../framework/deployment/how-the-runtime-locates-assemblies.md)中解释的探测规则检查该应用程序的目录和子目录。  
  
下图说明了这些步骤：  
  
![显示程序集绑定请求解析中的步骤的图表。](./media/versioning/resolve-assembly-binding-request.gif)
  
有关配置应用程序的更多信息，请参阅[配置应用](../../framework/configure-apps/index.md)。 有关绑定策略的更多信息，请参阅[运行时如何定位程序集](../../framework/deployment/how-the-runtime-locates-assemblies.md)。  
  
## <a name="version-information"></a>版本信息  

每一程序集都用两种截然不同的方法来表示版本信息：  
  
- 程序集的版本号，该版本号与程序集名称及区域性信息都是程序集标识的组成部分。 该号码将由运行时用来强制实施版本策略，它在运行时的类型解析进程中起着重要的作用。  
  
- 信息性版本，这是一个字符串，表示仅为提醒的目的而包括的附加版本信息。  
  
### <a name="assembly-version-number"></a>程序集版本号  

每一程序集都有一个版本号作为其标识的一部分。 因此，如果两个程序集具有不同的版本号，运行时就会将它们视作完全不同的程序集。 此版本号实际表示为具有以下格式的四部分号码：  
  
\<*major version*>.\<*minor version*>.\<*build number*>.\<*revision*>  
  
例如，版本 1.5.1254.0 中的 1 表示主版本，5 表示次版本，1254 表示生成号，而 0 则表示修订号。  
  
版本号与其他标识信息（包括程序集名称和公钥，以及与该应用程序所连接的其他程序集的关系和标识有关的信息）一起存储在程序集清单中。  
  
在生成程序集时，开发工具将把每一个被引用程序集的依赖项信息记录在程序集清单中。 运行时将这些版本号与管理员、应用程序或发行者设置的配置信息结合使用，以加载被引用程序集的正确版本。  
  
为进行版本控制，运行时会区分常规程序集和具有强名称的程序集。 只对具有强名称的程序集执行版本检查。  
  
有关指定版本绑定策略的信息，请参阅[配置应用](../../framework/configure-apps/index.md)。 有关运行时如何使用版本信息查找特定程序集的信息，请参阅[运行时如何定位程序集](../../framework/deployment/how-the-runtime-locates-assemblies.md)。  
  
### <a name="assembly-informational-version"></a>程序集信息性版本  

信息性版本是一个字符串，它仅出于提醒的目的将附加的版本信息附加到一个程序集；此信息不在运行时使用。 基于文本的信息性版本相当于产品的营销广告、包装或产品名称，不被运行时使用。 例如，信息性版本可以是“公共语言运行时 1.0 版”或“NET Control SP 2”。 在 Microsoft Windows 中的文件属性对话框的“版本”选项卡上，此信息显示在“产品版本”项中。  
  
> [!NOTE]
> 虽然可以指定任意文本，但是如果字符串的格式不是程序集版本号使用的格式，或者虽然是这种格式但包含通配符，则在编译时会显示一条警告消息。 此警告无碍。  
  
信息性版本用自定义特性 <xref:System.Reflection.AssemblyInformationalVersionAttribute?displayProperty=nameWithType> 表示。 有关信息性版本属性的更多信息，请参阅[设置程序集属性](set-attributes.md)。  
  
## <a name="see-also"></a>请参阅

- [运行时如何定位程序集](../../framework/deployment/how-the-runtime-locates-assemblies.md)
- [配置应用](../../framework/configure-apps/index.md)
- [设置程序集特性](set-attributes.md)
- [.NET 中的程序集](index.md)
