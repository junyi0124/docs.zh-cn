---
title: <disableFusionUpdatesFromADManager> 元素
ms.date: 03/30/2017
helpviewer_keywords:
- disableFusionUpdatesFromADManager element
- <disableFusionUpdatesFromADManager> element
ms.assetid: 58d2866c-37bd-4ffa-abaf-ff35926a2939
ms.openlocfilehash: c3971379b358ae16fc463df2b8d6288cf8881391
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91205030"
---
# <a name="disablefusionupdatesfromadmanager-element"></a>\<disableFusionUpdatesFromADManager> 元素

指定是否禁用允许运行时主机为应用程序域重写配置设置的默认行为。  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<runtime>**](runtime-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;**\<disableFusionUpdatesFromADManager>**  
  
## <a name="syntax"></a>语法  
  
```xml  
<disableFusionUpdatesFromADManager enabled="0|1"/>  
```  
  
## <a name="attributes-and-elements"></a>特性和元素  

 下列各节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  
  
|属性|说明|  
|---------------|-----------------|  
|enabled|必需的特性。<br /><br /> 指定是否禁用用于重写合成设置的默认功能。|  
  
## <a name="enabled-attribute"></a>enabled 特性  
  
|值|描述|  
|-----------|-----------------|  
|0|不要禁用替代合成设置的功能。 这是默认行为，从 .NET Framework 4 开始。|  
|1|禁用重写合成设置的功能。 这会恢复到 .NET Framework 早期版本的行为。|  
  
### <a name="child-elements"></a>子元素  

 无。  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|`configuration`|公共语言运行时和 .NET Framework 应用程序所使用的每个配置文件中的根元素。|  
|`runtime`|包含有关程序集绑定和垃圾回收的信息。|  
  
## <a name="remarks"></a>备注  

 从 .NET Framework 4 开始，默认行为是允许 <xref:System.AppDomainManager> 对象 <xref:System.AppDomainSetup.ConfigurationFile%2A> <xref:System.AppDomainSetup.SetConfigurationBytes%2A> <xref:System.AppDomainSetup> <xref:System.AppDomainManager.InitializeNewDomain%2A?displayProperty=nameWithType> 在的子类中使用传递给方法实现的属性或对象的方法来重写配置设置 <xref:System.AppDomainManager> 。 对于默认应用程序域，所更改的设置将覆盖由应用程序配置文件指定的设置。 对于其他应用程序域，它们会重写传递给或方法的配置设置 <xref:System.AppDomainManager.CreateDomain%2A?displayProperty=nameWithType> <xref:System.AppDomain.CreateDomain%2A?displayProperty=nameWithType> 。  
  
 可以传递新的配置信息，也可以将 null (传递 `Nothing` 到 Visual Basic) ，以消除传入的配置信息。  
  
 不要将配置信息传递给 <xref:System.AppDomainSetup.ConfigurationFile%2A> 属性和 <xref:System.AppDomainSetup.SetConfigurationBytes%2A> 方法。 如果将配置信息传递到这两个文件，则会忽略传递给属性的信息 <xref:System.AppDomainSetup.ConfigurationFile%2A> ，因为 <xref:System.AppDomainSetup.SetConfigurationBytes%2A> 方法会重写应用程序配置文件中的配置信息。 如果使用 <xref:System.AppDomainSetup.ConfigurationFile%2A> 属性，则可以将 `Nothing` Visual Basic) 中的 null (传递给方法， <xref:System.AppDomainSetup.SetConfigurationBytes%2A> 以消除对或方法的调用中指定的任何配置字节 <xref:System.AppDomainManager.CreateDomain%2A?displayProperty=nameWithType> <xref:System.AppDomain.CreateDomain%2A?displayProperty=nameWithType> 。  
  
 除了配置信息之外，你还可以在 <xref:System.AppDomainSetup> 传递给方法的实现的对象上更改以下设置 <xref:System.AppDomainManager.InitializeNewDomain%2A?displayProperty=nameWithType> ： <xref:System.AppDomainSetup.ApplicationBase%2A> 、 <xref:System.AppDomainSetup.ApplicationName%2A> 、 <xref:System.AppDomainSetup.CachePath%2A> 、 <xref:System.AppDomainSetup.DisallowApplicationBaseProbing%2A> 、、 <xref:System.AppDomainSetup.DisallowBindingRedirects%2A> <xref:System.AppDomainSetup.DisallowCodeDownload%2A> <xref:System.AppDomainSetup.DisallowPublisherPolicy%2A> <xref:System.AppDomainSetup.DynamicBase%2A> <xref:System.AppDomainSetup.LoaderOptimization%2A> <xref:System.AppDomainSetup.PrivateBinPath%2A> <xref:System.AppDomainSetup.PrivateBinPathProbe%2A> <xref:System.AppDomainSetup.ShadowCopyDirectories%2A> <xref:System.AppDomainSetup.ShadowCopyFiles%2A> 、、、、、、和。  
  
 作为使用元素的替代方法 `<disableFusionUpdatesFromADManager>` ，你可以通过创建注册表设置或通过设置环境变量来禁用默认行为。 在注册表中，创建一个名为或的 DWORD 值 `COMPLUS_disableFusionUpdatesFromADManager` `HKCU\Software\Microsoft\.NETFramework` `HKLM\Software\Microsoft\.NETFramework` ，并将该值设置为1。 在命令行中，将环境变量设置 `COMPLUS_disableFusionUpdatesFromADManager` 为1。  
  
## <a name="example"></a>示例  

 下面的示例演示如何使用元素禁用重写合成设置的功能 `<disableFusionUpdatesFromADManager>` 。  
  
```xml  
<configuration>  
   <runtime>  
      <disableFusionUpdatesFromADManager enabled="1" />  
   </runtime>  
</configuration>  
```  
  
## <a name="see-also"></a>请参阅

- [运行时设置架构](index.md)
- [配置文件架构](../index.md)
- [运行时如何定位程序集](../../../deployment/how-the-runtime-locates-assemblies.md)
