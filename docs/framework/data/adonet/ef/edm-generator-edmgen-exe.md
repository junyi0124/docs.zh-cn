---
title: EDM 生成器 (EdmGen.exe)
ms.date: 03/30/2017
ms.assetid: fe8297a1-1fc3-48ce-8eeb-f70f63f857aa
ms.openlocfilehash: da5b87fa76cbc8e44f6ed60b047e5a185c2aa603
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90542550"
---
# <a name="edm-generator-edmgenexe"></a>EDM 生成器 (EdmGen.exe)

EdmGen.exe 是一种用于处理实体框架模型和映射文件的命令行工具。 使用 EdmGen.exe 工具可以执行以下任务：

- 使用特定于数据源的 .NET Framework 数据提供程序连接到数据源，并生成概念模型 ( csdl) 、存储模型 ( ssdl) 和映射 ( 使用的文件) 。 有关详细信息，请参阅 [如何：使用 EdmGen.exe 生成模型和映射文件](how-to-use-edmgen-exe-to-generate-the-model-and-mapping-files.md)。

- 验证现有模型。 有关详细信息，请参阅 [如何：使用 EdmGen.exe 验证模型和映射文件](how-to-use-edmgen-exe-to-validate-model-and-mapping-files.md)。

- 生成包含从概念模型 (.csdl) 文件生成的对象类的 C# 或 Visual Basic 代码文件。 有关详细信息，请参阅 [如何：使用 EdmGen.exe 生成对象层代码](how-to-use-edmgen-exe-to-generate-object-layer-code.md)。

- 生成包含现有模型的预生成视图的 C# 或 Visual Basic 代码文件。 有关详细信息，请了解 [如何：预生成视图以提高查询性能](/previous-versions/dotnet/netframework-4.0/bb896240(v=vs.100))。

