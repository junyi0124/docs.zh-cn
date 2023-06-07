---
title: 应用程序域
description: 了解应用程序域，这些域为 .NET 中的安全性、可靠性、版本控制和卸载程序集在应用程序之间提供了隔离边界。
ms.date: 03/30/2017
helpviewer_keywords:
- process boundaries for isolation
- application isolation
- application domains, about
- common language runtime, application domains
- application domains
- runtime, application domains
- isolation between applications
- code, verification process
- verification testing code
ms.assetid: 113a8bbf-6875-4a72-a49d-ca2d92e19cc8
ms.openlocfilehash: 246566265d55a3289ef37a2987ed9c40f051e3c8
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90553364"
---
# <a name="application-domains"></a>应用程序域

操作系统和运行时环境通常会在应用程序间提供某种形式的隔离。 例如，Windows 使用进程来隔离应用程序。 为确保在一个应用程序中运行的代码不会对其他不相关的应用程序产生不良影响，这种隔离是必需的。  
  
 应用程序域为安全性、可靠性、版本控制以及卸载程序集提供了隔离边界。 应用程序域通常由运行时宿主创建，运行时宿主负责在运行应用程序之前引导公共语言运行时。  
  
## <a name="the-benefits-of-isolating-applications"></a>隔离应用程序的优点

 以前使用进程边界来隔离在同一台计算机上运行的应用程序。 每一个应用程序被加载到单独的进程中，这样就将该应用程序与在同一台计算机上运行的其他应用程序相隔离。  
  
 隔离这些应用程序的原因在于内存地址是与进程相关的；在目标进程中，不能通过任何有意义的方式使用从一个进程传递到另一个进程的内存指针。 此外，您不能在两个进程间进行直接调用。 您必须代之以使用代理，它提供一定程度的间接性。  
  
 托管代码必须先通过一个验证过程，然后才能运行（除非管理员已授权跳过该验证）。 此验证过程将验证以下内容：这些代码是否会尝试访问无效的内存地址？是否会尝试执行某些导致进程（该代码运行时所在的进程）无法正常进行的其他操作？ 通过此验证测试的代码将被认为是类型安全的。 由于公共语言运行时能够验证代码是否为类型安全的代码，所以它可以提供与进程边界一样大的隔离级别，而其性能开销则要低得多。  
  
 应用程序域提供了一个更安全、用途更广的处理单元，公共语言运行时可使用该单元提供应用程序之间的隔离。 您可以在具有同等隔离级别（存在于单独的进程中）的单个进程中运行几个应用程序域，而不会造成进程间调用或进程间切换等方面的额外开销。 在一个进程内运行多个应用程序的能力显著增强了服务器的可伸缩性。  
  
 隔离应用程序对于应用程序安全也是十分重要的。 例如，您可以在单个浏览器进程中运行几个 Web 应用程序中的控件，同时使这些控件不能访问彼此的数据和资源。  
  
 应用程序域所提供的隔离具有以下优点：  
  
- 在一个应用程序中出现的错误不会影响其他应用程序。 因为类型安全的代码不会导致内存错误，所以使用应用程序域可以确保在一个域中运行的代码不会影响进程中的其他应用程序。  
  
- 能够在不停止整个进程的情况下停止单个应用程序。 使用应用程序域使您可以卸载在单个应用程序中运行的代码。  
  
    > [!NOTE]
    > 不能卸载单个程序集或类型。 只能卸载整个域。  
  
