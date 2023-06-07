---
title: <loadFromRemoteSources> 元素
ms.date: 05/24/2018
helpviewer_keywords:
- loadFromRemoteSources element
- <loadFromRemoteSources> element
ms.assetid: 006d1280-2ac3-4db6-a984-a3d4e275046a
ms.openlocfilehash: 568c0c814dcc57be0f5be435bb7750c970ffec19
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91192446"
---
# <a name="loadfromremotesources-element"></a>\<loadFromRemoteSources> 元素

指定是否应在 .NET Framework 4 及更高版本中将从远程源加载的程序集授予完全信任。
  
> [!NOTE]
> 如果由于 Visual Studio 项目错误列表中的错误消息或生成错误而定向到本文，请参阅 [如何：在 Visual studio 中使用 Web 程序集](/previous-versions/visualstudio/visual-studio-2010/ee890038(v=vs.100))。  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<runtime>**](runtime-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;**\<loadFromRemoteSources>**  
  
## <a name="syntax"></a>语法  
  
```xml  
<loadFromRemoteSources
   enabled="true|false"/>  
```  
  
## <a name="attributes-and-elements"></a>特性和元素

 下列各节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  
  
|属性|描述|  
|---------------|-----------------|  
|`enabled`|必需的特性。<br /><br /> 指定是否应向从远程源加载的程序集授予完全信任。|  
  
## <a name="enabled-attribute"></a>enabled 属性  
  
|值|描述|  
|-----------|-----------------|  
|`false`|不要向远程源的应用程序授予完全信任。 这是默认设置。|  
|`true`|向远程源的应用程序授予完全信任。|  
  
### <a name="child-elements"></a>子元素  

 无。  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|`configuration`|公共语言运行时和 .NET Framework 应用程序所使用的每个配置文件中的根元素。|  
|`runtime`|包含有关运行时初始化选项的信息。|  
  
## <a name="remarks"></a>备注

在 .NET Framework 3.5 及更早版本中，如果从远程位置加载程序集，程序集中的代码将以部分信任的方式与依赖于它的区域的授权集一起运行。 例如，如果从网站加载程序集，则该程序集将加载到 Internet 区域并被授予 Internet 权限集。 换句话说，它是在 Internet 沙箱中执行的。

从 .NET Framework 4 开始，将禁用代码访问安全性 (CAS) 策略，并以完全信任方式加载程序集。 通常情况下，这会向用先前已进行沙盒处理的方法加载的程序集授予完全信任 <xref:System.Reflection.Assembly.LoadFrom%2A?displayProperty=nameWithType> 。 为了防止出现这种情况，默认情况下，将禁用在从远程源加载的程序集中运行代码的功能。 默认情况下，如果尝试加载远程程序集，则 <xref:System.IO.FileLoadException> 会引发并引发如下异常消息：

```text
System.IO.FileNotFoundException: Could not load file or assembly 'file:assem.dll' or one of its dependencies. Operation is not supported.
(Exception from HRESULT: 0x80131515)
File name: 'file:assem.dll' --->
System.NotSupportedException: An attempt was made to load an assembly from a network location which would have caused the assembly
to be sandboxed in previous versions of the .NET Framework. This release of the .NET Framework does not enable CAS policy by default,
so this load may be dangerous. If this load is not intended to sandbox the assembly, please enable the loadFromRemoteSources switch.
```

若要加载程序集并执行其代码，你必须：

- 为程序集显式创建沙盒 (参阅 [如何：在沙盒中运行部分受信任的代码](../../../misc/how-to-run-partially-trusted-code-in-a-sandbox.md)) 。

- 在完全信任环境中运行程序集的代码。 可以通过配置元素来执行此操作 `<loadFromRemoteSources>` 。 它使你能够指定在 .NET Framework 早期版本中以部分信任模式运行的程序集现在在 .NET Framework 4 及更高版本中以完全信任模式运行。

> [!IMPORTANT]
> 如果程序集不应在完全信任环境中运行，请不要设置此配置元素。 而是创建一个 <xref:System.AppDomain> 可在其中加载程序集的沙盒。

`enabled` `<loadFromRemoteSources>` 仅当禁用 (CAS) 的代码访问安全性时，元素的特性才有效。 默认情况下，CAS 策略在 .NET Framework 4 及更高版本中处于禁用状态。 如果将设置 `enabled` 为 `true` ，则会向远程程序集授予完全信任。

如果未 `enabled` 设置为 `true` ，则 <xref:System.IO.FileLoadException> 在以下条件之一下引发：

- 当前域的沙盒行为与 .NET Framework 3.5 中的行为不同。 这要求禁用 CAS 策略，当前域不会进行沙盒处理。

- 正在加载的程序集不是来自 `MyComputer` 区域。

设置 `<loadFromRemoteSources>` 元素可 `true` 防止引发此异常。 使用此方法，您可以指定不依赖公共语言运行时来对加载的程序集进行沙盒处理，以确保安全，并确保它们可以在完全信任的环境中执行。

## <a name="notes"></a>说明

- 在 .NET Framework 4.5 及更高版本中，本地网络共享上的程序集默认情况下以完全信任方式运行;您无需启用该 `<loadFromRemoteSources>` 元素。

- 如果应用程序是从 Web 复制而来，Windows 会将其标记为 Web 应用程序，即使它驻留在本地计算机上也不例外。 您可以通过更改其文件属性来更改该指定，或者可以使用 `<loadFromRemoteSources>` 元素授予程序集完全信任。 对于操作系统标记为从 Web 加载的本地程序集，也可以使用 <xref:System.Reflection.Assembly.UnsafeLoadFrom%2A> 方法进行加载。

- 你可能会 <xref:System.IO.FileLoadException> 在正在运行的应用程序中获取 Windows 虚拟 PC 应用程序。 当你尝试从宿主计算机上的链接文件夹中加载文件时，会发生这种情况。 当你尝试从 (终端服务) [远程桌面服务](/windows/win32/termserv/terminal-services-portal) 链接的文件夹中加载文件时，也会发生这种情况。 若要避免此异常，请将设置 `enabled` 为 `true` 。

## <a name="configuration-file"></a>配置文件

此元素通常用在应用程序配置文件中，但可根据上下文用于其他配置文件。 有关详细信息，请参阅 .NET Security 博客中的 [更隐式使用 CAS 策略： loadFromRemoteSources 一](/archive/blogs/shawnfa/more-implicit-uses-of-cas-policy-loadfromremotesources) 文。  

## <a name="example"></a>示例

下面的示例演示如何向从远程源加载的程序集授予完全信任。

```xml
<configuration>  
   <runtime>  
      <loadFromRemoteSources enabled="true"/>  
   </runtime>  
</configuration>  
```

## <a name="see-also"></a>请参阅

- [CAS 策略的更隐式使用： loadFromRemoteSources](/archive/blogs/shawnfa/more-implicit-uses-of-cas-policy-loadfromremotesources)
- [如何：运行沙盒中部分受信任的代码](../../../misc/how-to-run-partially-trusted-code-in-a-sandbox.md)
- [运行时设置架构](index.md)
- [配置文件架构](../index.md)
- <xref:System.Reflection.Assembly.LoadFrom%2A?displayProperty=nameWithType>
