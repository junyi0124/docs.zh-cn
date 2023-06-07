---
title: <appSettings> 的 <add> 元素
ms.date: 05/01/2017
f1_keywords:
- http://schemas.microsoft.com/.NetConfiguration/v2.0#configuration/appSettings/add
helpviewer_keywords:
- add Element
- <add> Element
ms.assetid: 8734efdc-00f6-4a65-bba6-084c5bc65246
ms.openlocfilehash: 5c7de79ec626966e71d461dd3865b294a8979db2
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/06/2020
ms.locfileid: "77214811"
---
# <a name="add-element-for-appsettings"></a>\<appSettings> 的 \<add> 元素

添加自定义应用程序设置。

[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<appSettings>**](appsettings-element-for-configuration.md)\
&nbsp;&nbsp;&nbsp;&nbsp;**\<add>**

## <a name="syntax"></a>语法

```xml
<appSettings>
  <add key="Key of custom setting" value="Value of custom setting" />
</appSettings>
```

## <a name="attributes"></a>特性

|           | 说明 |
| --------- | ----------- |
| **键**   | 必需的特性。<br><br>指定要添加的密钥的名称。 |
| **value** | 必需的特性。<br><br>指定要添加的键的值。 |

## <a name="parent-element"></a>父元素

|     | 说明 |
| --- | ----------- |
| [**\<appSettings>**](appsettings-element-for-configuration.md) | 包含自定义应用程序设置，如文件路径、XML Web service URL 或应用程序的任何其他自定义配置信息。 |

## <a name="child-elements"></a>子元素

无

## <a name="example"></a>示例

下面的示例演示如何为应用程序名称添加自定义配置设置：

```xml
<appSettings>
  <add key="ApplicationName" value="MyApplication" />
</appSettings>
```

下面的示例使用 `<add>` 元素定义 ASP.NET 应用程序中的两个兼容性设置：

```xml
<appSettings>
  <add key="AppContext.SetSwitch:Switch.System.Globalization.NoAsyncCurrentCulture" value="true" />
  <add key="AppContext.SetSwitch:Switch.System.Uri.DontEnableStrictRFC3986ReservedCharacterSets" value="true" />
</appSettings>
```

## <a name="see-also"></a>另请参阅

- [.NET Framework 的配置文件架构](../index.md)
