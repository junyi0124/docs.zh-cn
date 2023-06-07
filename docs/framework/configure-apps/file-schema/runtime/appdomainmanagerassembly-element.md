---
title: <appDomainManagerAssembly> 元素
ms.date: 03/30/2017
helpviewer_keywords:
- <appDomainManagerAssembly> element
- appDomainManagerAssembly element
ms.assetid: c7c56e39-a700-44f5-b94e-411bfce339d9
ms.openlocfilehash: 1716b11106775bed2c0d6ccb62e8d5b032b6e8be
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91176131"
---
# <a name="appdomainmanagerassembly-element"></a>\<appDomainManagerAssembly> 元素

指定为过程中的默认应用程序域提供应用程序域管理器的程序集。  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<runtime>**](runtime-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;**\<appDomainManagerAssembly>**  
  
## <a name="syntax"></a>语法  
  
```xml  
<appDomainManagerAssembly
   value="assembly display name" />  
```  
  
## <a name="attributes-and-elements"></a>特性和元素  

 下列各节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  
  
|属性|描述|  
|---------------|-----------------|  
|`value`|必需的特性。 指定为进程中的默认应用程序域提供应用程序域管理器的程序集的显示名称。|  
  
### <a name="child-elements"></a>子元素  

 无。  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|`configuration`|公共语言运行时和 .NET Framework 应用程序所使用的每个配置文件中的根元素。|  
|`runtime`|包含有关程序集绑定和垃圾回收的信息。|  
  
## <a name="remarks"></a>备注  

 若要指定应用程序域管理器的类型，必须同时指定此元素和 [\<appDomainManagerType>](appdomainmanagertype-element.md) 元素。 如果未指定这些元素中的任何一个，则会忽略另一个。  
  
 在加载默认应用程序域时， <xref:System.TypeLoadException> 如果指定的程序集不存在，或者如果程序集不包含元素指定的类型，则会引发 [\<appDomainManagerType>](appdomainmanagertype-element.md) ; 并且进程无法启动。 如果找到程序集，但版本信息不匹配， <xref:System.IO.FileLoadException> 则会引发。  
  
 指定默认应用程序域的应用程序域管理器类型时，从默认应用程序域创建的其他应用程序域将继承应用程序域管理器类型。 使用 <xref:System.AppDomainSetup.AppDomainManagerType%2A?displayProperty=nameWithType> 和 <xref:System.AppDomainSetup.AppDomainManagerAssembly%2A?displayProperty=nameWithType> 属性为新的应用程序域指定不同的应用程序域管理器类型。  
  
 指定应用程序域管理器类型要求应用程序具有完全信任。  (例如，在桌面上运行的应用程序具有完全信任。 ) 如果应用程序不具有完全信任， <xref:System.TypeLoadException> 则会引发。  
  
 有关程序集显示名称的格式，请参见 <xref:System.Reflection.Assembly.FullName%2A?displayProperty=nameWithType> 属性。  
  
 此配置元素仅在 .NET Framework 4 及更高版本中可用。  
  
## <a name="example"></a>示例  

 下面的示例演示如何指定进程的默认应用程序域的应用程序域管理器是 `MyMgr` `AdMgrExample` 程序集中的类型。  
  
```xml  
<configuration>  
   <runtime>  
      <appDomainManagerType value="MyMgr" />  
      <appDomainManagerAssembly
         value="AdMgrExample, Version=1.0.0.0, Culture=neutral, PublicKeyToken=6856bccf150f00b3" />  
   </runtime>  
</configuration>  
```  
  
## <a name="see-also"></a>请参阅

- <xref:System.AppDomainSetup.AppDomainManagerType%2A?displayProperty=nameWithType>
- <xref:System.AppDomainSetup.AppDomainManagerAssembly%2A?displayProperty=nameWithType>
- [\<appDomainManagerType> 元素](appdomainmanagertype-element.md)
- [运行时设置架构](index.md)
- [配置文件架构](../index.md)
- [SetAppDomainManagerType 方法](../../../unmanaged-api/hosting/iclrcontrol-setappdomainmanagertype-method.md)
