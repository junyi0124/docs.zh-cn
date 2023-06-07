---
title: 适用于程序集加载的最佳做法
description: 了解 .NET 中适用于程序集加载的最佳做法。 避免类型标识问题，这些问题可能导致转换无效、缺少方法和其他异常。
ms.date: 03/30/2017
helpviewer_keywords:
- assemblies,binding
- LoadFrom method
- default load context
- TypeLoadException class,causes
- assembly loading errors
- load contexts
- assemblies,loading
- LoadWithPartialName method
- load-from context
ms.assetid: 68d1c539-6a47-4614-ab59-4b071c9d4b4c
ms.openlocfilehash: 9e09c9e43a4bd8b13712d5fbbf85830394ac0b58
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96236437"
---
# <a name="best-practices-for-assembly-loading"></a>适用于程序集加载的最佳做法

本文讨论避免类型标识问题的方法，从而避免发生 <xref:System.InvalidCastException>、<xref:System.MissingMethodException> 以及其他错误。 本文讨论以下建议：  
  
- [了解加载上下文的优点和缺点](#load_contexts)  
  
- [避免对部分程序集名称进行绑定](#avoid_partial_names)  
  
- [避免将一个程序集加载到多个上下文中](#avoid_loading_into_multiple_contexts)  
  
- [避免将一个程序集的多个版本加载到同一上下文中](#avoid_loading_multiple_versions)  
  
- [考虑切换到默认加载上下文](#switch_to_default)  
  
 第一个建议（即[了解加载上下文的优点和缺点](#load_contexts)）为其他建议提供了背景信息，因为这些建议都依赖于对加载上下文的了解。  
  
<a name="load_contexts"></a>

## <a name="understand-the-advantages-and-disadvantages-of-load-contexts"></a>了解加载上下文的优点和缺点  

 在应用程序域中，可以将程序集加载到以下三个上下文之一中，也可以在没有上下文的情况下加载它们：  
  
- 默认加载上下文包含通过探测全局程序集缓存找到的程序集、（如果托管了运行时）主机程序集存储区（例如在 SQL Server 中）以及应用程序域的 <xref:System.AppDomainSetup.ApplicationBase%2A> 和 <xref:System.AppDomainSetup.PrivateBinPath%2A>。 <xref:System.Reflection.Assembly.Load%2A> 方法的大多数重载都将程序集加载到此上下文中。  
  
- 加载位置上下文包含从加载程序未搜索的位置加载的程序集。 例如，外接程序可能安装在一个不在应用程序路径下的目录中。 <xref:System.Reflection.Assembly.LoadFrom%2A?displayProperty=nameWithType>、<xref:System.AppDomain.CreateInstanceFrom%2A?displayProperty=nameWithType> 和 <xref:System.AppDomain.ExecuteAssembly%2A?displayProperty=nameWithType> 都是通过路径加载的方法的示例。  
  
- 仅反射上下文包含使用 <xref:System.Reflection.Assembly.ReflectionOnlyLoad%2A> 和 <xref:System.Reflection.Assembly.ReflectionOnlyLoadFrom%2A> 方法加载的程序集。 由于无法执行此上下文中的代码，因此在这里不对其进行讨论。 有关详细信息，请参阅[如何：将程序集加载到仅反射上下文中](../reflection-and-codedom/how-to-load-assemblies-into-the-reflection-only-context.md)。  
  
- 如果使用反射发出生成了一个瞬态动态程序集，则该程序集不在任何上下文中。 此外，使用 <xref:System.Reflection.Assembly.LoadFile%2A> 方法加载的大多数程序集都是在没有上下文的情况下加载的，并且从字节数组加载的程序集也是在没有上下文的情况下加载的，除非这些程序集的标识（在应用策略后）证实它们位于全局程序集缓存中。  
  
 以下各节将讨论执行上下文的优点和缺点。  
  
### <a name="default-load-context"></a>默认加载上下文  

 将程序集加载到默认加载上下文中时，会自动加载其依赖项。 将自动为默认加载上下文或加载位置上下文中的程序集查找加载到默认加载上下文中的依赖项。 按程序集标识进行加载可确保不使用未知版本的程序集，从而提高应用程序的稳定性（请参阅[避免对部分程序集名称进行绑定](#avoid_partial_names)一节）。  
  
 使用默认加载上下文具有以下缺点：  
  
- 加载到其他上下文中的依赖项将不可用。  
  
- 不能将位于探测路径外部位置的程序集加载到默认加载上下文中。  
  
### <a name="load-from-context"></a>加载位置上下文  

 利用加载位置上下文，可从不在应用程序路径下（因此不包含在探测路径中）的某个路径加载程序集。 加载位置上下文允许从该路径查找和加载依赖项，因为路径信息由上下文维护。 另外，此上下文中的程序集可以使用加载到默认加载上下文中的依赖项。  
  
 使用 <xref:System.Reflection.Assembly.LoadFrom%2A?displayProperty=nameWithType> 方法或其他按路径加载的方法之一加载程序集具有以下缺点：  
  
- 如果具有相同标识的程序集已加载到加载源上下文中，则即使指定了不同的路径，<xref:System.Reflection.Assembly.LoadFrom%2A> 也仍会返回已加载的程序集。  
  
- 如果用 <xref:System.Reflection.Assembly.LoadFrom%2A> 加载一个程序集，随后默认加载上下文中的一个程序集尝试按显示名称加载同一程序集，则加载尝试将失败。 对程序集进行反序列化时，可能发生这种情况。  
  
- 如果用 <xref:System.Reflection.Assembly.LoadFrom%2A> 加载一个程序集，并且探测路径包括一个具有相同标识但位置不同的程序集，则将发生 <xref:System.InvalidCastException>、<xref:System.MissingMethodException> 或其他意外行为。  
  
- <xref:System.Reflection.Assembly.LoadFrom%2A> 需要对指定路径的 <xref:System.Security.Permissions.FileIOPermissionAccess.Read?displayProperty=nameWithType> 和 <xref:System.Security.Permissions.FileIOPermissionAccess.PathDiscovery?displayProperty=nameWithType> 或 <xref:System.Net.WebPermission>。  
  
- 如果存在程序集的本机映像，将不会使用它。  
  
- 程序集不能以非特定于域的方式加载。  
  
- 该策略不适用于 1.0 和 1.1 版本的 .NET Framework。  
  
### <a name="no-context"></a>无上下文  

 使用反射发出生成的瞬态程序集只能选择在没有下文的情况下进行加载。 在没有上下文的情况下进行加载是将具有同一标识的多个程序集加载到一个应用程序域中的唯一方式。 这将省去探测成本。  
  
 从字节数组加载的程序集都是在没有上下文的情况下加载的，除非程序集的标识（在应用策略后建立）与全局程序集缓存中的程序集标识匹配；在此情况下，将会从全局程序集缓存加载程序集。  
  
 在没有上下文的情况下加载程序集具有以下缺点：  
  
- 无法将其他程序集绑定到在没有上下文的情况下加载的程序集，除非处理 <xref:System.AppDomain.AssemblyResolve?displayProperty=nameWithType> 事件。  
  
- 依赖项无法自动加载。 可以在没有上下文的情况下预加载依赖项、将依赖项预加载到默认加载上下文中或通过处理 <xref:System.AppDomain.AssemblyResolve?displayProperty=nameWithType> 事件来加载依赖项。  
  
- 在没有上下文的情况下加载具有同一标识的多个程序集会导致出现类型标识问题，这些问题与将具有同一标识的多个程序集加载到多个上下文中所导致的问题类似。 请参阅[避免将一个程序集加载到多个上下文中](#avoid_loading_into_multiple_contexts)。  
  
- 如果存在程序集的本机映像，将不会使用它。  
  
- 程序集不能以非特定于域的方式加载。  
  
- 该策略不适用于 1.0 和 1.1 版本的 .NET Framework。  
  
<a name="avoid_partial_names"></a>

## <a name="avoid-binding-on-partial-assembly-names"></a>避免对部分程序集名称进行绑定  

 加载程序集时，如果仅指定程序集显示名称的一部分 (<xref:System.Reflection.Assembly.FullName%2A>)，则会发生部分名称绑定。 例如，可能会在调用 <xref:System.Reflection.Assembly.Load%2A?displayProperty=nameWithType> 方法时仅使用程序集的简单名称，而忽略版本、区域性和公钥标记。 也可能会调用 <xref:System.Reflection.Assembly.LoadWithPartialName%2A?displayProperty=nameWithType> 方法，该方法首先调用 <xref:System.Reflection.Assembly.Load%2A?displayProperty=nameWithType> 方法，再搜索全局程序集缓存（如果未能找到程序集）并加载程序集的最新可用版本。  
  
 部分名称绑定会导致出现许多问题，其中包括：  
  
- <xref:System.Reflection.Assembly.LoadWithPartialName%2A?displayProperty=nameWithType> 方法可能会加载简单名称相同的不同程序集。 例如，两个应用程序可能会将具有简单名称 `GraphicsLibrary` 的两个完全不同的程序集安装到全局程序集缓存中。  
  
- 实际加载的程序集可能无法后向兼容。 例如，若不指定版本，加载的版本可能会比最初编写程序时指定使用的版本新很多。 较新版本中的更改可能会导致应用程序中出现错误。  
  
- 实际加载的程序集可能无法前向兼容。 例如，可以使用程序集的最新版本来生成并测试应用程序，但部分绑定可能会加载一个缺少应用程序所用功能的早期版本。  
  
- 安装新的应用程序可能会损坏现有应用程序。 安装共享程序集的更新的非兼容版本会损坏使用 <xref:System.Reflection.Assembly.LoadWithPartialName%2A> 方法的应用程序。  
  
- 可能发生意外的依赖项加载。 在加载共享一个依赖项的两个程序集时，如果利用部分绑定来加载它们，则可能会导致其中一个程序集使用未用来生成或测试该程序集的组件。  
  
 由于部分名称绑定会导致出现上述问题，因此已将 <xref:System.Reflection.Assembly.LoadWithPartialName%2A> 方法标记为已过时。 建议改用 <xref:System.Reflection.Assembly.Load%2A?displayProperty=nameWithType> 方法，并指定完整的程序集显示名称。 请参阅[了解加载上下文的优点和缺点](#load_contexts)和[考虑切换到默认加载上下文](#switch_to_default)。  
  
 如果希望使用 <xref:System.Reflection.Assembly.LoadWithPartialName%2A> 方法（因为此方法使程序集加载变得很轻松），请考虑在应用程序失败时提供用于标识缺失的程序集的错误消息，与自动使用程序集的未知版本（可能会导致不可预知的行为和安全漏洞）相比，这样做可能能够提供更好的用户体验。  
  
<a name="avoid_loading_into_multiple_contexts"></a>

## <a name="avoid-loading-an-assembly-into-multiple-contexts"></a>避免将一个程序集加载到多个上下文中  

 将一个程序集加载到多个上下文中会导致出现类型标识问题。 将同一个程序集中的相同类型加载到两个不同的上下文中，就像是加载具有相同名称的两个不同的类型一样。 如果尝试将一个类型强制转换为另一个类型，则将引发 <xref:System.InvalidCastException>，并显示一条令人混淆的消息，指示不能将类型 `MyType` 强制转换为类型 `MyType`。  
  
 例如，假设在一个名为 `Utility` 的程序集中声明 `ICommunicate` 接口，该接口由程序及其加载的其他程序集引用。 这些其他程序集包含实现 `ICommunicate` 接口的类型，并允许程序使用它们。  
  
 下面来看看在程序运行时会出现的情况。 程序所引用的程序集将加载到默认加载上下文中。 如果使用 <xref:System.Reflection.Assembly.Load%2A> 方法按照目标程序集的标识来加载该程序集，则该程序集及其依赖项都将位于默认加载上下文中。 程序和目标程序集将使用同一个 `Utility` 程序集。  
  
 不过，假设使用 <xref:System.Reflection.Assembly.LoadFile%2A> 方法按照目标程序集的文件路径加载该程序集。 该程序集将在没有任何上下文的情况下进行加载，因此不会自动加载其依赖项。 可能具有 <xref:System.AppDomain.AssemblyResolve?displayProperty=nameWithType> 事件的处理程序来提供依赖项，并且该处理程序可能会使用 <xref:System.Reflection.Assembly.LoadFile%2A> 方法在没有上下文的情况下加载 `Utility` 程序集。 此时，若创建目标程序集中包含的某个类型的实例，并尝试将该实例分配给类型 `ICommunicate` 的变量，则将引发 <xref:System.InvalidCastException>，因为运行时会将 `Utility` 程序集的两个副本中的 `ICommunicate` 接口视为不同的类型。  
  
 在许多其他情况下，也可以将一个程序集加载到多个上下文中。 最佳方法是通过在应用程序路径中重新定位目标程序集，并对 <xref:System.Reflection.Assembly.Load%2A> 方法使用完整的显示名称，从而避免冲突。 然后，将目标程序集加载到默认加载上下文中，并且两个程序集将使用同一个 `Utility` 程序集。  
  
 如果目标程序集必须保留在应用程序路径的外部，可以使用 <xref:System.Reflection.Assembly.LoadFrom%2A> 方法将目标程序集加载到加载位置上下文中。 如果编译的目标程序集中存在对应用程序的 `Utility` 程序集的引用，则目标程序集将会使用应用程序已加载到默认加载上下文中的 `Utility` 程序集。 请注意，如果目标程序集依赖应用程序路径外部的 `Utility` 程序集副本，则会出现问题。 如果在应用程序加载 `Utility` 程序集之前已将该程序集加载到加载位置上下文中，则应用程序的加载将失败。  
  
 [考虑切换到默认加载上下文](#switch_to_default)一节讨论了针对使用文件路径加载（例如 <xref:System.Reflection.Assembly.LoadFile%2A> 和 <xref:System.Reflection.Assembly.LoadFrom%2A>）的替代方法。  
  
<a name="avoid_loading_multiple_versions"></a>

## <a name="avoid-loading-multiple-versions-of-an-assembly-into-the-same-context"></a>避免将一个程序集的多个版本加载到同一上下文中  

 将一个程序集的多个版本加载到一个加载上下文中会导致出现类型标识问题。 从同一个程序集的两个版本加载同一个类型，就像是加载了具有相同名称的两个不同类型一样。 如果尝试将一个类型强制转换为另一个类型，则将引发 <xref:System.InvalidCastException>，并显示一条令人混淆的消息，指示不能将类型 `MyType` 强制转换为类型 `MyType`。  
  
 例如，程序可能会直接加载 `Utility` 程序集的一个版本，稍后它可能会加载另一个程序集，而该程序集将加载 `Utility` 程序集的一个不同的版本。 或者，编码错误可能会导致应用程序中两个不同的代码路径加载一个程序集的不同版本。  
  
 在默认加载上下文中，如果使用 <xref:System.Reflection.Assembly.Load%2A?displayProperty=nameWithType> 方法并指定包含不同版本号的完整程序集显示名称，则会出现此问题。 对于在没有上下文的情况下加载的程序集来说，若使用 <xref:System.Reflection.Assembly.LoadFile%2A?displayProperty=nameWithType> 方法从不同的路径加载同一程序集，则会出现此问题。 运行时会将从不同的路径加载的两个程序集视为不同的程序集，即使这两个程序集的标识相同也是如此。  
  
 除了类型标识问题之外，如果将从程序集的一个版本加载的类型传递给需要来自不同版本的类型的代码，则多个程序集版本还会导致 <xref:System.MissingMethodException>。 例如，此代码可能需要已添加到更高版本的方法。  
  
 如果版本之间的类型行为发生更改，则会出现更多的细微错误。 例如，某个方法可能会引发意外的异常或返回意外的值。  
  
 请认真检查代码，确保仅加载程序集的一个版本。 可以使用 <xref:System.AppDomain.GetAssemblies%2A?displayProperty=nameWithType> 方法确定在任何给定时间加载的程序集。  
  
<a name="switch_to_default"></a>

## <a name="consider-switching-to-the-default-load-context"></a>考虑切换到默认加载上下文  

 检查应用程序的程序集加载和部署模式。 是否能够消除从字节数组加载的程序集？ 是否能够将程序集移动到探测路径中？ 如果程序集位于全局程序集缓存中或应用程序域的探测路径（即 <xref:System.AppDomainSetup.ApplicationBase%2A> 和 <xref:System.AppDomainSetup.PrivateBinPath%2A>）中，则可以按照程序集的标识来加载程序集。  
  
 如果无法将所有程序集放入探测路径中，请考虑替代方式，例如使用 .NET Framework 外接程序模型，将程序集放置到全局程序集缓存中或创建应用程序域。  
  
### <a name="consider-using-the-net-framework-add-in-model"></a>考虑使用 .NET Framework 外接程序模型  

 如果使用加载位置上下文来实现外接程序（它们通常未安装在应用程序基中），请使用 .NET Framework 外接程序模型。 此模型提供应用程序域或进程级别的隔离，无需自行管理应用程序域。 有关外接程序模型的信息，请参阅[外接程序和扩展性](/previous-versions/dotnet/netframework-4.0/bb384200(v=vs.100))。  
  
### <a name="consider-using-the-global-assembly-cache"></a>考虑使用全局程序集缓存  

 将程序集置于全局程序集缓存中，不但可以获得位于应用程序基外部的共享程序集路径的好处，而且不会丧失默认加载上下文的优点，也不会承袭其他上下文的缺点。  
  
### <a name="consider-using-application-domains"></a>考虑使用应用程序域  

 如果确定无法在应用程序的探测路径中部署某些程序集，请考虑为这些程序集创建新的应用程序域。 使用 <xref:System.AppDomainSetup> 可创建新的应用程序域，而使用 <xref:System.AppDomainSetup.ApplicationBase%2A?displayProperty=nameWithType> 属性可指定包含要加载的程序集的路径。 如果要探测多个目录，则可以将 <xref:System.AppDomainSetup.ApplicationBase%2A> 设置为根目录，并使用 <xref:System.AppDomainSetup.PrivateBinPath%2A?displayProperty=nameWithType> 属性标识要探测的子目录。 或者，可以创建多个应用程序域，并将每个应用程序域的 <xref:System.AppDomainSetup.ApplicationBase%2A> 设置为其程序集的相应路径。  
  
 请注意，可以使用 <xref:System.Reflection.Assembly.LoadFrom%2A?displayProperty=nameWithType> 方法加载这些程序集。 由于这些程序集此时位于探测路径中，因此会将它们加载到默认加载上下文（而非加载位置上下文）中。 不过，建议切换到 <xref:System.Reflection.Assembly.Load%2A?displayProperty=nameWithType> 方法并提供完整的程序集显示名称，从而确保始终使用正确的版本。  
  
## <a name="see-also"></a>请参阅

- <xref:System.Reflection.Assembly.Load%2A?displayProperty=nameWithType>
- <xref:System.Reflection.Assembly.LoadFrom%2A?displayProperty=nameWithType>
- <xref:System.Reflection.Assembly.LoadFile%2A?displayProperty=nameWithType>
- <xref:System.AppDomain.AssemblyResolve?displayProperty=nameWithType>
