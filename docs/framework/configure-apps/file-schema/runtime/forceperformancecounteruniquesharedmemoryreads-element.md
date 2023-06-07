---
title: <forcePerformanceCounterUniqueSharedMemoryReads> 元素
ms.date: 03/30/2017
helpviewer_keywords:
- forcePerformanceCounterUniqueSharedMemoryReads element
- <forcePerformanceCounterUniqueSharedMemoryReads> element
ms.assetid: 91149858-4810-4f65-9b48-468488172c9b
ms.openlocfilehash: 719448ba3de2aca0621fc17b9fadbdd3b8588f2e
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91178237"
---
# <a name="forceperformancecounteruniquesharedmemoryreads-element"></a>\<forcePerformanceCounterUniqueSharedMemoryReads> 元素

指定 PerfCounter.dll 是否使用 .NET Framework 版本 1.1 应用程序中的 CategoryOptions 注册表设置，以确定是否加载来自特定于类别的共享内存或全局内存的性能计数器数据。  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<runtime>**](runtime-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;**\<forcePerformanceCounterUniqueSharedMemoryReads>**  
  
## <a name="syntax"></a>语法  
  
```xml  
<forcePerformanceCounterUniqueSharedMemoryReads
enabled="true|false"/>  
```  
  
## <a name="attributes-and-elements"></a>特性和元素  

 下列各节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  
  
|属性|描述|  
|---------------|-----------------|  
|`enabled`|必需的特性。<br /><br /> 指示 PerfCounter.dll 是否使用 CategoryOptions 注册表设置来确定是否从特定于类别的共享内存或全局内存加载性能计数器数据。|  
  
## <a name="enabled-attribute"></a>enabled 特性  
  
|值|描述|  
|-----------|-----------------|  
|`false`|PerfCounter.dll 不使用 CategoryOptions 注册表设置，这是默认设置。|  
|`true`|PerfCounter.dll 使用 CategoryOptions 注册表设置。|  
  
### <a name="child-elements"></a>子元素  

 无。  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|`configuration`|公共语言运行时和 .NET Framework 应用程序所使用的每个配置文件中的根元素。|  
|`runtime`|包含有关程序集绑定和垃圾回收的信息。|  
  
## <a name="remarks"></a>备注  

 在 .NET Framework 4 之前的 .NET Framework 版本中，已加载到进程中加载的运行时的 PerfCounter.dll 版本对应。 如果计算机已安装 .NET Framework 版本1.1 和 .NET Framework 2.0，.NET Framework 1.1 应用程序会加载 PerfCounter.dll 的 .NET Framework 1.1 版本。 从 .NET Framework 4 开始，将加载 PerfCounter.dll 的最新安装版本。 这意味着，如果计算机上安装了 .NET Framework 4 版本，.NET Framework 1.1 应用程序将加载 PerfCounter.dll 的 .NET Framework 4 版本。  
  
 从 .NET Framework 4 开始，使用性能计数器时，PerfCounter.dll 检查每个提供程序的 CategoryOptions 注册表项，以确定是否应从特定于类别的共享内存或全局共享内存中读取。 .NET Framework 1.1 PerfCounter.dll 未读取该注册表项，因为它不知道特定于类别的共享内存;它始终从全局共享内存进行读取。  
  
 为了向后兼容，.NET Framework 4 PerfCounter.dll 在 .NET Framework 1.1 应用程序中运行时，不会检查 CategoryOptions 注册表项。 它只使用全局共享内存，就像 .NET Framework 1.1 PerfCounter.dll 一样。 不过，你可以通过启用元素指示 .NET Framework 4 PerfCounter.dll 检查注册表设置 `<forcePerformanceCounterUniqueSharedMemoryReads>` 。  
  
> [!NOTE]
> 启用 `<forcePerformanceCounterUniqueSharedMemoryReads>` 元素并不保证将使用特定于类别的共享内存。 设置为 `true` 仅导致 PerfCounter.dll 引用 CategoryOptions 注册表设置。 CategoryOptions 的默认设置是使用特定于类别的共享内存;但是，您可以更改 CategoryOptions 以指示应使用全局共享内存。  
  
 包含 CategoryOptions 设置的注册表项是 HKEY_LOCAL_MACHINE \System\CurrentControlSet\Services \\<类别名称 \> \Performance。 默认情况下，CategoryOptions 设置为3，指示 PerfCounter.dll 使用特定于类别的共享内存。 如果将 CategoryOptions 设置为0，则 PerfCounter.dll 使用全局共享内存。 仅当正在创建的实例的名称与重复使用的实例相同时，才会重新使用实例数据。 所有版本都可以写入该类别。 如果 CategoryOptions 设置为1，则使用全局共享内存，但如果类别名称与要重用的类别的长度相同，则可以重用实例数据。  
  
 设置0和1可能会导致内存泄漏和填充性能计数器内存。  
  
## <a name="example"></a>示例  

 下面的示例演示如何指定 PerfCounter.dll 应引用 CategoryOptions 注册表项，以确定它是否应使用特定于类别的共享内存。  
  
```xml  
<configuration>  
  <runtime>  
    <forcePerformanceCounterUniqueSharedMemoryReads enabled="true"/>  
  </runtime>  
</configuration>  
```  
  
## <a name="see-also"></a>请参阅

- [运行时设置架构](index.md)
- [配置文件架构](../index.md)
