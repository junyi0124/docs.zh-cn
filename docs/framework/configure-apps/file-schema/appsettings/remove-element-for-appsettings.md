---
title: <appSettings> 的 <remove> 元素
ms.date: 05/01/2017
f1_keywords:
- http://schemas.microsoft.com/.NetConfiguration/v2.0#configuration/appSettings/remove
helpviewer_keywords:
- remove Element
- <remove> Element
ms.assetid: 218c4464-e007-4539-803f-7c8b0a909fd8
ms.openlocfilehash: 83abbdbf0d3e4dfd16c0e8c649200c4ecc7329f7
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/06/2020
ms.locfileid: "77215495"
---
# <a name="remove-element-for-appsettings"></a>\<appSettings> 的 \<remove> 元素

删除自定义应用程序设置。

[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<appSettings>**](appsettings-element-for-configuration.md)\
&nbsp;&nbsp;&nbsp;&nbsp;**\<remove>**

## <a name="syntax"></a>语法

```xml
<appSettings>
  <remove key="Key of custom setting" />
</appSettings>
```

### <a name="attribute"></a>属性

|         | 说明 |
| ------- | ----------- |
| **键** | 必需的特性。<br><br>指定要删除的密钥的名称。 |

### <a name="parent-element"></a>父元素

|     | 说明 |
| --- | ----------- |
| [**\<appSettings>**](appsettings-element-for-configuration.md) | 包含自定义应用程序设置，如文件路径、XML Web service URL 或应用程序的任何其他自定义配置信息。 |

## <a name="child-elements"></a>子元素

无

## <a name="example"></a>示例

下面的示例演示如何删除的自定义配置设置 `ApplicationName` ：

```xml
<appSettings>
  <remove key="ApplicationName" />
</appSettings>
```

## <a name="see-also"></a>另请参阅

- [.NET Framework 的配置文件架构](../index.md)
