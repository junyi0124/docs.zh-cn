---
title: <providerOption> 元素
ms.date: 03/30/2017
f1_keywords:
- provideroption
helpviewer_keywords:
- <provideroption> element
- providerOptions
- provideroption element
ms.assetid: 014f2e0b-c0b5-4fc4-92d3-73f02978b2a1
ms.openlocfilehash: 9374fbaf7ceb61e5b72335417d32a08525477e0d
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91149629"
---
# <a name="provideroption-element"></a>\<providerOption> 元素

指定语言提供程序的编译器版本特性。  

[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.codedom>**](system-codedom-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<compilers>**](compilers-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<compiler>**](compiler-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<providerOption>**

## <a name="syntax"></a>语法  
  
```xml  
<providerOption  
  name="option-name"  
  value="option-value"  
/>  
```  
  
## <a name="attributes-and-elements"></a>特性和元素  

 下列各节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  
  
|属性|描述|  
|---------------|-----------------|  
|`name`|必需的特性。<br /><br /> 指定选项的名称;例如 "CompilerVersion"。|  
|`value`|必需的特性。<br /><br /> 指定选项的值;例如，"v 3.5"。|  
  
### <a name="child-elements"></a>子元素  

 无。  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<configuration> 元素](../configuration-element.md)|公共语言运行库和 .NET Framework 应用程序所使用的每个配置文件中的根元素。|  
|[\<system.codedom> 元素](system-codedom-element.md)|指定可用语言提供程序的编译器配置设置。|  
|[\<compilers> 元素](compilers-element.md)|编译器配置元素的容器;包含零个或多个 `<compiler>` 元素。|  
|[\<compiler> 元素](compiler-element.md)|指定语言提供程序的编译器配置属性。|  
  
## <a name="remarks"></a>备注  

 在 .NET Framework 版本3.5 中，代码文档对象模型 (CodeDOM) 代码提供程序可以使用元素支持特定于提供程序的选项 `<providerOption>` 。  
  
 3.5 .NET Framework 包括更新的 .NET Framework 2.0 程序集，并提供包含新类型的3.5 版本的新程序集。 Microsoft c # 和 Visual Basic 代码提供程序包含在 .NET Framework 2.0 程序集中，但已更新为支持版本3.5 编译器。 默认情况下，更新的代码提供程序为版本2.0 编译器生成代码。 您可以使用 `<providerOption>` 元素将目标编译器版本更改为3.5。 为此，请将属性指定为 "CompilerVersion" `name` ，并为属性指定 "v 3.5" `value` 。 必须在版本号前面加上小写 "v"。  
  
 可以通过将 `<providerOption>` 元素添加到 .NET Framework 2.0 Machine.config 或根 Web.config 文件来使版本规范成为全局版本。 如果将 Machine.config 文件中的默认编译器版本更新为3.5，则可以通过使用 `<providerOption>` 应用程序配置文件中的元素，将其更改为每个应用程序2.0。  
  
 CodeDOM 代码提供程序实现者可以通过提供采用类型为的参数的构造函数来处理自定义选项 `providerOptions` <xref:System.Collections.Generic.IDictionary%602> 。  
  
## <a name="example"></a>示例  

 下面的示例演示如何指定应使用 c # 代码提供程序的3.5 版。  
  
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
        warningLevel="1" >  
          <providerOption  
            name="CompilerVersion"  
            value="v3.5" />  
      </compiler>  
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
