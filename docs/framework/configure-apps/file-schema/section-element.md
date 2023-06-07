---
title: <section> element
ms.date: 05/01/2017
f1_keywords:
- http://schemas.microsoft.com/.NetConfiguration/v2.0#configuration/configSections/section
- http://schemas.microsoft.com/.NetConfiguration/v2.0#configuration/configSections/sectionGroup/section
helpviewer_keywords:
- section Element
- <section> Element
ms.assetid: ec7d4110-2403-47ac-8218-499bfe9d5ddb
ms.openlocfilehash: 88f74c02ef627e9136e4437ffa150c36445266a3
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/06/2020
ms.locfileid: "79153720"
---
# <a name="section-element"></a>\<section> 元素

包含配置节声明。

[**\<configuration>**](configuration-element.md)\
&nbsp;&nbsp;[**\<configSections>**](configsections-element-for-configuration.md)\
&nbsp;&nbsp;&nbsp;&nbsp;**\<section>**

[**\<configuration>**](configuration-element.md)\
&nbsp;&nbsp;[**\<configSections>**](configsections-element-for-configuration.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<sectionGroup>**](sectiongroup-element-for-configsections.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<section>**

## <a name="syntax"></a>语法

```xml
<section name="section name"
         type="configuration section handler class, assembly"
         allowDefinition="Everywhere|MachineOnly|MachineToApplication"
         allowLocation="true|false" />
```

## <a name="required-attributes"></a>必需属性

|           | 说明 |
| --------- | ----------- |
| **name**  | 指定配置节的名称。 |
| type  | 指定从配置文件读取节的配置节处理程序类的名称。 类型值具有语法 "完全限定的节-名称、简单程序集名称"。 简单程序集名称是没有 *.dll*文件扩展名的根文件名。 |

## <a name="optional-attributes"></a>可选属性

以下属性仅适用于 ASP.NET 应用程序。 对于其他应用程序类型，配置系统将忽略这些属性。

|                     | 说明 |
| ------------------- | ----------- |
| **allowDefinition** | 指定可在其中使用节的配置文件。 使用以下值之一：<br><br>**无处不在**<br>允许在任何配置文件中使用节。 这是默认设置。<br>**MachineOnly**<br>允许部分仅在计算机配置文件（*machine.config*）中使用。<br>**MachineToApplication**<br>允许在计算机配置文件或应用程序配置文件中使用部分。 |
| **allowLocation**   | 确定是否可以在元素中使用节 **\<location>** 。 使用以下值之一：<br><br>true<br>允许在元素中使用节 **\<location>** 。 这是默认设置。<br>**false**<br>不允许在元素中使用节 **\<location>** 。 |

## <a name="parent-elements"></a>父元素

|     | 说明 |
| --- | ----------- |
| [**\<configSections>** Element](configsections-element-for-configuration.md) | 包含配置节和命名空间声明。 |
| [**\<sectionGroup>** Element](sectiongroup-element-for-configsections.md) | 定义配置节的命名空间。 |

> [!NOTE]
> **\<section>** 元素是或中的子元素， **\<configSections>** **\<sectionGroup>** 而不是同时为两者。

## <a name="child-elements"></a>子元素

无

## <a name="remarks"></a>备注

声明配置节本质上定义了配置文件的新元素。 新元素包含配置节处理程序（即实现接口的类）的设置 <xref:System.Configuration.IConfigurationSectionHandler> 。 你定义的节的特性和子元素取决于用于读取设置的部分处理程序。

通过在*machine.config*文件中声明配置节处理程序，你可以使用该计算机上任何应用程序配置文件中的配置节，除非**allowDefinition**属性指定了其他内容。

## <a name="example"></a>示例

下面的示例演示如何定义配置节并定义该部分的设置：

```xml
<configuration>
  <configSections>
    <section name="sampleSection"
             type="System.Configuration.SingleTagSectionHandler"
             allowLocation="false" />
  </configSections>
  <sampleSection setting1="Value1"
                 setting2="value two"
                 setting3="third value" />
</configuration>
```

## <a name="configuration-file"></a>配置文件

此元素可用于应用程序配置文件、计算机配置文件（*machine.config*）和不在应用程序目录级别的*web.config 文件。*

## <a name="see-also"></a>另请参阅

- [.NET Framework 的配置文件架构](index.md)
