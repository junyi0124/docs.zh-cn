---
title: <supportedRuntime> 配置元素-.NET
ms.date: 04/02/2019
f1_keywords:
- http://schemas.microsoft.com/.NetConfiguration/v2.0#supportedRuntime
- http://schemas.microsoft.com/.NetConfiguration/v2.0#configuration/startup/supportedRuntime
helpviewer_keywords:
- supportedRuntime element
- <supportedRuntime> element
ms.assetid: 1ae16e23-afbe-4de4-b413-bc457f37b69f
ms.openlocfilehash: 79b49cbc9b122e6591d07643a341841b262edff4
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91201702"
---
# <a name="supportedruntime-element"></a>\<supportedRuntime> 元素

指定应用程序支持的公共语言运行时版本和（可选） .NET Framework 版本。  

[\<configuration>](../configuration-element.md)  
&nbsp;&nbsp;[\<startup>](startup-element.md)  
&nbsp;&nbsp;&nbsp;&nbsp;**\<supportedRuntime>**  

## <a name="syntax"></a>语法

```xml
<supportedRuntime version="runtime version" sku="sku id"/>
```

## <a name="attributes"></a>特性

|属性|描述|
|---------------|-----------------|
|**version**|可选特性。<br /><br /> 一个字符串值，它指定此应用程序支持的公共语言运行时 (CLR) 版本。 有关属性的有效值 `version` ，请参阅 ["运行时版本" 值](#version) 部分。 **注意：**  通过 .NET Framework 3.5，"*运行时版本*" 值采用 *主*格式。*次要*。*生成*。 从 .NET Framework 4 开始，只需要主要版本号和次要版本号 (即，"4.0.30319" 而不是 "v" ) 。 建议使用较短字符串。|
|**sku**|可选特性。<br /><br /> 一个字符串值，该值指定库存单位 (SKU)，库存单位则指定此应用程序支持的 .NET Framework 版本。<br /><br /> 从 .NET Framework 4.0 起，建议使用 `sku` 特性。  若存在该特性，则它指示应用面向的 .NET Framework 版本。<br /><br /> 有关 sku 属性的有效值，请参阅 ["sku id" 值](#sku) 一节。|

## <a name="remarks"></a>备注

如果 **\<supportedRuntime>** 应用程序配置文件中不存在该元素，则使用用于生成应用程序的运行时版本。

**\<supportedRuntime>** 元素应由使用版本1.1 或更高版本的运行时生成的所有应用程序使用。 构建为仅支持1.0 版运行时的应用程序必须使用 [\<requiredRuntime>](requiredruntime-element.md) 元素。

> [!NOTE]
> 如果使用 [CorBindToRuntimeByCfg](../../../unmanaged-api/hosting/corbindtoruntimebycfg-function.md) 函数指定配置文件，则必须将 `<requiredRuntime>` 元素用于运行时的所有版本。 `<supportedRuntime>`当使用[CorBindToRuntimeByCfg](../../../unmanaged-api/hosting/corbindtoruntimebycfg-function.md)时，将忽略元素。  
  
对于支持从 .NET Framework 1.1 到 3.5 的运行时版本的应用，支持多个运行时版本时，第一个元素应指定优先级最高的版本，最后一个元素应指定优先级最低的版本。 对于支持 .NET Framework 4.0 或更高版本的应用程序，该 `version` 属性指示 CLR 版本（对于 .NET Framework 4 及更高版本是通用的）， `sku` 属性指示应用面向的单一 .NET Framework 版本。

如果 **\<supportedRuntime>** `sku` 配置文件中存在具有属性的元素，并且安装的 .NET Framework 版本低于指定的受支持版本，应用程序将无法运行，而是显示一条消息要求安装支持的版本。 否则，应用程序会尝试在任何已安装的版本上运行，但如果该版本与该版本不完全兼容，则它可能会发生意外行为。  (.NET Framework 版本之间的兼容性差异，请参阅 [.NET Framework 中的应用程序兼容性](../../../migration-guide/application-compatibility.md)) 。因此，建议你在应用程序配置文件中包括此元素，以便更轻松地诊断错误。  (在创建新项目时由 Visual Studio 自动生成的配置文件。 ) 
  
> [!NOTE]
> 如果你的应用程序使用旧的激活路径（如 [CorBindToRuntimeEx 函数](../../../unmanaged-api/hosting/corbindtoruntimeex-function.md)），并且你希望这些路径激活 CLR 的版本4（而不是早期版本），或者如果你的应用程序是 .NET Framework 使用 .NET Framework 早期版本生成的混合模式程序集生成的，则在受支持的运行时列表中指定 .NET Framework 4 并不足够。 此外，在配置文件的[ \<startup> 元素](startup-element.md)中，你必须将 `useLegacyV2RuntimeActivationPolicy` 属性设置为 `true` 。 但是，将此特性设置为 `true` 意味着使用 .NET Framework 4 （而不是生成它们的运行时）运行使用 .NET Framework 的早期版本生成的所有组件。

我们建议你使用应用程序可在其上运行的所有 .NET Framework 版本来测试这些应用程序。

<a name="version"></a>

## <a name="runtime-version-values"></a>“运行时版本”值

`runtime`特性指定特定应用程序所需的公共语言运行时 (CLR) 版本。 请注意，所有 .NET Framework v4. x 版本都指定 `v4.0` CLR。 下表列出了属性的 *运行时版本* 值的有效值 `version` 。

|.NET Framework 版本|`version` 特性|
|----------------------------|-------------------------|
|1.0|"v1.0.3705"|
|1.1|"v1.1.4322"|
|2.0|"v2.0.50727"|
|3.0|"v2.0.50727"|
|3.5|"v2.0.50727"|
|4.0-4。8|"v4.0"|

## <a name="sku-id-values"></a><a name="sku"></a> "sku id" 值

该 `sku` 属性使用目标框架名字对象 (TFM) ，以指示应用程序的目标和需要运行的 .NET Framework 版本。 下表列出了属性支持的有效值 `sku` ，从 .NET Framework 4 开始。

|.NET Framework 版本|`sku` 特性|
|----------------------------|---------------------|
|4.0|".NETFramework,Version=v4.0"|
|4.0，客户端配置文件|".NETFramework,Version=v4.0,Profile=Client"|
|4.0，平台更新 1|"..Netframework，Version = v 4.0.1 "|
|4.0，客户端配置文件，Update 1|"..Netframework，Version = v 4.0.1，Profile = Client "|
|4.0，平台更新 2|"..Netframework，Version = v 4.0.2 "|
|4.0，客户端配置文件，Update 2|"..Netframework，Version = v 4.0.2，Profile = Client "|
|4.0，平台更新 3|"..Netframework，Version = v 4.0.3 "|
|4.0，客户端配置文件，Update 3|"..Netframework，Version = v 4.0.3，Profile = Client "|
|4.5|".NETFramework,Version=v4.5"|
|4.5.1|".NETFramework,Version=v4.5"|
|4.5.2|".NETFramework,Version=v4.5"|
|4.6|".NETFramework,Version=v4.5"|
|4.6.1|".NETFramework,Version=v4.5"|
|4.6.2|"..Netframework，Version = v 4.6.2 "|
|4.7|"..Netframework，Version = 4.7 "|
|4.7.1|"..Netframework，Version = v 4.7.1 "|
|4.7.2|"..Netframework，Version = v 4.7.2 "|
|4.8|"..Netframework，Version = v4.0 "|

## <a name="example"></a>示例

下面的示例演示如何在配置文件中指定支持的运行时版本。 配置文件指示应用面向 .NET Framework 4.7。

```xml
<configuration>
   <startup>
      <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.7" />
   </startup>
</configuration>
```

## <a name="configuration-file"></a>配置文件

此元素可用于应用程序配置文件中。

## <a name="see-also"></a>请参阅

- [启动设置架构](index.md)
- [配置文件架构](../index.md)
- [进程内并行执行](../../../deployment/in-process-side-by-side-execution.md)
