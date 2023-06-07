---
title: <clear>NameValueSectionHandler 和 DictionarySectionHandler 的元素
ms.date: 05/01/2017
f1_keywords:
- http://schemas.microsoft.com/.NetConfiguration/v2.0#configuration/sectionName/clear
helpviewer_keywords:
- clear Element
- <clear> Element
ms.assetid: ff2294ec-fb82-4b0c-933e-ae185433fc7b
ms.openlocfilehash: f6d860f35d22002030ffa3d09dd0d8a96116bf5e
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/06/2020
ms.locfileid: "77214741"
---
# <a name="clear-element-for-namevaluesectionhandler-and-dictionarysectionhandler"></a>\<clear>NameValueSectionHandler 和 DictionarySectionHandler 的元素

清除节中所有先前定义的设置。

[**\<configuration>**](configuration-element.md)\
&nbsp;&nbsp;[**\<sectionName>**](custom-element-2.md)\
&nbsp;&nbsp;&nbsp;&nbsp;**\<clear>**

## <a name="syntax"></a>语法

```xml
<clear />
```

## <a name="attributes"></a>特性

无

## <a name="parent-element"></a>父元素

|     | 描述 |
| --- | ------------|
| [**\<sectionName>** Element](custom-element-2.md) | 定义使用和类的自定义配置节的设置 <xref:System.Configuration.NameValueSectionHandler> <xref:System.Configuration.DictionarySectionHandler> 。 |

## <a name="child-elements"></a>子元素

无

## <a name="remarks"></a>备注

你可以使用 **\<clear>** 元素从你的应用程序中删除在配置文件层次结构中较高级别上定义的所有设置。

## <a name="example"></a>示例

此示例定义了计算机配置文件和应用程序配置文件，并演示了如何使用 **\<clear>** 应用程序配置文件中的元素清除之前在计算机配置文件中定义的部分。

以下计算机配置文件代码声明了节 **\<mySection>** ：

```xml
<!-- Machine.config file -->
<configuration>
  <configSections>
    <section name="mySection" type="System.Configuration.NameValueSectionHandler,System" />
  </configSections>
  <mySection>
    <add key="key1" value="value1" />
    <add key="key2" value="value2" />
  </mySection>
</configuration>
```

以下应用程序配置文件代码将从中删除所有设置 **\<mySection>** 。 应用程序无法检索在计算机配置文件的部分中的中声明的任何设置 **\<mySection>** 。

```xml
<!-- Application configuration file -->
<configuration>
  <mySection>
    <clear/>
  </mySection>
</configuration>
```

## <a name="configuration-file"></a>配置文件

此元素可用于应用程序配置文件、计算机配置文件（*machine.config*）和不在应用程序目录级别的*web.config 文件。*

## <a name="see-also"></a>另请参阅

- [.NET Framework 的配置文件架构](index.md)
