---
title: <filter>的的 <add> 的元素 <listeners><source>
ms.date: 03/30/2017
f1_keywords:
- http://schemas.microsoft.com/.NetConfiguration/v2.0#filter
- http://schemas.microsoft.com/.NetConfiguration/v2.0#configuration/system.diagnostics/sources/source/listeners/add/filter
helpviewer_keywords:
- initializeData attribute
- <filter> element for <add> for <listeners> for <source>
- filter element for <add> for <listeners> for <source>
ms.assetid: 15808b80-4579-4c25-b385-178cfdf154ba
ms.openlocfilehash: 2b83fd10da506202427aaeee454411822ff1ae5b
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91201676"
---
# <a name="filter-element-for-add-for-listeners-for-source"></a>\<filter>的的 \<add> 的元素 \<listeners>\<source>

将筛选器添加到跟踪源的 `Listeners` 集合中的侦听器。  

[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.diagnostics>**](system-diagnostics-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<sources>**](sources-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<source>**](source-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<listeners>**](listeners-element-for-source.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<add>**](add-element-for-listeners-for-source.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<filter>**

## <a name="syntax"></a>语法  
  
```xml  
<filter
  type="traceFilterClassName"
  initializeData="data" />  
```  
  
## <a name="attributes-and-elements"></a>特性和元素  

 下列各节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  
  
|属性|描述|  
|---------------|-----------------|  
|`type`|必需的特性。<br /><br /> 指定筛选器的类型，该类型应继承自 <xref:System.Diagnostics.TraceFilter> 类。 你可以使用类型的命名空间限定名称，该名称与类型的属性相对应 <xref:System.Type.FullName%2A> ，或者你可以使用包含程序集信息（与属性相对应）的完全限定的类型名称 <xref:System.Type.AssemblyQualifiedName%2A> 。 有关完全限定类型名称的信息，请参阅 [指定完全限定的类型名称](../../../reflection-and-codedom/specifying-fully-qualified-type-names.md)。|  
|`initializeData`|可选特性。<br /><br /> 传递给指定筛选器类的构造函数的字符串。|  
  
### <a name="child-elements"></a>子元素  

 无。  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|`configuration`|公共语言运行时和 .NET Framework 应用程序所使用的每个配置文件中的根元素。|  
|`system.diagnostics`|指定用于收集、存储和路由消息的跟踪侦听器以及对跟踪开关设置的级别。|  
|`sources`|包含用于启动跟踪消息的跟踪源。|  
|`source`|指定用于启动跟踪消息的跟踪源。|  
|`listeners`|包含收集、存储和路由消息的侦听器。 侦听器将跟踪输出定向到适当的目标。|  
|`add`|将侦听器添加到跟踪源的 `Listeners` 集合中。|  
  
## <a name="remarks"></a>备注  

 `<filter>`元素必须包含在 `<add>` 指定侦听器类型的跟踪源侦听器的元素中，而不只是包含中定义的侦听器的名称 [\<sharedListeners>](sharedlisteners-element.md) 。 如果侦听器是在中定义的 [\<sharedListeners>](sharedlisteners-element.md) ，则必须在该元素中定义该侦听器的筛选器。  
  
 此元素可在计算机配置文件中使用 ( # A0) 和应用程序配置文件。  
  
## <a name="example"></a>示例  

 下面的示例演示如何使用 `<filter>` 元素将筛选器添加到 `console` 跟踪源的集合中的侦听器，并将 `Listeners` `myTraceSource` 筛选器事件级别指定为 `Error` 。  
  
```xml  
<configuration>  
  <system.diagnostics>  
    <sources>  
      <source name="myTraceSource" switchName="SourceSwitch"
        switchType="System.Diagnostics.SourceSwitch"  >  
        <listeners>  
          <add name="console"
            type="System.Diagnostics.ConsoleTraceListener" >  
            <filter type="System.Diagnostics.EventTypeFilter"
              initializeData="Error" />  
          </add>  
          <remove name="Default" />  
        </listeners>  
      </source>  
    </sources>  
    <switches>  
      <add name="SourceSwitch" value="Warning" />  
    </switches>  
  </system.diagnostics>  
</configuration>  
```  
  
## <a name="see-also"></a>请参阅

- <xref:System.Diagnostics.TraceSource>
- <xref:System.Diagnostics.TraceListener>
- <xref:System.Diagnostics.TraceListener.Filter%2A?displayProperty=nameWithType>
- <xref:System.Diagnostics.TraceFilter>
- [跟踪和调试设置架构](index.md)
