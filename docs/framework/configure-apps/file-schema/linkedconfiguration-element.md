---
title: <linkedConfiguration> 元素
ms.date: 03/30/2017
f1_keywords:
- http://schemas.microsoft.com/.NetConfiguration/v2.0#configuration/assemblyBinding/linkedConfiguration
- http://schemas.microsoft.com/.NetConfiguration/v2.0#linkedConfiguration
helpviewer_keywords:
- configuration files [.NET Framework],linked configuration files
- <linkedConfiguration> element
- including configuration files
- linked configuration files
- linkedConfiguration Element
ms.assetid: 8eb34f3b-427e-4288-a7ff-c73f489deb45
ms.openlocfilehash: 14ee2275ecf690ab16ffaabd71fbbe7e1a4897bc
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/06/2020
ms.locfileid: "74087966"
---
# <a name="linkedconfiguration-element"></a>\<linkedConfiguration> 元素

指定要包含的配置文件。

[**\<configuration>**](configuration-element.md)\
&nbsp;&nbsp;[**\<assemblyBinding>**](assemblybinding-element-for-configuration.md)\
&nbsp;&nbsp;&nbsp;&nbsp;**\<linkedConfiguration>**

## <a name="syntax"></a>语法

```xml
<linkedConfiguration href="URL of linked configuration file" />
```

## <a name="attribute"></a>属性

|           | 说明 |
| --------- | ----------- |
| **href**  | 必需的特性。<br><br>要包含的配置文件的 URL。 **Href**特性支持的唯一格式为 `file://` 。 支持本地文件和 UNC 文件。 |

## <a name="parent-element"></a>父元素

|     | 说明 |
| --- | ----------- |
| [**\<assemblyBinding>** Element](assemblybinding-element-for-configuration.md) | 指定配置级的程序集绑定策略。 |

## <a name="child-elements"></a>子元素

无

## <a name="remarks"></a>备注

**\<linkedConfiguration>** 元素简化了组件程序集的服务。 如果一个或多个应用程序使用的程序集具有驻留在众所周知位置的配置文件，则使用该程序集的应用程序的配置文件可以使用 **\<linkedConfiguration>** 元素来包含程序集配置文件，而不是直接包含配置信息。 处理组件程序集时，更新公共配置文件会为使用该程序集的所有应用程序提供更新的配置信息。

> [!NOTE]
> **\<linkedConfiguration>** 具有 Windows 并行清单的应用程序不支持该元素。

以下规则控制链接配置文件的使用：

- 包含的配置文件中的设置只会影响加载程序绑定策略并仅由加载程序使用。 包含的配置文件可以包含绑定策略以外的设置，但这些设置不会产生任何影响。

- 特性支持的唯一格式 `href` 为 `file://` 。 支持本地文件和 UNC 文件。

- 每个配置文件的链接配置数没有限制。

- 所有链接的配置文件合并到一个文件中，这与 `#include` c/c + + 中指令的行为类似。

- **\<linkedConfiguration>** 仅允许在应用程序配置文件中使用元素; 它在*machine.config*中被忽略。

- 检测和终止循环引用。 也就是说，如果 **\<linkedConfiguration>** 一系列配置文件的元素形成循环，则检测并停止循环。

## <a name="example"></a>示例

下面的示例演示如何包括本地硬盘中的配置文件：

```xml
<configuration>
  <assemblyBinding xmlns="urn:schemas-microsoft-com:asm.v1">
    <linkedConfiguration href="file://c:\Program Files\Contoso\sharedConfig.xml"/>
  </assemblyBinding>
</configuration>
```

## <a name="see-also"></a>另请参阅

- [**\<assemblyBinding>** Element](assemblybinding-element-for-configuration.md)
- [.NET Framework 的配置文件架构](index.md)