- 在一个应用程序中运行的代码不能直接访问其他应用程序中的代码或资源。 为了强制实施此隔离，公共语言运行时禁止在不同应用程序域中的对象之间进行直接调用。 要在各域之间传递对象，可以复制这些对象，或通过代理访问这些对象。 如果复制对象，那么对该对象的调用为本地调用。 也就是说，调用方和被引用的对象位于同一应用程序域中。 如果通过代理访问对象，那么对该对象的调用为远程调用。 在此情况下，调用方和被引用的对象位于不同的应用程序域中。 域间调用所采用的远程调用基础结构与两个进程间的调用或两台计算机间的调用的基础结构相同。 因此，被引用的对象的元数据必须对于两个应用程序域均可用，以便用 JIT 正确编译该方法调用。 如果调用域对被调用对象的元数据没有访问权，则编译可能失败，并引发类型为 <xref:System.IO.FileNotFoundException> 的异常。 有关详细信息，请参阅 [Remote Objects](/previous-versions/dotnet/netframework-4.0/72x4h507(v=vs.100))。 确定如何跨域访问对象的机制是由该对象决定的。 有关详细信息，请参阅 <xref:System.MarshalByRefObject?displayProperty=nameWithType>。  
  
- 代码行为的作用范围由它运行所在的应用程序决定。 换言之，应用程序域将提供应用程序版本策略等配置设置、它所访问的任意远程程序集的位置，以及加载到该域中的程序集的位置信息。  
  
- 向代码授予的权限可以由代码运行所在的应用程序域来控制。  
  
## <a name="application-domains-and-assemblies"></a>应用程序域和程序集

 本节描述应用程序域和程序集之间的关系。 在可以执行程序集中所包含的代码之前，必须将程序集加载到应用程序域中。 运行普通的应用程序会导致将几个程序集加载到一个应用程序域中。  
  
 程序集的加载方式决定其实时 (JIT) 编译代码是否可以在进程中由多个应用程序域共享，以及该程序集是否可以从进程中卸载。  
  
- 如果程序集是以非特定于域的形式进行加载，则共享相同安全授权集的所有应用程序域都可以共享相同的 JIT 编译代码，从而减少应用程序所需的内存。 但是，程序集则永远不能从进程中卸载。  
  
- 如果程序集不是以非特定于域的形式进行加载，则它必须在加载的每个应用程序域中都是 JIT 编译的。 但是，通过卸载程序集加载的所有应用程序域，可以从进程中卸载程序集。  
  
 运行时宿主决定在将运行时加载到进程中时是否以非特定于域的形式加载程序集。 对于托管应用程序，将 <xref:System.LoaderOptimizationAttribute> 特性应用于进程的入口点方法，并从关联的 <xref:System.LoaderOptimization> 枚举指定一个值。 对于托管公共语言运行时的非托管应用程序，在调用 [CorBindToRuntimeEx 函数](../unmanaged-api/hosting/corbindtoruntimeex-function.md)方法时，指定适当的标志。  
  
 有三个选项用于加载非特定于域的程序集：  
  
- <xref:System.LoaderOptimization.SingleDomain?displayProperty=nameWithType> 不以非特定于域的形式加载任何程序集（Mscorlib 除外，它始终以非特定于域的形式加载）。 此设置称作单域，因为它通常用在宿主只运行进程中的单个应用程序时。

- <xref:System.LoaderOptimization.MultiDomain?displayProperty=nameWithType> 以非特定于域的形式加载所有程序集。 此设置用于以下情况：进程中有多个应用程序域，所有这些应用程序域均运行相同的代码。

- <xref:System.LoaderOptimization.MultiDomainHost?displayProperty=nameWithType> 以非特定于域的形式加载强名称程序集（如果它们以及它们的所有依赖项都已在全局程序集缓存中安装）。 其他程序集都将针对它们加载的每个应用程序域分别进行加载和 JIT 编译，从而可以从进程中卸载。 如果您在同一进程中运行多个应用程序，或者如果您有混合的程序集，其中包括许多应用程序域共享的程序集和需要从进程中卸载的程序集，则可以使用此设置。
  
 以下程序集不能共享 JIT 编译代码：使用 <xref:System.Reflection.Assembly.LoadFrom%2A> 类的 <xref:System.Reflection.Assembly> 方法加载到“加载源”上下文中的程序集，或者使用 <xref:System.Reflection.Assembly.Load%2A> 方法的重载（指定字节数组）从图像加载的程序集。  
  
 使用 [Ngen.exe（本机映像生成器）](../tools/ngen-exe-native-image-generator.md)编译为本机代码的程序集如果在第一次加载到进程中时是以非特定于域的形式加载的，则可以在不同应用程序域之间共享这些程序集。  
  
 包含应用程序入口点的程序集的 JIT 编译代码只有在其所有依赖项都可以被共享的情况下，才可以被共享。  
  
 非特定于域的程序集可以进行多次 JIT 编译。 例如，如果两个应用程序域的安全授权集不同，则它们不能共享相同的 JIT 编译代码。 但是，JIT 编译程序集的每个副本都可以与其他具有相同授权集的应用程序域共享。  
  
 当您决定是否以非特定于域的形式加载程序集时，必须在减少内存占用和降低其他性能因素之间加以权衡。  
  
