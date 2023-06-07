---
title: SingleTagSectionHandler 的自定义元素
ms.date: 05/01/2017
f1_keywords:
- http://schemas.microsoft.com/.NetConfiguration/v2.0#configuration/sectionName
helpviewer_keywords:
- custom element
ms.assetid: e62056c6-b351-40eb-afc0-cc13fc44e45e
ms.openlocfilehash: a40f35838655f6021af0b2e966335803ec8c16b4
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/06/2020
ms.locfileid: "80635402"
---
# <a name="custom-element-for-singletagsectionhandler"></a>SingleTagSectionHandler 的自定义元素

定义自定义配置节中由 \<section> 元素定义并使用类的设置 <xref:System.Configuration.SingleTagSectionHandler> 。

[**\<configuration>**](configuration-element.md) &nbsp;&nbsp;*\<sectionName>*

## <a name="syntax"></a>语法

```xml
<sectionName key="value" key2="value2" />
```

## <a name="attributes"></a>特性

属性和属性值是用户定义的。

## <a name="parent-element"></a>父元素

|     | 描述 |
| --- | ----------- |
| [**\<configuration>**](configuration-element.md) | 公共语言运行时和 .NET Framework 应用程序所使用的每个配置文件中的根元素。 |

## <a name="child-elements"></a>子元素

无

## <a name="remarks"></a>备注

**\<sectionName>** 元素是由元素中的标记定义的自定义元素 [**\<section>**](section-element.md) [**\<configSections>**](configsections-element-for-configuration.md) 。 调用时，配置系统返回 <xref:System.Collections.IDictionary> 对象 <xref:System.Configuration.Configuration.GetSection(System.String)?displayProperty=nameWithType> 。

## <a name="example"></a>示例

下面的示例声明一个名为的自定义元素 **\<sampleSection>** ，该元素包含由类读取的设置 <xref:System.Configuration.SingleTagSectionHandler> ：

```xml
<configuration>
  <configSections>
    <section name="sampleSection"
             type="System.Configuration.SingleTagSectionHandler" />
  </configSections>
  <sampleSection setting1="Value1"
                 setting2="value two"
                 setting3="third value" />
</configuration>
```

## <a name="configuration-file"></a>配置文件

此元素可用于应用程序配置文件、计算机配置文件（*machine.config*）和不在应用程序目录级别*的 web.config 文件*。

## <a name="see-also"></a>另请参阅

- [.NET Framework 的配置文件架构](index.md)
