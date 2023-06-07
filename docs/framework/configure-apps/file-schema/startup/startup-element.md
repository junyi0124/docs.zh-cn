---
title: <startup> 元素
ms.date: 03/30/2017
f1_keywords:
- http://schemas.microsoft.com/.NetConfiguration/v2.0#configuration/startup
- http://schemas.microsoft.com/.NetConfiguration/v2.0#startup
helpviewer_keywords:
- container tags, <startup> element
- <startup> element
- startup element
ms.assetid: 536acfd8-f827-452f-838a-e14fa3b87621
ms.openlocfilehash: cd91abb288c1cfb281f17f2fce95d4956908468f
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90550841"
---
# <a name="startup-element"></a>\<startup> 元素

指定公共语言运行时启动信息。

[**\<configuration>**](../configuration-element.md)  
&nbsp;&nbsp;**\<startup>**  

## <a name="syntax"></a>语法

```xml
<startup useLegacyV2RuntimeActivationPolicy="true|false" >
</startup>
```

## <a name="attributes-and-elements"></a>特性和元素

 下列各节描述了特性、子元素和父元素。

### <a name="attributes"></a>特性

|属性|描述|
|---------------|-----------------|
|`useLegacyV2RuntimeActivationPolicy`|可选特性。<br /><br /> 指定是启用 .NET Framework 2.0 运行时激活策略还是使用 .NET Framework 4 激活策略。|

## <a name="uselegacyv2runtimeactivationpolicy-attribute"></a>useLegacyV2RuntimeActivationPolicy 特性

|“值”|说明|
|-----------|-----------------|
|`true`|为所选运行时启用 .NET Framework 2.0 运行时激活策略，这是为了将旧的运行时激活 (技术（如 [CorBindToRuntimeEx 函数](../../../unmanaged-api/hosting/corbindtoruntimeex-function.md) ）) 到从配置文件中选择的运行时，而不是在 CLR 版本2.0 上覆盖它们。 因此，如果从配置文件中选择了 CLR 版本4或更高版本，则将用所选的 CLR 版本加载随 .NET Framework 早期版本创建的混合模式程序集。 设置此值可防止 CLR 版本1.1 或 CLR 版本2.0 加载到同一进程中，从而有效禁用进程内并行功能。|
|`false`|使用 .NET Framework 4 和更高版本的默认激活策略，这是为了允许旧的运行时激活技术将 CLR 版本1.1 或2.0 加载到进程中。 设置此值可防止混合模式程序集加载到 .NET Framework 4 或更高版本中，除非它们是用 .NET Framework 4 或更高版本生成的。 这是默认值。|

### <a name="child-elements"></a>子元素

|元素|说明|
|-------------|-----------------|
|[\<requiredRuntime>](requiredruntime-element.md)|指定应用程序仅支持 1.0 版本的公共语言运行时。 使用运行时版本1.1 或更高版本生成的应用程序应使用 **\<supportedRuntime>** 元素。|
|[\<supportedRuntime>](supportedruntime-element.md)|指定应用程序支持的公共语言运行时版本。|

### <a name="parent-elements"></a>父元素

|元素|说明|
|-------------|-----------------|
|`configuration`|公共语言运行时和 .NET Framework 应用程序所使用的每个配置文件中的根元素。|

## <a name="remarks"></a>备注

 **\<supportedRuntime>** 元素应由使用版本1.1 或更高版本的运行时生成的所有应用程序使用。 构建为仅支持1.0 版运行时的应用程序必须使用 **\<requiredRuntime>** 元素。

 在 Microsoft Internet Explorer 中托管的应用程序的启动代码将忽略该 **\<startup>** 元素及其子元素。

## <a name="the-uselegacyv2runtimeactivationpolicy-attribute"></a>UseLegacyV2RuntimeActivationPolicy 特性

 如果你的应用程序使用旧的激活路径（如 [CorBindToRuntimeEx 函数](../../../unmanaged-api/hosting/corbindtoruntimeex-function.md)），并且你希望这些路径激活 CLR 版本4（而不是早期版本），或者如果你的应用程序是 .NET Framework 使用 .NET Framework 早期版本生成的混合模式程序集生成的，则此属性很有用。 在这些情况下，请将属性设置为 `true` 。

> [!NOTE]
> 设置属性可 `true` 防止 clr 版本1.1 或 clr 版本2.0 加载到同一进程中，有效禁用进程内并行功能 (参阅 [COM 互操作) 的并行执行](/previous-versions/dotnet/netframework-4.0/8t8td04t(v=vs.100)) 。

## <a name="example"></a>示例

 下面的示例演示如何在配置文件中指定运行时版本。

```xml
<!-- When used with version 1.0 of the .NET Framework runtime -->
<configuration>
   <startup>
      <requiredRuntime version="v1.0.3705" safemode="true"/>
   </startup>
</configuration>
<!-- When used with version 1.1 (or later) of the runtime -->
<configuration>
   <startup>
      <supportedRuntime version="v1.1.4322"/>
      <supportedRuntime version="v1.0.3705"/>
   </startup>
</configuration>
```

## <a name="see-also"></a>请参阅

- [启动设置架构](index.md)
- [配置文件架构](../index.md)
- [如何：将应用配置为支持 .NET Framework 4 或更高版本](../../../migration-guide/how-to-configure-an-app-to-support-net-framework-4-or-4-5.md)
- [COM 互操作的并行执行](/previous-versions/dotnet/netframework-4.0/8t8td04t(v=vs.100))
- [进程内并行执行](../../../deployment/in-process-side-by-side-execution.md)
