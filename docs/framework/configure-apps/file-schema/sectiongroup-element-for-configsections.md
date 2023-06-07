---
title: <configSections> 的 <sectionGroup> 元素
ms.date: 05/01/2017
f1_keywords:
- http://schemas.microsoft.com/.NetConfiguration/v2.0#configuration/configSections/sectionGroup
helpviewer_keywords:
- sectionGroup Element
- <sectionGroup> Element
ms.assetid: 6c27f9e2-809c-4bc9-aca9-72f90360e7a3
ms.openlocfilehash: eb221027470fe6e485f8fcc4b939b71e4f219712
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/06/2020
ms.locfileid: "77215264"
---
# <a name="sectiongroup-element-for-configsections"></a>\<configSections> 的 \<sectionGroup> 元素

定义配置节的命名空间。

[**\<configuration>**](configuration-element.md)\
&nbsp;&nbsp;[**\<configSections>**](configsections-element-for-configuration.md)\
&nbsp;&nbsp;&nbsp;&nbsp;**\<sectionGroup>**

## <a name="syntax"></a>语法

```xml
<sectionGroup name="section group name">
  <!-- Configuration sections -->
</sectionGroup>
```

## <a name="attribute"></a>属性

|           | 说明 |
| --------- | ----------- |
| **name**  | 必需的特性。<br><br>指定正在定义的节组的名称。 |

## <a name="parent-element"></a>父元素

|     | 说明 |
| --- | ----------- |
| [**\<configSections>** Element](configsections-element-for-configuration.md) | 包含配置节和命名空间声明。 |

## <a name="child-elements"></a>子元素

|     | 说明 |
| --- | ----------- |
| [**\<section>**](section-element.md) | 包含配置节声明。 |

## <a name="remarks"></a>注解

声明节组将为配置节创建容器标记，并确保与其他人定义的配置节之间没有命名冲突。 可以在彼此 **\<sectionGroup>** 之间嵌套元素。

## <a name="example"></a>示例

下面的示例演示如何声明一个节组，并在节组中声明部分：

```xml
<configuration>
  <configSections>
    <sectionGroup name="mySectionGroup">
      <section name="mySection"
               type="System.Configuration.NameValueSectionHandler,System" />
    </sectionGroup>
  </configSections>
  <mySectionGroup>
    <mySection>
      <add key="key1" value="value1" />
    </mySection>
  </mySectionGroup>
</configuration>
```

## <a name="configuration-file"></a>配置文件

此元素可用于应用程序配置文件、计算机配置文件（*machine.config*）和不在应用程序目录级别的*web.config 文件。*

## <a name="see-also"></a>另请参阅

- [.NET Framework 的配置文件架构](index.md)
