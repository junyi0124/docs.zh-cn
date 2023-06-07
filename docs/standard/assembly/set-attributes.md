---
title: 设置程序集属性
description: 可以设置 .NET 程序集的程序集属性，包括程序集标识、信息性、程序集清单和强名称属性。
ms.date: 08/20/2019
helpviewer_keywords:
- assemblies [.NET], attributes
- assembly binding, attributes
- assembly manifest, attributes
ms.assetid: 36a98a81-b5b5-4c19-912a-11f91eff7f4e
dev_langs:
- csharp
- vb
- cpp
ms.openlocfilehash: 32318d647dee8f3f397e3497e7c2da640bd492d0
ms.sourcegitcommit: 279fb6e8d515df51676528a7424a1df2f0917116
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2020
ms.locfileid: "92687685"
---
# <a name="set-assembly-attributes"></a>设置程序集属性

程序集特性是提供程序集相关信息的值。 特性分为以下几组信息：

- 程序集标识特性

- 信息性特性

- 程序集清单特性

- 强名称特性

## <a name="assembly-identity-attributes"></a>程序集标识特性

三个特性与强名称（如果适用）组合起来可以确定程序集的标识：名称、版本和区域性。 这些特性构成程序集的完整名称，在代码中引用程序集时必需使用。 特性可用于设置程序集的版本和区域性。 编译器或 [程序集链接器 (Al.exe)](../../framework/tools/al-exe-assembly-linker.md) 依据包含程序集清单的文件在创建该程序集时设置名称值。

下表介绍了版本和区域性特性。

|程序集标识特性|描述|
|---------------------------------|-----------------|
|<xref:System.Reflection.AssemblyCultureAttribute>|表示程序集所支持的区域性的枚举字段。 程序集还可以指定区域性独立性，表示它包含默认区域性的资源。 **注意：** 运行时将任何没有将区域性属性设置为 null 的程序集视为附属程序集。 此类程序集需遵循附属程序集绑定规则。 有关详细信息，请参阅[运行时如何定位程序集](../../framework/deployment/how-the-runtime-locates-assemblies.md)。|
|<xref:System.Reflection.AssemblyFlagsAttribute>|设置程序集特性（例如是否可以并行运行程序集）的值。|
|<xref:System.Reflection.AssemblyVersionAttribute>|以 *主版本* . *次版本* . *内部版本* . *修订版本* 为格式的数值（例如 2.4.0.0）。 公共语言运行时使用此值在强名称程序集中执行绑定操作。 **注意：** 如果 <xref:System.Reflection.AssemblyInformationalVersionAttribute> 特性不适用于程序集，则 <xref:System.Windows.Forms.Application.ProductVersion%2A?displayProperty=nameWithType>、<xref:System.Windows.Forms.Application.UserAppDataPath%2A?displayProperty=nameWithType> 和 <xref:System.Windows.Forms.Application.UserAppDataRegistry%2A?displayProperty=nameWithType> 属性使用由 <xref:System.Reflection.AssemblyVersionAttribute> 特性指定的版本号。|

下面的示例演示了如何将版本和区域性特性应用到程序集中。

```cpp
// Set version number for the assembly.
[assembly:AssemblyVersionAttribute("4.3.2.1")];
// Set culture as German.
[assembly:AssemblyCultureAttribute("de")];
```

```csharp
// Set version number for the assembly.
[assembly:AssemblyVersionAttribute("4.3.2.1")]
// Set culture as German.
[assembly:AssemblyCultureAttribute("de")]
```

```vb
' Set version number for the assembly.
<Assembly:AssemblyVersionAttribute("4.3.2.1")>
' Set culture as German.
<Assembly:AssemblyCultureAttribute("de")>
```

## <a name="informational-attributes"></a>信息性特性

信息性特性可用于提供程序集的其他公司或产品信息。 下表介绍了可应用于程序集的信息性特性。