EdmGen.exe 工具安装在 .NET Framework 目录中。 多数情况下，它位于 C:\windows\Microsoft.NET\Framework\v4.0 中。 对于 64 位系统，它位于 C:\windows\Microsoft.NET\Framework64\v4.0 中。 你还可以从 Visual Studio 命令提示符访问 EdmGen.exe 工具 (单击 " **开始**"，指向 " **所有程序**"，指向 " **Microsoft Visual Studio 2010**"，指向 " **Visual Studio Tools**"，然后单击 " **Visual Studio 2010 命令提示符**) "。

## <a name="syntax"></a>语法

```console
EdmGen /mode:choice [options]
```

## <a name="mode"></a>模式

使用 EdmGen.exe 工具时，必须指定以下模式之一。

|“模式”|说明|
|----------|-----------------|
|`/mode:ValidateArtifacts`|验证 .csdl、.ssdl 和 .msl 文件并显示所有错误或警告。<br /><br /> 此选项需要至少一个 `/inssdl` 或 `/incsdl` 自变量。 如果指定 `/inmsl`，则还需要 `/inssdl` 和 `/incsdl` 自变量。|
|`/mode:FullGeneration`|使用 `/connectionstring` 选项中指定的数据库连接信息，生成 .csdl、.ssdl、.msl、对象层和视图文件。<br /><br /> 此选项需要一个 `/connectionstring` 参数以及一个 `/project` 参数或 `/outssdl`、`/outcsdl`、`/outmsdl`、`/outobjectlayer`、`/outviews`、`/namespace` 和 `/entitycontainer` 参数。|
|`/mode:FromSSDLGeneration`|根据指定的 .ssdl 文件生成 .csdl 和 .msl 文件、源代码和视图。<br /><br /> 此选项需要 `/inssdl` 参数，以及 `/project` 参数或 `/outcsdl`、`/outmsl`、`/outobjectlayer`、`/outviews`、`/namespace,` 和 `/entitycontainer` 参数。|
|`/mode:EntityClassGeneration`|创建包含根据 .csdl 文件生成的类的源代码文件。<br /><br /> 此选项需要 `/incsdl` 参数，以及 `/project` 参数或 `/outobjectlayer` 参数。 `/language` 参数是可选的。|
|`/mode:ViewGeneration`|创建包含根据 .csdl、.ssdl 和 .msl 文件生成的视图的源代码文件。<br /><br /> 此选项需要 `/inssdl`、`/incsdl`、`/inmsl,`，以及 `/project` 或 `/outviews` 自变量。 `/language` 参数是可选的。|

## <a name="options"></a>选项

|选项|说明|
|------------|-----------------|
|`/p[roject]:`\<string>|指定要使用的项目名称。 该项目名称用作命名空间设置、模型和映射文件的名称、对象源文件的名称以及视图生成源文件的名称的默认值。 实体容器名称设置为 \<project> Context。|
|`/prov[ider]:`\<string>|用于生成存储模型 (.ssdl) 文件的 .NET Framework 数据提供程序的名称。 默认提供程序是用于 SQL Server 的 .NET Framework 数据提供程序 (<xref:System.Data.SqlClient?displayProperty=nameWithType>)。|
|`/c[onnectionstring]:`\<connection string>|指定用于连接数据源的字符串。|
|`/incsdl:`\<file>|指定 .csdl 文件或 .csdl 文件所在的目录。 此自变量可多次指定，这样可以指定多个目录或 .csdl 文件。 当概念模型跨多个文件拆分时，对于生成类 (`/mode:EntityClassGeneration`) 或视图 (`/mode:ViewGeneration`)，指定多个目录十分有用。 如果希望验证多个模型 (`/mode:ValidateArtifacts`)，这样做也很有用。|
|`/refcsdl:`\<file>|指定用于解析源 .csdl 文件中的任何引用的其他 .csdl 文件。 （源 .csdl 文件是 `/incsdl` 选项指定的文件）。 `/refcsdl` 文件包含源 .csdl 文件所依赖的类型。 此自变量可多次指定。|
|`/inmsl:`\<file>|指定 .msl 文件或 .msl 文件所在的目录。 此自变量可多次指定，这样可以指定多个目录或 .msl 文件。 当概念模型跨多个文件拆分时，对于生成视图 (`/mode:ViewGeneration`)，指定多个目录十分有用。 如果希望验证多个模型 (`/mode:ValidateArtifacts`)，这样做也很有用。|
|`/inssdl:`\<file>|指定 .ssdl 文件或 .ssdl 文件所在的目录。 此参数可多次指定，这样可以指定多个目录或 .ssdl 文件。 如果希望验证多个模型 (`(/mode:ValidateArtifacts)`)，这样做很有用。|
|`/outcsdl:`\<file>|指定将创建的 .csdl 文件的名称。|
|`/outmsl:`\<file>|指定将创建的 .msl 文件的名称。|
|`/outssdl:`\<file>|指定将创建的 .ssdl 文件的名称。|
|`/outobjectlayer:`\<file>|指定包含根据 .csdl 文件生成的对象的源代码文件的名称。|
|`/outviews:`\<file>|指定包含所生成的视图的源代码文件的名称。|
|`/language:`[VB&#124;CSharp]|指定生成的源代码文件的语言。 默认语言为 C#。|
|`/namespace:`\<string>|指定要使用的模型命名空间。 命名空间是在运行 `/mode:FullGeneration` 或 `/mode:FromSSDLGeneration` 时在 .csdl 文件中设置的。 在运行 `/mode:EntityClassGeneration` 时不使用该命名空间。|
|`/entitycontainer:`\<string>|指定要应用于生成的模型和映射文件中 `<EntityContainer>` 元素的名称。|
|`/pl[uralize]`|对概念模型中 `Entity`、`EntitySet` 和 `NavigationProperty` 名称的单复数形式应用英语语言规则。 此选项将执行以下操作：<br /><br /> -将所有 `EntityType` 名称设置为单数形式。<br />-将所有 `EntitySet` 名称设置为复数形式。<br />-对于 `NavigationProperty` 最多返回一个实体的每个实体，使名称为单数形式。<br />-对于返回多个实体的每个 `NavigationProperty` ，将名称设置为复数形式。|
|`/SuppressForeignKeyProperties or /nofk`|防止外键列公开为概念模型中实体类型上的标量属性。|
|`/help` 或 `?`|显示该工具的命令语法和选项。|
|`/nologo`|禁止显示版权信息。|
|`/targetversion:` \<string>|将用于编译生成的代码的 .NET Framework 版本。 支持的版本是 4 和 4.5。 默认值为 4。|

## <a name="in-this-section"></a>本节内容

[如何：使用 EdmGen.exe 生成模型和映射文件](how-to-use-edmgen-exe-to-generate-the-model-and-mapping-files.md)

[如何：使用 EdmGen.exe 生成对象层代码](how-to-use-edmgen-exe-to-generate-object-layer-code.md)

[如何：使用 EdmGen.exe 验证模型和映射文件](how-to-use-edmgen-exe-to-validate-model-and-mapping-files.md)

## <a name="see-also"></a>请参阅

- [ADO.NET 实体数据模型工具](/previous-versions/dotnet/netframework-4.0/bb399249(v=vs.100))
- [实体数据模型](../entity-data-model.md)
- [CSDL、SSDL 和 MSL 规范](/ef/ef6/modeling/designer/advanced/edmx/csdl-spec)
