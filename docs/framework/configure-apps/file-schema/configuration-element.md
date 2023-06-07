---
title: <configuration> 元素
ms.date: 03/30/2017
f1_keywords:
- http://schemas.microsoft.com/.NetConfiguration/v2.0#configuration
helpviewer_keywords:
- <configuration> element
- configuration element
- container tags, <configuration> element
ms.assetid: 2ec1c9dc-2e5c-4ef0-9958-81670ab88449
ms.openlocfilehash: 8f79981a55d0bc9b1cd522e45f5606fda102c72c
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90544683"
---
# <a name="configuration-element"></a>\<configuration> 元素

公共语言运行时和 .NET Framework 应用程序所使用的每个配置文件中的根元素。

**\<configuration>**

## <a name="syntax"></a>语法

```xml
<configuration>
  <!-- Configuration settings -->
</configuration>
```

## <a name="attributes"></a>特性

无

## <a name="parent-element"></a>父元素

无

## <a name="child-elements"></a>子元素

|     | 说明 |
| --- | ----------- |
| [**\<assemblyBinding>**](assemblybinding-element-for-configuration.md) | 指定配置级的程序集绑定策略。|
| [**\<startup>** 设置架构](./startup/index.md) | 启动设置架构中的所有元素。 |
| [**\<runtime>** 设置架构](./runtime/index.md) | 运行时设置架构中的所有元素。 |
| [**\<system.runtime.remoting>** 设置架构](/previous-versions/dotnet/netframework-4.0/z415cf9a(v=vs.100)) | 远程处理设置架构中的所有元素。 |
| [**\<system.Net>** 设置架构](./network/index.md) | 网络设置架构中的所有元素。 |
| [**\<cryptographySettings>** 设置架构](./cryptography/index.md) | 加密设置架构中的所有元素。 |
| [**\<configuration>** 节架构](configuration-sections-schema.md) | 配置节设置架构中的所有元素。 |
| [跟踪和调试设置架构](./trace-debug/index.md) | 跟踪和调试设置架构中的所有元素。 |
| [ASP.NET 配置设置架构](/previous-versions/dotnet/netframework-4.0/b5ysx397(v=vs.100)) | ASP.NET 配置架构中的所有元素，包括用于配置 ASP.NET 网站和应用程序的元素。 用于 *Web.config* 文件中。 |
| [**\<webServices>** 设置架构](/previous-versions/dotnet/netframework-4.0/cctwteet(v=vs.100)) | Web 服务设置架构中的所有元素。 |
| [Web 设置架构](./web/index.md) | Web 设置架构中的所有元素，包括用于配置 ASP.NET 如何与主机应用程序（如 IIS）一起工作的元素。 用于 *aspnet.config* 文件中。 |

## <a name="remarks"></a>备注

每个配置文件必须仅包含一个 **\<configuration>** 元素。

## <a name="see-also"></a>请参阅

- [.NET Framework 的配置文件架构](index.md)
