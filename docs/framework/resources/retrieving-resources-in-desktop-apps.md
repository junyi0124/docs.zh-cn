---
title: 检索桌面应用程序中的资源
description: 检索桌面应用中的资源。 将默认（非特定）区域性的资源与主程序集一起打包，并为每个区域性创建附属程序集。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- deploying applications [.NET Framework], resources
- resource files, deploying
- resource files, packaging
- application resources, packaging
- localization
- versioning satellite assemblies
- retrieving localized resources
- application resources, deploying
- packaging application resources
- versioning resources
- translating resources into languages
- localizing resources
ms.assetid: eca16922-1c46-4f68-aefe-e7a12283641f
ms.openlocfilehash: 26e4367d28193ce731198ee0ba3d3b35d83cf19c
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96254540"
---
# <a name="retrieving-resources-in-desktop-apps"></a>检索桌面应用程序中的资源

使用 .NET Framework 桌面应用中的本地化资源时，最好用主程序集打包默认或非特定区域性的资源，并为应用支持的每种语言或区域性单独创建附属程序集。 可以使用下一节中介绍的 <xref:System.Resources.ResourceManager> 类访问已命名的资源。 如果选择不在主程序集和附属程序集中嵌入资源，也可以按本文后面的 [从 .resources 文件中检索资源](#from_file) 一节中所述直接访问二进制 .resources 文件。  若要检索 Windows 8.x 应用商店应用中的资源，请参阅[在 Windows 应用商店应用中创建和检索资源](/previous-versions/windows/apps/hh694557(v=vs.140))。  
  
<a name="from_assembly"></a>

## <a name="retrieving-resources-from-assemblies"></a>从程序集中检索资源  

 <xref:System.Resources.ResourceManager> 类提供对运行时资源的访问权限。 使用 <xref:System.Resources.ResourceManager.GetString%2A?displayProperty=nameWithType> 方法检索字符串资源和 <xref:System.Resources.ResourceManager.GetObject%2A?displayProperty=nameWithType> 或使用 <xref:System.Resources.ResourceManager.GetStream%2A?displayProperty=nameWithType> 方法检索非字符串资源。 每个方法都有两种重载：  
  
- 单一参数是包含资源名称的字符串的重载。 该方法尝试为当前线程区域性检索资源。 有关详细信息，请参阅 <xref:System.Resources.ResourceManager.GetString%28System.String%29>、 <xref:System.Resources.ResourceManager.GetObject%28System.String%29>和 <xref:System.Resources.ResourceManager.GetStream%28System.String%29> 方法。  
  
- 具有两个参数的重载：一个字符串包含资源名称，一个 <xref:System.Globalization.CultureInfo> 对象表示要对其检索资源的区域性。 如果找不到该区域性的资源集，资源管理器将使用回退规则检索相应的资源。 有关详细信息，请参阅 <xref:System.Resources.ResourceManager.GetString%28System.String%2CSystem.Globalization.CultureInfo%29>、 <xref:System.Resources.ResourceManager.GetObject%28System.String%2CSystem.Globalization.CultureInfo%29>和 <xref:System.Resources.ResourceManager.GetStream%28System.String%2CSystem.Globalization.CultureInfo%29> 方法。  
  
 资源管理器使用资源回退进程控制应用检索区域性特定资源的方式。 有关详细信息，请参阅 [Packaging and Deploying Resources](packaging-and-deploying-resources-in-desktop-apps.md)中的“资源回退进程”一节。 有关实例化 <xref:System.Resources.ResourceManager> 对象的详细信息，请参阅 <xref:System.Resources.ResourceManager> 类主题中的“实例化 ResourceManager 对象”一节。  
  
### <a name="retrieving-string-data-an-example"></a>检索字符串数据：示例  

 下面的示例调用 <xref:System.Resources.ResourceManager.GetString%28System.String%29> 方法检索当前 UI 区域性的字符串资源。 它包括英语（美国）区域性的非特定字符串资源和法语（法国）和俄语（俄罗斯）区域性的本地化资源。 下面的英语（美国）资源位于名为 Strings.txt 的文件中：  
  
```text
TimeHeader=The current time is  
```  
  
 法语（法国）资源位于名为 Strings.fr-FR.txt 的文件中：  
  
```text
TimeHeader=L'heure actuelle est  
```  
  
 俄语（俄罗斯）资源位于名为 Strings.ru-RU.txt 的文件中：  
  
```text
TimeHeader=Текущее время —  
```  
  
 此示例的源代码（在代码的 C# 版本中位于名为 GetString.cs 的文件中，在 Visual Basic 版本中位于名为 GetString.vb 的文件中）定义包含四种区域性名称（资源可用的三种区域性和西班牙语（西班牙）区域性）的字符串数组。 一个随机执行 5 次的循环选择其中一种区域性并将其分配到 <xref:System.Threading.Thread.CurrentCulture%2A?displayProperty=nameWithType> 和 <xref:System.Globalization.CultureInfo.CurrentUICulture%2A?displayProperty=nameWithType> 属性。 然后调用 <xref:System.Resources.ResourceManager.GetString%28System.String%29> 方法检索与一天当中的时间一起显示的本地化的字符串。  
  
 [!code-csharp[Conceptual.Resources.Retrieving#3](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.resources.retrieving/cs/getstring.cs#3)]
 [!code-vb[Conceptual.Resources.Retrieving#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.resources.retrieving/vb/getstring.vb#3)]  
  
 以下批处理 (.bat) 文件编译该示例，并在相应的目录中生成附属程序集。 为 C# 语言和编译器提供了命令。 对于 Visual Basic ，将 `csc` 更改为 `vbc`，并将 `GetString.cs` 更改为 `GetString.vb`。  
  
```console
resgen strings.txt  
csc GetString.cs -resource:strings.resources  
  
resgen strings.fr-FR.txt  
md fr-FR  
al -embed:strings.fr-FR.resources -culture:fr-FR -out:fr-FR\GetString.resources.dll  
  
resgen strings.ru-RU.txt  
md ru-RU  
al -embed:strings.ru-RU.resources -culture:ru-RU -out:ru-RU\GetString.resources.dll  
```  
  
 当前 UI 区域性为西班牙语（西班牙）时，请注意该示例会显示英语语言资源，因为西班牙语语言资源不可用，而英语是该示例的默认区域性。  
  
### <a name="retrieving-object-data-two-examples"></a>检索对象数据：2 个示例  

 可以使用 <xref:System.Resources.ResourceManager.GetObject%2A> 和 <xref:System.Resources.ResourceManager.GetStream%2A> 方法检索对象数据。 这包括基元数据类型、可序列化对象和以二进制格式存储的对象（如图像）。  
  
 下面的示例使用 <xref:System.Resources.ResourceManager.GetStream%28System.String%29> 方法检索应用启动初始窗口中使用的位图。 以下源代码位于名为 CreateResources.cs 的文件中（C# 版本）或位于名为 CreateResources.vb 的文件中（Visual Basic 版本），它能生成包含序列化图像的 .resx 文件。 在这种情况下，图片从一个名为 SplashScreen.jpg 的文件中加载；可以修改文件名以替换你自己的图像。  
  
 [!code-csharp[Conceptual.Resources.Retrieving#4](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.resources.retrieving/cs/createresources.cs#4)]
 [!code-vb[Conceptual.Resources.Retrieving#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.resources.retrieving/vb/createresources.vb#4)]  
  
 以下代码将检索该资源，并在 <xref:System.Windows.Forms.PictureBox> 控件中显示图像。  
  
 [!code-csharp[Conceptual.Resources.Retrieving#5](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.resources.retrieving/cs/getstream.cs#5)]
 [!code-vb[Conceptual.Resources.Retrieving#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.resources.retrieving/vb/getstream.vb#5)]  
  
 可以使用以下批处理文件生成 C# 示例。 对于 Visual Basic，将 `csc` 更改为 `vbc`，并将源代码文件的扩展名由 `.cs` 更改为 `.vb`。  
  
```console
csc CreateResources.cs  
CreateResources  
  
resgen AppResources.resx  
  
csc GetStream.cs -resource:AppResources.resources  
```  
  
 下面的示例使用 <xref:System.Resources.ResourceManager.GetObject%28System.String%29?displayProperty=nameWithType> 方法反序列化一个自定义对象。 该示例包含一个名为 UIElements.cs （对于 Visual Basic 则为 UIElements.vb）的源代码文件，用于定义以下名为 `PersonTable`的结构。 此结构应由显示表列的本地化名称的常规表显示例程使用。 请注意， `PersonTable` 结构标有 <xref:System.SerializableAttribute> 属性。  
  
 [!code-csharp[Conceptual.Resources.Retrieving#6](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.resources.retrieving/cs/example.cs#6)]
 [!code-vb[Conceptual.Resources.Retrieving#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.resources.retrieving/vb/example.vb#6)]  
  
 下面的代码来自名为 CreateResources.cs（对于 Visual Basic 则为 CreateResources.vb）的文件，该代码创建一个名为 UIResources.resx 的 XML 资源文件，该文件存储有表标题和包含已针对英语语言本地化的应用的信息的 `PersonTable` 对象。  
  
 [!code-csharp[Conceptual.Resources.Retrieving#7](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.resources.retrieving/cs/example1.cs#7)]
 [!code-vb[Conceptual.Resources.Retrieving#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.resources.retrieving/vb/example.vb#7)]  
  
 下面的代码位于名为 GetObject.cs (GetObject.vb) 的源代码文件中，然后检索资源并将其显示在控制台上。  
  
 [!code-csharp[Conceptual.Resources.Retrieving#8](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.resources.retrieving/cs/example2.cs#8)]
 [!code-vb[Conceptual.Resources.Retrieving#8](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.resources.retrieving/vb/example2.vb#8)]  
  
 可以生成必要的资源文件和程序集，并通过执行以下批处理文件运行该应用。 必须使用 `/r` 选项提供具有对 UIElements.dll 的引用的 Resgen.exe，以便其能够访问有关 `PersonTable` 结构的信息。 如果使用 C#，请将 `vbc` 编译器名称替换为 `csc`，并将 `.vb` 扩展名替换为 `.cs`。  
  
```console
vbc -t:library UIElements.vb  
vbc CreateResources.vb -r:UIElements.dll  
CreateResources  
  
resgen UIResources.resx  -r:UIElements.dll  
vbc GetObject.vb -r:UIElements.dll -resource:UIResources.resources  
  
GetObject.exe  
```  
  
## <a name="versioning-support-for-satellite-assemblies"></a>附属程序集的版本控制支持  

 默认情况下， <xref:System.Resources.ResourceManager> 对象检索请求的资源时，会寻找版本号与主程序集版本号相匹配的附属程序集。 部署应用后，建议更新主程序集或特定资源附属程序集。 .NET Framework 提供对主程序集和附属程序集的版本控制支持。  
  
 <xref:System.Resources.SatelliteContractVersionAttribute> 属性提供对主程序集的版本控制支持。 在应用的主程序集上指定此属性，无需更新主程序集的附属程序集即可更新和重新部署主程序集。 更新主程序集后，递增主程序集的版本号，但附属协定版本号保持不变。 资源管理器检索请求的资源时，会加载此属性指定的附属程序集版本。  
  
 发行者策略程序集提供对附属程序集的版本控制支持。 你可以更新并重新部署附属程序集，而不用更新主程序集。 更新附属程序集后，递增其版本号，并将其附带到发行者策略程序集中。 在发行者策略程序集中，指定新附属程序集为后向兼容其之前版本。 资源管理器会使用 <xref:System.Resources.SatelliteContractVersionAttribute> 属性确定附属程序集的版本，但程序集加载程序将绑定到发行者策略所指定的附属程序集版本。 有关发行者策略程序集的详细信息，请参阅 [创建发行者策略文件](../configure-apps/how-to-create-a-publisher-policy.md)。  
  
 若要启用完全的程序集版本控制支持，建议你在 [全局程序集缓存](../app-domains/gac.md) 中部署具有强名称的程序集，并将不具有强名称的程序集部署在应用程序目录中。 若在应用程序目录中部署具有强名称的程序集，则无法在更新程序集时递增附属程序集的版本号。 相反，必须在使用更新的代码替换现有代码处执行就地更新，并保持相同的版本号。 例如，若要使用完全指定的程序集名称 "myApp.resources, Version=1.0.0.0, Culture=de, PublicKeyToken=b03f5f11d50a3a" 更新版本 1.0.0.0 的附属程序集，请使用已编译同一个完全指定的程序集名称 "myApp.resources, Version=1.0.0.0, Culture=de, PublicKeyToken=b03f5f11d50a3a" 的更新的 myApp.resources.dll 来覆盖它。 请注意，在附属程序集文件上使用就地更新会使应用难以准确确定附属程序集的版本。  
  
 有关程序集版本控制的详细信息，请参阅 [程序集版本控制](../../standard/assembly/versioning.md) 和 [运行时如何定位程序集](../deployment/how-the-runtime-locates-assemblies.md)。  
  
<a name="from_file"></a>

## <a name="retrieving-resources-from-resources-files"></a>从 .resources 文件中检索资源  

 如果选择不在附属程序集中部署资源，你仍可以使用 <xref:System.Resources.ResourceManager> 对象直接访问 .resources 文件中的资源。 要执行此操作，必须正确部署.resources 文件。 然后使用 <xref:System.Resources.ResourceManager.CreateFileBasedResourceManager%2A?displayProperty=nameWithType> 方法实例化 <xref:System.Resources.ResourceManager> 对象，并指定包含独立 .resources 文件的目录。  
  
### <a name="deploying-resources-files"></a>部署 .resources 文件  

 将 .resources 文件嵌入应用程序程序集和附属程序集后，每个附属程序集都具有相同的文件名，但被放在反射附属程序集区域性的子目录中。 与此相反，从 .resources 文件直接访问资源时，可以将所有 .resources 文件放在单一目录（通常为应用程序目录的子目录）中。 应用的默认 .resources 文件名称仅包含一个根名称，不带有其区域性的指示（例如 strings.resources）。 每个本地化的区域性资源存储在名称包含根名称，后带有区域性标记所组成的文件中（例如 strings.ja.resources 或 strings.de-DE.resources）。

 下图显示资源文件应被放置在目录结构中的何处。 它还提供了.resource 文件的命名约定。  

 ![显示应用程序主目录的插图。](./media/retrieving-resources-in-desktop-apps/resource-application-directory.gif)  
  
### <a name="using-the-resource-manager"></a>使用资源管理器  

 创建资源并将其放置在相应的目录中后，通过调用 <xref:System.Resources.ResourceManager> 方法创建 <xref:System.Resources.ResourceManager.CreateFileBasedResourceManager%28System.String%2CSystem.String%2CSystem.Type%29> 对象以使用资源。 第一个参数指定应用的默认 .resources 文件的根名称（在上一节的示例中为 "strings"）。 第二个参数指定的资源的位置（上一个示例中为 "Resources"）。 第三个参数指定要使用的 <xref:System.Resources.ResourceSet> 实现。 如果第三个参数为 `null`，则使用默认运行时 <xref:System.Resources.ResourceSet> 。  
  
> [!NOTE]
> 请勿使用独立 .resources 文件部署 ASP.NET 应用。 这可能会导致锁定问题并破坏 XCOPY 部署。 建议部署附属程序集中的 ASP.NET 资源。 有关更多信息，请参见 [ASP.NET Web Page Resources Overview](/previous-versions/aspnet/ms227427(v=vs.100))。  
  
 实例化 <xref:System.Resources.ResourceManager> 对象后，使用前文所述的 <xref:System.Resources.ResourceManager.GetString%2A>、 <xref:System.Resources.ResourceManager.GetObject%2A>和 <xref:System.Resources.ResourceManager.GetStream%2A> 方法检索资源。 但是，直接从 .resources 文件中检索资源与从程序集中检索嵌入的资源有所不同。 从 .resources 文件中检索资源时， <xref:System.Resources.ResourceManager.GetString%28System.String%29>、 <xref:System.Resources.ResourceManager.GetObject%28System.String%29>和 <xref:System.Resources.ResourceManager.GetStream%28System.String%29> 方法总是忽略当前区域性检索默认区域性的资源。 若要检索应用的当前区域性资源或指定区域性的资源，必须调用 <xref:System.Resources.ResourceManager.GetString%28System.String%2CSystem.Globalization.CultureInfo%29>、 <xref:System.Resources.ResourceManager.GetObject%28System.String%2CSystem.Globalization.CultureInfo%29>或 <xref:System.Resources.ResourceManager.GetStream%28System.String%2CSystem.Globalization.CultureInfo%29> 方法并指定要检索资源的区域性。 若要检索当前区域性的资源，请将 <xref:System.Globalization.CultureInfo.CurrentCulture%2A?displayProperty=nameWithType> 属性的值指定为 `culture` 参数。 如果资源管理器无法检索 `culture`的资源，则使用标准资源回退规则检索相应的资源。  
  
### <a name="an-example"></a>示例  

 下面的示例说明资源管理器如何直接从 .resources 文件中检索资源。 此示例由三个基于文本的资源文件组成，区域性分别为英语（美国）、法语（法国）和俄语（俄罗斯）。 英语（美国）为示例的默认区域性。 其资源存储在以下名为 Strings.txt 的文件中：  
  
```text
Greeting=Hello  
Prompt=What is your name?  
```  
  
 法语(法国) 区域性的资源存储在以下名为 Strings.fr-FR.txt 的文件中：  
  
```text
Greeting=Bon jour  
Prompt=Comment vous appelez-vous?  
```  
  
 俄语(俄罗斯) 区域性的资源存储在以下名为 Strings.ru-RU.txt 的文件中：  
  
```text
Greeting=Здравствуйте  
Prompt=Как вас зовут?  
```  
  
 以下是该实例的源代码。 该示例为英语（美国）、英语（加拿大）、法语（法国）和俄语（俄罗斯）区域性实例化 <xref:System.Globalization.CultureInfo> 对象，并将以上每一种语言作为当前区域性。 <xref:System.Resources.ResourceManager.GetString%28System.String%2CSystem.Globalization.CultureInfo%29?displayProperty=nameWithType> 方法提供 <xref:System.Globalization.CultureInfo.CurrentCulture%2A?displayProperty=nameWithType> 属性的值作为 `culture` 参数来检索相应的区域性指定资源。  
  
 [!code-csharp[Conceptual.Resources.Retrieving#9](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.resources.retrieving/cs/example3.cs#9)]
 [!code-vb[Conceptual.Resources.Retrieving#9](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.resources.retrieving/vb/example3.vb#9)]  
  
 可以通过运行以下批处理文件编译该示例的 C# 版本。 如果使用 Visual Basic，请将 `csc` 替换为 `vbc`，并将 `.cs` 扩展名替换为 `.vb`。  
  
```console
Md Resources  
Resgen Strings.txt Resources\Strings.resources  
Resgen Strings.fr-FR.txt Resources\Strings.fr-FR.resources  
Resgen Strings.ru-RU.txt Resources\Strings.ru-RU.resources  
  
csc Example.cs  
```  
  
## <a name="see-also"></a>请参阅

- <xref:System.Resources.ResourceManager>
- [桌面应用中的资源](index.md)
- [打包和部署资源](packaging-and-deploying-resources-in-desktop-apps.md)
- [运行时如何定位程序集](../deployment/how-the-runtime-locates-assemblies.md)
- [在 Windows 应用商店应用中创建和检索资源](/previous-versions/windows/apps/hh694557(v=vs.140))
