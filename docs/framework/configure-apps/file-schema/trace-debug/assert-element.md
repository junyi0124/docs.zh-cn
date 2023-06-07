---
title: <assert> 元素
ms.date: 03/30/2017
f1_keywords:
- http://schemas.microsoft.com/.NetConfiguration/v2.0#configuration/system.diagnostics/assert
- http://schemas.microsoft.com/.NetConfiguration/v2.0#assert
helpviewer_keywords:
- <assert> element
- assert element
ms.assetid: ef4c3229-b151-4d85-8091-e6456af9b935
ms.openlocfilehash: eb29701912a45a484b1716195b449e8a97d1d4b5
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91149291"
---
# <a name="assert-element"></a>\<assert> 元素

指定调用 <xref:System.Diagnostics.Debug.Assert%2A?displayProperty=nameWithType> 方法时是否显示消息框；另外指定要写入消息的文件的名称。  

[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.diagnostics>**](system-diagnostics-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;**\<assert>**

## <a name="syntax"></a>语法  
  
```xml  
<assert assertuienabled="true|false" logfilename="file name"/>  
```  
  
## <a name="attributes-and-elements"></a>特性和元素  

 下列各节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  
  
|属性|描述|  
|---------------|-----------------|  
|`assertuienabled`|可选特性。<br /><br /> 指定在 **Debug 断言** 方法的计算结果为 **false**时是否显示消息框。|  
|`logfilename`|可选特性。<br /><br /> 指定如果 **debug.exe** 的计算结果为 **false**，则为要将消息写入到的文件的名称。|  
  
## <a name="assertuienabled-attribute"></a>assertuienabled 特性  
  
|值|描述|  
|-----------|-----------------|  
|`true`|显示消息框。 这是默认设置。|  
|`false`|不显示消息框。|  
  
### <a name="child-elements"></a>子元素  

 无。  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|`configuration`|公共语言运行时和 .NET Framework 应用程序所使用的每个配置文件中的根元素。|  
|`system.diagnostics`|指定用于收集、存储和路由消息的跟踪侦听器以及对跟踪开关设置的级别。|  
  
## <a name="remarks"></a>备注  

 元素中的两个特性 **\<assert>** 都是可选的。 您可以禁用消息框而不指定要向其中写入消息的文件，也可以指定在使消息框处于启用状态时要将消息写入到的文件。  
  
## <a name="example"></a>示例  

 下面的示例演示如何在调用 **Debug. 断言** 并将消息写入到时禁用显示消息框 `c:\log.txt` 。  
  
```xml  
<configuration>  
   <system.diagnostics>  
      <assert assertuienabled="false" logfilename="c:\log.txt"/>  
   </system.diagnostics>  
</configuration>  
```  
  
## <a name="see-also"></a>请参阅

- <xref:System.Diagnostics.Debug>
- [跟踪和调试设置架构](index.md)
