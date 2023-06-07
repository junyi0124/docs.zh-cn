---
title: 卷影复制程序集
description: 探索 .NET 中程序集的卷影复制，以便无需卸载应用程序域即可更新在此应用程序域中使用的程序集。
ms.date: 03/30/2017
helpviewer_keywords:
- assemblies [.NET Framework], shadow copying
- application domains, shadow copying assemblies
- shadow copying assemblies
ms.assetid: de8b8759-fca7-4260-896b-5a4973157672
ms.openlocfilehash: a7ff72763dd26dbc50cd37e070c2d25ababa00f3
ms.sourcegitcommit: 1c37a894c923bea021a3cc38ce7cba946357bbe1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/19/2020
ms.locfileid: "85104556"
---
# <a name="shadow-copying-assemblies"></a>卷影复制程序集

借助卷影复制，无需卸载应用程序域就可更新用于此应用程序域的程序集。 这对必须连续可用的应用程序（如 ASP.NET 网站）特别有用。

> [!IMPORTANT]
> 卷影复制在 Windows 8.x Store 应用中不受支持。

公共语言运行时会在加载程序集时锁定程序集文件，因此只有卸载此程序集才能更新此文件。 从应用程序域中卸载程序集的唯一方法是卸载应用程序域，因此在正常情况下，只有卸载了正在使用程序集的所有应用程序域才能在磁盘中更新此程序集。

应用程序域配置为卷影复制文件时，应用程序路径中的程序集复制到了另一个位置，并从此位置加载。 此副本处于锁定状态，但原始程序集文件已解锁并可进行更新。

> [!IMPORTANT]
> 只有存储在应用程序目录或其子目录中的程序集才可进行卷影复制，这些程序集在配置应用程序域时由 <xref:System.AppDomainSetup.ApplicationBase%2A> 和 <xref:System.AppDomainSetup.PrivateBinPath%2A> 指定。 存储在全局程序集缓存中的程序集不可进行卷影复制。

本文包含以下各节：

