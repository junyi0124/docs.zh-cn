---
title: <codeBase> 元素
ms.date: 03/30/2017
f1_keywords:
- http://schemas.microsoft.com/.NetConfiguration/v2.0#codeBase
- http://schemas.microsoft.com/.NetConfiguration/v2.0#configuration/runtime/assemblyBinding/dependentAssembly/codeBase
helpviewer_keywords:
- <codeBase> element
- container tags, <codeBase> element
- codeBase element
ms.assetid: d48a3983-2297-43ff-a14d-1f29d3995822
ms.openlocfilehash: 475b7df55ed509157c1da0aeb8f979de238c72b5
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/06/2020
ms.locfileid: "70971887"
---
# <a name="codebase-element"></a>\<codeBase> 元素

指定公共语言运行时可在何处找到程序集。

[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<runtime>**](runtime-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<assemblyBinding>**](assemblybinding-element-for-runtime.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<dependentAssembly>**](dependentassembly-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<codeBase>**

## <a name="syntax"></a>语法

```xml
   <codeBase
        version="Assembly version"
        href="URL of assembly"/>
```

## <a name="attributes-and-elements"></a>特性和元素

下列各节描述了特性、子元素和父元素。

### <a name="attributes"></a>特性

|属性|说明|
|---------------|-----------------|
|`href`|必需的特性。<br /><br /> 指定运行时可在其中找到程序集的指定版本的 URL。|
|`version`|必需的特性。<br /><br /> 指定基本代码所应用的程序集的版本。 程序集版本号的格式为 "主要版本. 次要版本. 内部版本.*修订*版本"。|

## <a name="version-attribute"></a>version 特性

|值|说明|
|-----------|-----------------|
|版本号的每个部分的有效值为0至65535。|不适用。|

### <a name="child-elements"></a>子元素

无。

### <a name="parent-elements"></a>父元素

|元素|描述|
|-------------|-----------------|
|`buildproviders`|定义用于编译自定义资源文件的生成提供程序的集合。 您可以拥有任意数量的生成提供程序。|
|`compilation`|配置 ASP.NET 使用的所有编译设置。|
|`configuration`|公共语言运行时和 .NET Framework 应用程序所使用的每个配置文件中的根元素。|
|`System.web`|为 ASP.NET 配置节指定根元素。|

## <a name="remarks"></a>注解

为了使运行时 **\<codeBase>** 在计算机配置文件或发布服务器策略文件中使用该设置，该文件还必须重定向程序集版本。 应用程序配置文件可以具有基本代码设置，而不会重定向程序集版本。 确定要使用的程序集版本后，运行时将从确定版本的文件应用基本代码设置。 如果未指定基本代码，则运行时以常规方式探测程序集。

如果程序集具有强名称，则基本代码设置可以是本地 intranet 或 Internet 上的任何位置。 如果程序集是私有程序集，则 codebase 设置必须是相对于应用程序目录的路径。

对于没有强名称的程序集，将忽略版本，加载程序将使用内部的第一个外观 \<codebase> \<dependentAssembly> 。 如果应用程序配置文件中存在重定向绑定到另一个程序集的条目，则即使程序集版本与绑定请求不匹配，重定向也将优先。

## <a name="example"></a>示例

下面的示例演示如何指定运行时在何处可以找到程序集。

```xml
<configuration>
   <runtime>
      <assemblyBinding xmlns="urn:schemas-microsoft-com:asm.v1">
         <dependentAssembly>
            <assemblyIdentity name="myAssembly"
                              publicKeyToken="32ab4ba45e0a69a1"
                              culture="neutral" />
            <codeBase version="2.0.0.0"
                      href="http://www.litwareinc.com/myAssembly.dll"/>
         </dependentAssembly>
      </assemblyBinding>
   </runtime>
</configuration>
```

## <a name="see-also"></a>另请参阅

- [运行时设置架构](index.md)
- [配置文件架构](../index.md)
- [指定程序集的位置](../../../../standard/assembly/location.md)
- [运行时如何定位程序集](../../../deployment/how-the-runtime-locates-assemblies.md)
