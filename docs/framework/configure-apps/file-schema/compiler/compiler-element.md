---
title: <compiler> 元素
ms.date: 08/14/2018
f1_keywords:
- http://schemas.microsoft.com/.NetConfiguration/v2.0#compiler
- http://schemas.microsoft.com/.NetConfiguration/v2.0#configuration/system.codedom/compilers/compiler
helpviewer_keywords:
- compiler configuration elements, <compiler> element
- <compiler> element
- compiler configuration attributes
- compiler element
ms.assetid: 7a151659-b803-4c27-b5ce-1c4aa0d5a823
ms.openlocfilehash: 0abbe594754cbd70ec4732a1e7ef98e8e88bf167
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90544747"
---
# <a name="compiler-element"></a>\<compiler> 元素

指定语言提供程序的编译器配置属性。

[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.codedom>**](system-codedom-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<compilers>**](compilers-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<compiler>**

## <a name="syntax"></a>语法

```xml
<compiler
  language="languageName[;...;...]"
  extension="fileExtension[;...;...]"
  type="typeName, assemblyName"
  warningLevel="number"
  compilerOptions="option1 option2"
/>
```

## <a name="attributes-and-elements"></a>特性和元素

下列各节描述了特性、子元素和父元素。

### <a name="attributes"></a>特性

|属性|描述|
|---------------|-----------------|
|`compilerOptions`|可选特性。<br /><br /> 为编译指定其他特定于编译器的参数。 特性的值 `compilerOptions` 通常在编译器的编译器选项主题中列出。|
|`extension`|必需的特性。<br /><br /> 提供语言提供程序的源文件所使用的文件扩展名的分号分隔列表。 例如“.cs”。|
|`language`|必需的特性。<br /><br /> 提供语言提供程序支持的语言名称的分号分隔列表。 例如“C#;cs;csharp”。|
|`type`|必需的特性。<br /><br /> 指定语言提供程序的类型名称，包括包含提供程序实现的程序集的名称。 类型名称必须满足 [指定完全限定的类型名称](../../../reflection-and-codedom/specifying-fully-qualified-type-names.md)中定义的要求。|
|`warningLevel`|可选特性。<br /><br /> 指定默认编译器警告等级;确定语言提供程序将编译警告视为错误的级别。|

### <a name="child-elements"></a>子元素

|元素|描述|
|-------------|-----------------|
|[\<providerOption> 元素](provideroption-element.md)|指定语言提供程序的编译器版本特性。|

### <a name="parent-elements"></a>父元素

|元素|描述|
|-------------|-----------------|
|[\<configuration> 元素](../configuration-element.md)|公共语言运行时和 .NET Framework 应用程序所使用的每个配置文件中的根元素。|
|[\<system.codedom> 元素](system-codedom-element.md)|指定可用语言提供程序的编译器配置设置。|
|[\<compilers> 元素](compilers-element.md)|编译器配置元素的容器;包含零个或多个 `<compiler>` 元素。|

## <a name="remarks"></a>备注

每个 `<compiler>` 元素指定特定语言提供程序的编译器配置特性。 提供程序为 <xref:System.CodeDom.Compiler.CodeDomProvider?displayProperty=nameWithType> 特定语言扩展了类; `<compiler>` 元素定义了语言提供程序的编译器和代码生成器设置。

.NET Framework 在计算机配置文件 (Machine.config) 中定义初始编译器设置。 开发人员和编译器供应商可以添加新 <xref:System.CodeDom.Compiler.CodeDomProvider> 实现的配置设置。 使用 <xref:System.CodeDom.Compiler.CodeDomProvider.GetAllCompilerInfo%2A?displayProperty=nameWithType> 方法，以编程方式枚举计算机上的语言提供程序和编译器配置设置。

应用程序或 Web 配置文件中的编译器元素可以补充或覆盖计算机配置文件中的设置。 如果为相同的语言名称或相同的文件扩展名配置了多个提供程序实现，则最后一个匹配配置会重写该语言名称或文件扩展名的任何以前配置的提供程序。

## <a name="configuration-file"></a>配置文件

此元素可在计算机配置文件和应用程序配置文件中使用。

## <a name="example"></a>示例

下面的示例演示了一个典型的编译器配置元素：

```xml
<configuration>
  <system.codedom>
    <compilers>
      <!-- zero or more compiler elements -->
      <compiler
        language="c#;cs;csharp"
        extension=".cs"
        type="Microsoft.CSharp.CSharpCodeProvider, System,
          Version=2.0.3600.0, Culture=neutral,
          PublicKeyToken=b77a5c561934e089"
        compilerOptions="/optimize"
        warningLevel="1" />
    </compilers>
  </system.codedom>
</configuration>
```

## <a name="see-also"></a>请参阅

- <xref:System.CodeDom.Compiler.CompilerInfo>
- <xref:System.CodeDom.Compiler.CodeDomProvider>
- [配置文件架构](../index.md)
- [\<compilers> 元素](compilers-element.md)
- [指定完全限定的类型名称](../../../reflection-and-codedom/specifying-fully-qualified-type-names.md)
- [compilation 的 compilers 的 compiler 元素（ASP.NET 设置架构）](/previous-versions/dotnet/netframework-4.0/a15ebt6c(v=vs.100))