- 对于非特定于域的程序集，对静态数据和方法的访问较慢的原因在于需要隔离程序集。 访问该程序集的每一应用程序域都必须具有静态数据的单独副本，以避免跨域边界引用静态字段中的对象。 因此，运行时包含附加的逻辑，用以将调用方引导到静态数据或静态方法的适当副本。 这一额外的逻辑将降低调用速度。  
  
- 当以非特定于域的形式加载程序集时，必须找到并加载该程序集的所有依赖项，因为如果一个依赖项不能以非特定于域的形式加载，则会妨碍以非特定于域的形式加载程序集。  
  
## <a name="application-domains-and-threads"></a>应用程序域和线程

 应用程序域为安全性、版本控制、可靠性和托管代码的卸载形成隔离边界。 线程是公共语言运行时用来执行代码的操作系统构造。 在运行时，所有托管代码均加载到一个应用程序域中，并由一个或多个托管线程运行。  
  
 应用程序域和线程之间不具有一对一的相关性。 在任意给定时间，可以在单个应用程序域中执行几个线程，而且特定线程并不局限在单个应用程序域内。 也就是说，线程可以自由跨越应用程序域边界；不为每个应用程序域创建新线程。  
  
 在任意给定时间，每个线程都在一个应用程序域中执行。 在任何给定的应用程序域中，可能正在执行零个、一个或多个线程。 运行时会跟踪在哪些应用程序域中有哪些线程正在运行。 通过调用 <xref:System.Threading.Thread.GetDomain%2A?displayProperty=nameWithType> 方法，您可以随时确定线程执行所在的域。

### <a name="application-domains-and-cultures"></a>应用程序域和区域性

 区域性（由 <xref:System.Globalization.CultureInfo> 对象表示）与线程关联。 您可以通过使用 <xref:System.Globalization.CultureInfo.CurrentCulture%2A?displayProperty=nameWithType> 属性获取与当前正在执行的线程关联的区域性，并且您可以通过使用 <xref:System.Threading.Thread.CurrentCulture%2A?displayProperty=nameWithType> 属性获取或设置与当前正在执行的线程关联的区域性。 如果已使用 <xref:System.Threading.Thread.CurrentCulture%2A?displayProperty=nameWithType> 属性显式设置与线程关联的区域性，则当线程跨越应用程序域边界时，它将继续与该线程关联。 否则，在任何给定时间内与线程关联的区域性将由线程执行所在的应用程序域中的 <xref:System.Globalization.CultureInfo.DefaultThreadCurrentCulture%2A?displayProperty=nameWithType> 属性的值确定：  
  
- 如果该属性的值不是 `null`，则由该属性返回的区域性与线程（并因此由 <xref:System.Threading.Thread.CurrentCulture%2A?displayProperty=nameWithType> 和 <xref:System.Globalization.CultureInfo.CurrentCulture%2A?displayProperty=nameWithType> 属性返回）关联。  
  
- 如果该属性的值为 `null`，则当前系统区域性与线程关联。  
  
## <a name="programming-with-application-domains"></a>对应用程序域进行编程

 应用程序域通常由运行时宿主以编程的方式来创建和操作。 但是，有时应用程序还可能要和应用程序域结合起来使用。 例如，应用程序可能将应用程序组件加载到域中以便能够在不停止整个应用程序的情况下卸载域（以及该组件）。  
  
 <xref:System.AppDomain> 是应用程序域的程序设计界面。 此类包括各种方法，这些方法可以创建和卸载域、创建域中各类型的实例以及注册各种通知（如应用程序域卸载）。 下表列出了常用的 <xref:System.AppDomain> 方法。  
  