|信息性特性|描述|
|-----------------------------|-----------------|
|<xref:System.Reflection.AssemblyCompanyAttribute>|指定公司名称的字符串值。|
|<xref:System.Reflection.AssemblyCopyrightAttribute>|指定版权信息的字符串值。|
|<xref:System.Reflection.AssemblyFileVersionAttribute>|指定 Win32 文件版本号的字符串值。 通常默认为程序集版本。|
|<xref:System.Reflection.AssemblyInformationalVersionAttribute>|指定公共语言运行时未使用的版本信息（例如完整的产品版本号）的字符串值。 **注意：** 如果将此属性应用于程序集，可以在运行时使用 <xref:System.Windows.Forms.Application.ProductVersion%2A?displayProperty=nameWithType> 属性获取它指定的字符串。 此字符串还用于 <xref:System.Windows.Forms.Application.UserAppDataPath%2A?displayProperty=nameWithType> 和 <xref:System.Windows.Forms.Application.UserAppDataRegistry%2A?displayProperty=nameWithType> 属性所提供的路径和注册表项。|
|<xref:System.Reflection.AssemblyProductAttribute>|指定产品信息的字符串值。|
|<xref:System.Reflection.AssemblyTrademarkAttribute>|指定商标信息的字符串值。|

这些特性可以出现在程序集的 Windows 属性页上，或使用 **/win32res** 编译器选项重写，以指定自己的 Win32 资源文件。

## <a name="assembly-manifest-attributes"></a>程序集清单特性

程序集清单特性可用于在程序集清单中提供标题、说明、默认别名和配置等信息。 下表介绍了程序集清单特性。

|程序集清单特性|描述|
|---------------------------------|-----------------|
|<xref:System.Reflection.AssemblyConfigurationAttribute>|表示程序集配置（如零售或调试）的字符串值。 运行时不使用此规则。|
|<xref:System.Reflection.AssemblyDefaultAliasAttribute>|指定引用程序集所用的默认别名的字符串值。 此值会在程序集本身的名称不友好（例如 GUID 值）的情况下提供一个友好名称。 此值还可用于完整程序集名称的缩写。|
|<xref:System.Reflection.AssemblyDescriptionAttribute>|指定描述程序集性质和用途简短说明的字符串值。|
|<xref:System.Reflection.AssemblyTitleAttribute>|指定程序集的友好名称的字符串值。 例如，名为 comdlg 的程序集的标题可能是“Microsoft 通用对话框控件”。|

## <a name="strong-name-attributes"></a>强名称特性

强名称特性可用于设置程序集的强名称。 下表介绍了强名称特性。

|强名称属性|描述|
|----------------------------|-----------------|
|<xref:System.Reflection.AssemblyDelaySignAttribute>|表示正在使用延迟签名的布尔值。|
|<xref:System.Reflection.AssemblyKeyFileAttribute>|表示文件名称的字符串值，该文件包含公钥（如果使用延迟签名）或同时包含作为参数传递给此特性的构造函数的公钥和私钥。 请注意，文件名与输出文件路径（.exe 或.dll）有关，与源文件路径无关 。|
|<xref:System.Reflection.AssemblyKeyNameAttribute>|表示包含作为参数传递给此特性的构造函数的密钥对的密钥容器。|

以下代码示例显示了使用延迟签名创建具有名为 myKey.snk 的公钥文件的强名称程序集时要应用的属性。

```cpp
[assembly:AssemblyKeyFileAttribute("myKey.snk")];
[assembly:AssemblyDelaySignAttribute(true)];
```

```csharp
[assembly:AssemblyKeyFileAttribute("myKey.snk")]
[assembly:AssemblyDelaySignAttribute(true)]
```

```vb
<Assembly:AssemblyKeyFileAttribute("myKey.snk")>
<Assembly:AssemblyDelaySignAttribute(True)>
```

## <a name="see-also"></a>请参阅

- [创建程序集](create.md)