- [启用和使用卷影复制](#EnablingAndUsing)描述了卷影复制的基本用法和可用选项。

- [启动性能](#StartupPerformance)描述了为提高启动性能对 .NET Framework 4 中的卷影复制进行的更改，以及还原到早期版本的行为的方法。

- [已过时的方法](#ObsoleteMethods)描述了对控制 .NET Framework 2.0 中卷影复制的属性和方法进行的更改。

<a name="EnablingAndUsing"></a>

## <a name="enabling-and-using-shadow-copying"></a>启用和使用卷影复制

若要配置卷影复制的应用程序域，可使用 <xref:System.AppDomainSetup> 类的属性，如下所示：

- 通过将 <xref:System.AppDomainSetup.ShadowCopyFiles%2A> 属性设置为字符串值 `"true"` 来启用卷影复制。

  默认情况下，此设置会导致在加载程序集之前，将应用程序路径中的所有程序集复制到下载缓存。 此缓存即是公共语言运行时为了存储从其他计算机中下载的文件而维护的缓存，并且公共语言运行时自动删除不再需要的文件。

- 或者，可通过使用 <xref:System.AppDomainSetup.CachePath%2A> 属性和 <xref:System.AppDomainSetup.ApplicationName%2A> 属性设置卷影复制文件的自定义位置。

  可通过将 <xref:System.AppDomainSetup.ApplicationName%2A> 属性和 <xref:System.AppDomainSetup.CachePath%2A> 属性串联为子目录，形成位置的基路径。 将程序集卷影复制到此路径的子目录，而不是复制到基路径本身。

  > [!NOTE]
  > 如果未设置 <xref:System.AppDomainSetup.ApplicationName%2A> 属性，则忽略 <xref:System.AppDomainSetup.CachePath%2A> 属性并使用下载缓存。 不引发异常。

  如果指定自定义位置，则应负责清理不再需要的目录和复制的文件。 它们不会自动删除。

  为卷影复制文件设置自定义位置还存在以下几个原因。 如果应用程序生成了大量副本，则可能希望为卷影复制文件设置自定义位置。 由于下载缓存受到大小（而非生存期）的限制，所以公共语言运行时可能将尝试删除仍在使用的文件。 设置自定义位置的另一个原因是运行应用程序的用户对公共语言运行时用于下载缓存的目录位置不具备写入权限。

- 或者，使用 <xref:System.AppDomainSetup.ShadowCopyDirectories%2A> 属性限制卷影复制的程序集。

  启用应用程序域的卷影复制时，默认复制应用程序路径（即 <xref:System.AppDomainSetup.ApplicationBase%2A> 属性和 <xref:System.AppDomainSetup.PrivateBinPath%2A> 属性指定的目录）中的所有程序集。 可以通过创建仅包含想要卷影复制的目录的字符串并将此字符串分配至 <xref:System.AppDomainSetup.ShadowCopyDirectories%2A> 属性来限制复制到选定目录。 目录用分号分隔。 只有选定目录中的程序集才进行卷影复制。

  > [!NOTE]
  > 如果未将字符串分配至 <xref:System.AppDomainSetup.ShadowCopyDirectories%2A> 属性或者将此属性设置为 `null`，则 <xref:System.AppDomainSetup.ApplicationBase%2A> 和 <xref:System.AppDomainSetup.PrivateBinPath%2A> 属性指定的目录中的所有程序集均进行卷影复制。

  > [!IMPORTANT]
  > 目录路径不能包含分号，因为分号是分隔符。 分号没有转义符。

<a name="StartupPerformance"></a>

## <a name="startup-performance"></a>启动性能

如果启动使用卷影复制的应用程序域，在将应用程序目录中的程序集复制到卷影复制目录或验证程序集是否已经位于此位置时，会出现延迟。 在 .NET Framework 4 之前，所有程序集均复制到临时目录。 已打开每个程序集以验证程序集名称，并验证了强名称。 已检查每个程序集以查看其更新到的版本是否比卷影复制目录中的副本新。 如果是，则将它复制到卷影复制目录。 最后，删除临时副本。

自 .NET Framework 4 以来，默认启动行为就是直接将应用程序目录中每个程序集的文件日期和时间与影子副本目录中副本的文件日期和时间进行比较。 如果程序集已更新，则使用 .NET Framework 早期版本中的相同过程进行复制；否则，将加载卷影复制目录中的副本。

对于其中程序集未频繁更改且通常程序集的较小子集发生更改的应用程序，产生的性能提升最大。 如果应用程序中的大部分程序集频繁发生更改，则新的默认行为可能导致性能退化。 可以通过向配置文件添加 [\<shadowCopyVerifyByTimestamp> 元素](../configure-apps/file-schema/runtime/shadowcopyverifybytimestamp-element.md)来还原早期版本的 .NET Framework 的启动行为，其中 `enabled="false"`。

<a name="ObsoleteMethods"></a>

## <a name="obsolete-methods"></a>已过时的方法

<xref:System.AppDomain> 类具有几种可用来控制应用程序域中卷影复制的方法（如 <xref:System.AppDomain.SetShadowCopyFiles%2A> 和 <xref:System.AppDomain.ClearShadowCopyPath%2A>），但这些方法在 .NET Framework 2.0 版本中已标记为过时。 若要配置进行卷影复制的应用程序域，推荐使用 <xref:System.AppDomainSetup> 类的属性。

## <a name="see-also"></a>请参阅

- <xref:System.AppDomainSetup.ShadowCopyFiles%2A?displayProperty=nameWithType>
- <xref:System.AppDomainSetup.CachePath%2A?displayProperty=nameWithType>
- <xref:System.AppDomainSetup.ApplicationName%2A?displayProperty=nameWithType>
- <xref:System.AppDomainSetup.ShadowCopyDirectories%2A?displayProperty=nameWithType>
- [\<shadowCopyVerifyByTimestamp> 元素](../configure-apps/file-schema/runtime/shadowcopyverifybytimestamp-element.md)
