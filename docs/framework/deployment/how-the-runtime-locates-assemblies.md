---
title: 运行时如何定位程序集
description: 了解公共语言运行时 (CLR) 如何在 .NET 中查找并绑定到构成应用程序的程序集。
ms.date: 03/30/2017
helpviewer_keywords:
- app.config files, assembly locations
- deploying applications [.NET Framework], assembly locations
- application configuration files, locating assemblies
- .NET Framework application deployment, locating assemblies
- locating assemblies
- assemblies [.NET Framework], location
ms.assetid: 772ac6f4-64d2-4cfb-92fd-58096dcd6c34
ms.openlocfilehash: 1b2ee58ccbd4bdfceb6300c20d5255718982f2e5
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96272522"
---
# <a name="how-the-runtime-locates-assemblies"></a>运行时如何定位程序集

若要成功部署 .NET Framework 应用程序，必须了解公共语言运行时如何查找和绑定到构成应用程序的程序集。 默认情况下，运行时尝试与生成应用程序的程序集的准确版本绑定。 可通过配置文件设置重写此默认行为。

在尝试查找程序集和解析程序集引用时，公共语言运行时会执行多个步骤。 以下各节将分别阐述每个步骤。 描述运行时如何查找程序集时，通常使用术语“探测”；它指一套用于根据名称和区域性查找程序集的试探法。

> [!NOTE]
> 可使用 Windows SDK 中附带的[程序集绑定日志查看器 (Fuslogvw.exe)](../tools/fuslogvw-exe-assembly-binding-log-viewer.md) 查看日志文件中的绑定信息。

## <a name="initiating-the-bind"></a>启动绑定

在运行时尝试解析其他程序集的引用时，开始查找和绑定到程序集的进程。 此引用可为静态，也可为动态。 编译器在生成时记录程序集清单的元数据中的静态引用。 由于调用各种方法（如 <xref:System.Reflection.Assembly.Load%2A?displayProperty=nameWithType>），所以将及时构造动态引用。

引用程序集的首选方式是使用完整引用，包括程序集名称、版本、区域性和公钥标记（若有）。 运行时按照本节稍后描述的步骤使用此信息查找程序集。 无论引用针对的程序集是静态还是动态，运行时均使用相同的解析进程。

还可通过仅向调用方法提供程序集的部分信息（例如，仅指定程序集名称）动态引用程序集。 在这种情况下，仅搜索程序集的应用程序目录，不进行其他检查。 通过使用任一方式加载程序集（ <xref:System.Reflection.Assembly.Load%2A?displayProperty=nameWithType> 或 <xref:System.AppDomain.Load%2A?displayProperty=nameWithType>），可执行部分引用。

最后，可使用 <xref:System.Reflection.Assembly.Load%2A?displayProperty=nameWithType> 等方法执行动态引用并仅提供部分信息；然后使用应用程序配置文件中的 [\<qualifyAssembly>](../configure-apps/file-schema/runtime/qualifyassembly-element.md) 元素限定该引用。 借助此元素，你可以提供应用程序配置文件（而不是代码）中的完全引用信息（名称、版本、区域性和公钥标记（若适用））。 如果想要完全限定应用程序目录之外的程序集引用，或者如果想要引用全局程序集缓存中的程序集且轻松指定配置文件（而不是代码）中的完全引用，可使用此技术。

> [!NOTE]
> 多个应用程序间共享的程序集不应使用此类型的部分引用。 因为配置设置是基于每个应用程序（而非每个程序集）应用的，所以使用此类部分引用的共享程序集需要使用共享程序集的每个应用程序的配置文件中都具有限定信息。

运行时使用以下步骤解析程序集引用：