|AppDomain 方法|描述|  
|----------------------|-----------------|  
|<xref:System.AppDomain.CreateDomain%2A>|创建新的应用程序域。 建议使用此方法指定 <xref:System.AppDomainSetup> 对象的重载形式。 这是设置新域的各个属性的首选方式，这些属性包括应用程序基（即该应用程序的根目录）、域的配置文件的位置、以及公共语言运行时用于将程序集加载到域中的搜索路径等。|  
|<xref:System.AppDomain.ExecuteAssembly%2A> 和 <xref:System.AppDomain.ExecuteAssemblyByName%2A>|执行应用程序域中的程序集。 这是一个实例方法，因此它可用来执行另一个应用程序域（你拥有对该域的引用）中的代码。|  
|<xref:System.AppDomain.CreateInstanceAndUnwrap%2A>|在应用程序域中创建指定类型的实例，并返回一个代理。 使用此方法以避免将包含创建的类型的程序集加载到调用程序集中。|  
|<xref:System.AppDomain.Unload%2A>|执行域的正常关闭。 只有应用程序域中正在运行的所有线程都已停止或域中不再有运行的线程之后，才卸载该应用程序域。|  
  
> [!NOTE]
> 公共语言运行时不支持全局方法序列化，因此不能使用委托来执行其他应用程序域中的全局方法。  
  
 公共语言运行时承载接口规范中介绍的非托管接口也提供对应用程序域的访问。 运行时宿主可以使用非托管代码的接口在进程内创建应用程序域和获取对这些应用程序域的访问。  
  
## <a name="the-complus_loaderoptimization-environment-variable"></a>COMPLUS_LoaderOptimization 环境变量

 用于设置可执行应用程序的默认加载程序优化策略的环境变量。  
  
### <a name="syntax"></a>语法  
  
```env  
COMPLUS_LoaderOptimization = 1  
```  
  
### <a name="remarks"></a>备注

 典型应用程序必须先将几个程序集加载到一个应用程序域中，然后才能执行其所包含的代码。  
  
 程序集的加载方式决定了其实时 (JIT) 编译的代码是否可由进程中的多个应用程序域共享。  
  
- 如果程序集以非特定于域的形式加载，则所有共享相同安全授权集的应用程序域都可以共享相同的 JIT 编译代码。 这将减少应用程序所需的内存。  
  
- 如果程序集不是以非特定于域的形式加载，则必须在加载程序集的每个应用程序域中对其进行 JIT 编译，并且加载程序不得跨应用程序域共享国际资源。  
  
 在设置为 1 时，COMPLUS_LoaderOptimization 环境标志强制运行时主机以非特定于域的方式（称为 SingleDomain）加载所有程序集。 SingleDomain 不以非特定于域的形式加载任何程序集（Mscorlib 除外，它始终以非特定于域的形式加载）。 此设置称作单域，因为它通常用在宿主只运行进程中的单个应用程序时。  
  
> [!CAUTION]
> COMPLUS_LoaderOptimization 环境标志旨在用于诊断和测试方案。 启用该标志会导致速度严重减慢，并会增加内存使用率。  
  
### <a name="code-example"></a>代码示例

 可通过将 `COMPLUS_LoaderOptimization=1` 追加到 HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\services\IISADMIN 键中环境的多字符串值中，来强制所有程序集不以 IISADMIN 服务的非特定于域的形式加载。  
  
```env  
Key = HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\services\IISADMIN  
Name = Environment  
Type = REG_MULTI_SZ  
Value (to append) = COMPLUS_LoaderOptimization=1  
```  
  
## <a name="see-also"></a>请参阅

- <xref:System.AppDomain?displayProperty=nameWithType>
- <xref:System.MarshalByRefObject?displayProperty=nameWithType>
- [使用应用程序域和程序集进行编程](index.md)
- [使用应用程序域](use.md)
