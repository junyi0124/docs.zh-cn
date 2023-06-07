---
title: C# 保留的特性：全局属性
ms.date: 04/09/2020
description: 特性提供编译器用来了解程序的更多语义的元数据
ms.openlocfilehash: f855f32cd7349da34ffbd20fededdf42c3023938
ms.sourcegitcommit: c91110ef6ee3fedb591f3d628dc17739c4a7071e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/15/2020
ms.locfileid: "81389804"
---
# <a name="reserved-attributes-assembly-level-attributes"></a>保留的特性：程序集级别特性

大多数特性应用于特定语言元素，如类或方法；但是，一些特性是全局特性 - 它们应用于整个程序集或模块。 例如，<xref:System.Reflection.AssemblyVersionAttribute> 属性可用于将版本信息嵌入程序集，如下所示：

```csharp
[assembly: AssemblyVersion("1.0.0.0")]
```

全局特性出现在源代码中任何顶级 `using` 指令之后和任何类型、模块或命名空间声明之前。 全局特性可以出现在多个源文件中，但必须在单个编译过程中编译这些文件。 Visual Studio 将全局特性添加到 .NET Framework 项目中的 AssemblyInfo.cs 文件中。 这些特性不会添加到 .NET Core 项目中。

程序集特性是提供程序集相关信息的值。 它们分为以下几类：

- 程序集标识特性
- 信息性特性
- 程序集清单特性

## <a name="assembly-identity-attributes"></a>程序集标识特性

三个特性（与强名称（如果适用））组合起来可以确定程序集的标识：名称、版本和区域性。 这些特性构成程序集的全名，在代码中引用程序集时必需使用。 可使用特性设置程序集的版本和区域性。 但是，创建程序集时，由编译器、[程序集信息对话框](/visualstudio/ide/reference/assembly-information-dialog-box)中的 Visual Studio IDE 或程序集链接器 (Al.exe) 设置名称值。 程序集名称基于程序集清单。 <xref:System.Reflection.AssemblyFlagsAttribute> 属性指定程序集的多个副本是否可以共存。

下表显示标识特性。

|特性|目标|
|---------------|-------------|
|<xref:System.Reflection.AssemblyVersionAttribute>|指定程序集的版本。|
|<xref:System.Reflection.AssemblyCultureAttribute>|指定程序集支持的区域性。|
|<xref:System.Reflection.AssemblyFlagsAttribute>|指定程序集是否支持在同一计算机上、同一进程中或同一应用程序域中并行执行。|

## <a name="informational-attributes"></a>信息性特性

你可使用信息性特性为程序集提供其他公司或产品信息。 下表显示 <xref:System.Reflection?displayProperty=nameWithType> 命名空间中定义的信息性属性。

|特性|目标|
|---------------|-------------|
|<xref:System.Reflection.AssemblyProductAttribute>|指定程序集清单的产品名称。|
|<xref:System.Reflection.AssemblyTrademarkAttribute>|指定程序集清单的商标。|
|<xref:System.Reflection.AssemblyInformationalVersionAttribute>|为程序集清单指定信息性版本。|
|<xref:System.Reflection.AssemblyCompanyAttribute>|为程序集清单指定公司名称。|
|<xref:System.Reflection.AssemblyCopyrightAttribute>|定义为程序集清单指定版权的自定义属性。|
|<xref:System.Reflection.AssemblyFileVersionAttribute>|设置 Win32 文件版本资源的特定版本号。|
|<xref:System.CLSCompliantAttribute>|指示程序集是否符合公共语言规范 (CLS)。|

## <a name="assembly-manifest-attributes"></a>程序集清单特性

程序集清单特性可用于提供程序集清单中的信息。 特性包括标题、说明、默认别名和配置。 下表显示 <xref:System.Reflection?displayProperty=nameWithType> 命名空间中定义的程序集清单属性。

|特性|目标|
|---------------|-------------|
|<xref:System.Reflection.AssemblyTitleAttribute>|为程序集清单指定程序集标题。|
|<xref:System.Reflection.AssemblyDescriptionAttribute>|为程序集清单指定程序集说明。|
|<xref:System.Reflection.AssemblyConfigurationAttribute>|为程序集清单指定程序集配置（如零售或调试）。|
|<xref:System.Reflection.AssemblyDefaultAliasAttribute>|定义程序集清单的友好默认别名|