1. 通过检查适用的配置文件（包括应用程序配置文件、发布服务器策略文件和计算机配置文件）[确定正确的程序集版本](#step1) 。 如果配置文件位于远程计算机，运行时必须首先查找并下载此应用程序配置文件。

2. [检查之前是否已绑定程序集名称](#step2) ，若如此，请使用先前加载的程序集。 如果加载程序集的先前请求失败，此请求会立即失败且不会尝试加载程序集。

    > [!NOTE]
    > 程序集绑定故障缓存是 .NET Framework 2.0 版本中的新增功能。

3. [检查全局程序集缓存](#step3)。 如果此处存在程序集，则运行时使用此程序集。

4. 使用以下步骤[探测程序集](#step4) ：

    1. 如果配置和发布服务器策略均不影响原始引用且绑定请求是使用 <xref:System.Reflection.Assembly.LoadFrom%2A?displayProperty=nameWithType> 方法创建的，运行时将检查是否存在位置提示。

    2. 如果配置文件中存在代码库，则运行时只检查此位置。 如果此探测失败，运行时确定绑定请求已失败且不执行其他探测。

    3. 使用 [探测章节](#step4)中所述的试探法探测程序集。 如果探测后找不到程序集，运行时将请求 Windows Installer 提供程序集。 这相当于按需安装功能。

        > [!NOTE]
        > 不会检查未带强名称的程序集版本，运行时也不会检查全局程序集缓存中未带强名称的程序集。

<a name="step1"></a>

## <a name="step-1-examining-the-configuration-files"></a>步骤 1：检查配置文件

可根据 3 个 XML 文件在不同级别上配置程序集绑定行为：

- 应用程序配置文件。

- 发布服务器策略文件。

- 计算机配置文件。

这些文件遵循相同的语法，并提供绑定重定向、代码位置和特定程序集的绑定模式等信息。 每个配置文件均可包含用于重定向绑定过程的 [\<assemblyBinding> 元素](../configure-apps/file-schema/runtime/assemblybinding-element-for-runtime.md)。 [\<assemblyBinding> 元素](../configure-apps/file-schema/runtime/assemblybinding-element-for-runtime.md)的子元素包含 [\<dependentAssembly> 元素](../configure-apps/file-schema/runtime/dependentassembly-element.md)。 [\<dependentAssembly> 元素](../configure-apps/file-schema/runtime/dependentassembly-element.md)的子元素包含 [\<assemblyIdentity> 元素](/visualstudio/deployment/assemblyidentity-element-clickonce-deployment)、[\<bindingRedirect> 元素](../configure-apps/file-schema/runtime/bindingredirect-element.md) 和 [\<codeBase> 元素](../configure-apps/file-schema/runtime/codebase-element.md)。

> [!NOTE]
> 这 3 个配置文件中均存在配置信息；并非所有配置文件中的所有元素都有效。 例如，绑定模式和专用路径信息仅存在于应用程序配置文件。 有关包含在每个文件内的完整信息列表，请参阅 [使用配置文件配置应用](../configure-apps/index.md)。

### <a name="application-configuration-file"></a>应用程序配置文件

首先，公共语言运行时检查应用程序配置文件是否存在重写调用程序集清单中存储的版本信息的相关信息。 可借助应用程序部署应用程序配置文件，但执行应用程序时不需要此文件。 通常几乎可瞬时完成此文件的检索，但如果应用程序基位于完成计算机上（例如，在基于 Internet Explorer Web 的方案中），必须下载配置文件。

对于客户端可执行文件，应用程序配置文件和应用程序的可执行文件驻留在同一目录中，并且它与扩展名为 .config 的可执行文件具有相同的基名称。 例如，C:\Program Files\Myapp\Myapp.exe 的配置文件是 C:\Program Files\Myapp\Myapp.exe.config。在基于浏览器的方案中，HTML 文件必须使用 \<link> 元素显式指向该配置文件。

以下代码提供了一个有关应用程序配置文件的简单示例。 此示例将 <xref:System.Diagnostics.TextWriterTraceListener> 添加到 <xref:System.Diagnostics.Debug.Listeners%2A> 集合，以便可以将调试信息记录到文件中。

```xml
<configuration>
   <system.diagnostics>
      <trace useGlobalLock="false" autoflush="true" indentsize="0">
         <listeners>
            <add name="myListener" type="System.Diagnostics.TextWriterTraceListener, system version=1.0.3300.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" initializeData="c:\myListener.log" />
         </listeners>
      </trace>
   </system.diagnostics>
</configuration>
```

### <a name="publisher-policy-file"></a>发布服务器策略文件

然后，运行时检查发布服务器策略文件（若存在）。 组件发布服务器将发布服务器策略文件作为修复文件或更新文件分配至共享组件。 这些文件包含共享组件的发布服务器所发布的的兼容性信息，其中此组件将程序集引用定向到新版本。 与应用程序和计算机配置文件不同，发布服务器策略文件位于必须在全局程序集缓存中安装的自己的程序集中。

以下示例阐述了发行者策略配置文件：

```xml
<configuration>
    <runtime>
        <assemblyBinding xmlns="urn:schemas-microsoft-com:asm.v1">

            <dependentAssembly>
                <assemblyIdentity name="asm6" publicKeyToken="c0305c36380ba429" />
                <bindingRedirect oldVersion="3.0.0.0" newVersion="2.0.0.0"/>
            </dependentAssembly>

        </assemblyBinding>
    </runtime>
</configuration>
```

若要创建程序集，可使用具有如下命令的 [Al.exe（程序集链接器）](../tools/al-exe-assembly-linker.md)工具：

```console
Al.exe /link:asm6.exe.config /out:policy.3.0.asm6.dll /keyfile: compatkey.dat /v:3.0.0.0
```

`compatkey.dat` 是一个强名称密钥文件。 此命令创建可置于全局程序集缓存中的强名称程序集。

> [!NOTE]
> 发布服务器策略会影响所有使用共享组件的应用程序。

发布服务器策略配置文件会重写来自应用程序（即，来自程序集清单或应用程序配置文件）的版本信息。 如果应用程序配置文件中不存在重定向程序集清单中指定版本的语句，发布服务器策略文件将重写程序集清单中指定的版本。 但是，如果应用程序配置文件中存在重定向语句，发布服务器策略将重写此版本（而不是清单中指定的版本）。

共享组件已更新且使用此组件的所有应用程序都应选取共享组件的新版本时，使用发行者策略文件。 发布服务器策略文件中的设置会重写应用程序配置文件中的设置，除非应用程序配置文件强制实施了安全模式。

#### <a name="safe-mode"></a>安全模式

发布服务器策略文件通常作为服务包或程序更新的一部分显式安装。 如果升级后的共享组件有任何问题，可使用安全模式忽略发布服务器策略文件中的重写。 安全模式由仅位于应用程序配置文件中的 \<publisherPolicy apply="yes**&#124;**no"/> 元素确定。 它指定是否应从绑定进程删除发布服务器策略配置信息。

可针对整个应用程序或所选程序集设置安全模式。 即，可关闭构成应用程序的所有程序集的策略，也可仅打开部分程序集的策略。 若要将发布服务器策略有选择地应用于构成应用程序的程序集，请设置 \<publisherPolicy apply\=no/> 并使用 \<**dependentAssembly**> 元素指定要影响的程序集。 若要将发布服务器策略应用于构成应用程序的所有程序集，请设置 \<publisherPolicy apply\=no/>，且不包含从属程序集元素。 有关配置的详细信息，请参阅 [使用配置文件配置应用](../configure-apps/index.md)。

### <a name="machine-configuration-file"></a>计算机配置文件

最后，运行时检查计算机配置文件。 此文件名为 Machine.config，驻留在本地计算机上安装有运行时的根目录的配置子目录中。 管理员可使用此文件来指定此计算机本地的程序集绑定限制。 计算机配置文件中的设置优先于所有其他配置设置 ；但是，这并不意味着所有配置设置都应置于此文件中。 管理员策略文件确定的版本为最终版本，且不能重写。 Machine.config 文件中指定的重写可影响所有应用程序。 有关配置文件的详细信息，请参阅 [使用配置文件配置应用](../configure-apps/index.md)。

<a name="step2"></a>

## <a name="step-2-checking-for-previously-referenced-assemblies"></a>步骤 2：检查以前引用的程序集

如果请求的程序集已先前调用中请求过，则公共语言运行时将使用已加载的程序集。 命名构成应用程序的程序集时，这可能会造成影响。 有关命名程序集的详细信息，请参阅 [程序集名称](../../standard/assembly/names.md)。

如果先前的程序集请求失败，此程序集的后续请求立即失败且不会尝试加载程序集。 从 .NET Framework 2.0 版开始，将缓存程序集绑定故障，且缓存的信息用于确定是否尝试加载此程序集。

> [!NOTE]
> 若要还原到 .NET framework 版本 1.0 和 1.1 的行为（即，不缓存绑定故障），请将 [\<disableCachingBindingFailures> 元素](../configure-apps/file-schema/runtime/disablecachingbindingfailures-element.md)包括到配置文件中。

<a name="step3"></a>

## <a name="step-3-checking-the-global-assembly-cache"></a>步骤 3：检查全局程序集缓存

对于强名称程序集，通过查看全局程序集缓存继续执行绑定进程。 全局程序集缓存存储可由同一计算机上多个应用程序使用的程序集。 全局程序集缓存中的所有程序集都必须具有强名称。

<a name="step4"></a>

## <a name="step-4-locating-the-assembly-through-codebases-or-probing"></a>步骤 4：通过基本代码或探测定位程序集

在通过使用调用程序集引用和配置文件中的信息确定正确的程序集版本，且已在全局程序集缓存中检查此版本（仅针对强名称程序集）之后，公共语言运行时将尝试查找此程序集。 查找程序集的过程涉及以下步骤：

1. 如果在应用程序配置文件中找到 [\<codeBase>](../configure-apps/file-schema/runtime/codebase-element.md) 元素，则运行时会检查指定的位置。 如果找到匹配项，则使用此程序集且不执行探测。 如果此处不存在程序集，则绑定请求失败。

2. 然后，运行时使用本节稍后指定的规则探测引用的程序集。

> [!NOTE]
> 如果目录中有多个版本的程序集，并且要引用该程序集的某个特定版本，则必须使用 [\<codeBase>](../configure-apps/file-schema/runtime/codebase-element.md) 元素而不是 [\<probing>](../configure-apps/file-schema/runtime/probing-element.md) 元素的 `privatePath` 特性。 如果使用 [\<probing>](../configure-apps/file-schema/runtime/probing-element.md) 元素，则运行时首次找到与引用的简单程序集名称匹配的程序集时，无论此匹配项是否正确，运行时都会停止探测。 如果此匹配项正确，则使用此程序集。 如果此匹配项不正确，则停止探测且绑定失败。

### <a name="locating-the-assembly-through-codebases"></a>通过基本代码查找程序集

通过使用配置文件中的 [\<codeBase>](../configure-apps/file-schema/runtime/codebase-element.md) 元素，可提供基本代码信息。 在运行时尝试探测引用的程序集之前，始终检查此基本代码。 如果包含最终版本重定向的发布服务器策略文件也包含 [\<codeBase>](../configure-apps/file-schema/runtime/codebase-element.md) 元素，则使用 [\<codeBase>](../configure-apps/file-schema/runtime/codebase-element.md) 元素。 例如，如果应用程序配置文件指定一个 [\<codeBase>](../configure-apps/file-schema/runtime/codebase-element.md) 元素，而重写应用程序信息的发布服务器策略文件也指定一个 [\<codeBase>](../configure-apps/file-schema/runtime/codebase-element.md) 元素，则使用发布服务器策略文件中的 [\<codeBase>](../configure-apps/file-schema/runtime/codebase-element.md) 元素。

如果在 [\<codeBase>](../configure-apps/file-schema/runtime/codebase-element.md) 元素指定的位置找不到匹配项，则绑定请求失败，且不再执行任何步骤。 如果运行时确定程序集与调用程序集的条件相匹配，则使用此程序集。 加载由给定 [\<codeBase>](../configure-apps/file-schema/runtime/codebase-element.md) 元素指定的文件时，运行时执行检查，确保名称、版本、区域性和公钥与调用程序集的引用相匹配。

> [!NOTE]
> 应用程序根目录外的被引用程序集必须具有强名称，并且必须安装在全局程序集缓存中，或者使用 [\<codeBase>](../configure-apps/file-schema/runtime/codebase-element.md) 元素指定。

### <a name="locating-the-assembly-through-probing"></a>通过探测查找程序集

如果应用程序配置文件中没有 [\<codeBase>](../configure-apps/file-schema/runtime/codebase-element.md) 元素，则运行时使用以下 4 个条件来探测程序集：

- 应用程序基，即正在执行应用程序的根位置。

- 区域性，即被引用的程序集的区域性属性。

- 名称，即被引用的程序集的名称。

- [\<probing>](../configure-apps/file-schema/runtime/probing-element.md) 元素的 `privatePath` 特性，这是根位置下用户定义的子目录列表。 可使用应用程序域的 <xref:System.AppDomainSetup.PrivateBinPath?displayProperty=nameWithType> 属性在应用程序配置文件和托管代码中指定此位置。 在托管代码中指定时，先探测托管代码 `privatePath` ，然后探测应用程序配置文件中指定的路径。

#### <a name="probing-the-application-base-and-culture-directories"></a>探测应用程序基和区域性目录

运行时始终先探测应用程序基，它可能是计算机的 URL 或应用程序根目录。 如果应用程序基中不存在引用的程序集且未提供区域性信息，则运行时将搜索具有程序集名称的所有子目录。 探测的目录包括：

- [application base] / [assembly name].dll

- [application base] / [assembly name] / [assembly name].dll

如果指定了被引用程序集的区域性信息，则只探测以下目录：

- [application base] / [culture] / [assembly name].dll

- [application base] / [culture] / [assembly name] / [assembly name].dll

#### <a name="probing-with-the-privatepath-attribute"></a>用 privatePath 属性进行探测

除区域性子目录和为被引用程序集指定的子目录外，运行时还探测使用 [\<probing>](../configure-apps/file-schema/runtime/probing-element.md) 元素的 `privatePath` 特性指定的目录。 使用 `privatePath` 属性指定的目录必须是应用程序根目录的子目录。 根据引用的程序集请求中是否包含区域性信息，探测的目录会所有不同。

在运行时首次找到与引用的简单程序集名称相匹配的程序集时，无论此匹配项是否正确，运行时都将停止探测。 如果此匹配项正确，则使用此程序集。 如果此匹配项不正确，则停止探测且绑定失败。

如果包括区域性，则探测以下目录：

- [application base] / [binpath] / [culture] / [assembly name].dll

- [application base] / [binpath] / [culture] / [assembly name] / [assembly name].dll

如果不包括区域性信息，则探测以下目录：

- [application base] / [binpath] / [assembly name].dll

- [application base] / [binpath] / [assembly name] / [assembly name].dll

#### <a name="probing-examples"></a>探测示例

给定以下信息：

- 引用的程序集名称：myAssembly

- 应用程序根目录：`http://www.code.microsoft.com`

- 配置文件中的 [\<probing>](../configure-apps/file-schema/runtime/probing-element.md) 元素指定：bin

- 区域性：de

运行时探测以下 URL：

- `http://www.code.microsoft.com/de/myAssembly.dll`

- `http://www.code.microsoft.com/de/myAssembly/myAssembly.dll`

- `http://www.code.microsoft.com/bin/de/myAssembly.dll`

- `http://www.code.microsoft.com/bin/de/myAssembly/myAssembly.dll`

##### <a name="multiple-assemblies-with-the-same-name"></a>具有相同名称的多个程序集

以下示例显示了如何配置具有相同名称的多个程序集。

```xml
<dependentAssembly>
   <assemblyIdentity name="Server" publicKeyToken="c0305c36380ba429" />
   <codeBase version="1.0.0.0" href="v1/Server.dll" />
   <codeBase version="2.0.0.0" href="v2/Server.dll" />
</dependentAssembly>
```

#### <a name="other-locations-probed"></a>探测的其他位置

还可使用当前的绑定上下文来确定程序集的位置。 在 COM 互操作方案中使用 <xref:System.Reflection.Assembly.LoadFrom%2A?displayProperty=nameWithType> 方法时，最常使用此操作。 如果某个程序集使用 <xref:System.Reflection.Assembly.LoadFrom%2A> 方法引用其他程序集，调用程序集的位置被视为可提示引用的程序集的位置。 如果找到匹配项，则加载此程序集。 如果未找到匹配项，则运行时将继续执行搜索语义，然后查询 Windows Installer 以提供程序集。 如果未提供与绑定请求匹配的程序集，则将引发异常。 如果引用了类型，则此异常为托管代码中的 <xref:System.TypeLoadException> 如果找不到正在加载的程序集，则为 <xref:System.IO.FileNotFoundException> 。

例如，如果 Assembly1 引用 Assembly2 且 Assembly1 是从 `http://www.code.microsoft.com/utils` 下载的，则此位置被视为可提示 Assembly2.dll 的位置。 然后运行时会在 `http://www.code.microsoft.com/utils/Assembly2.dll` 和 `http://www.code.microsoft.com/utils/Assembly2/Assembly2.dll` 中探测程序集。 如果在这两个位置中均未找到 Assembly2，则运行时将查询 Windows Installer。

## <a name="see-also"></a>请参阅

- [适用于程序集加载的最佳做法](best-practices-for-assembly-loading.md)
- [部署](index.md)
