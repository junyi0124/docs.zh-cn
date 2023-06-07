---
title: <serviceActivations>
ms.date: 03/30/2017
ms.assetid: 97e665b6-1c51-410b-928a-9bb42c954ddb
ms.openlocfilehash: 64ae0bfd90ae941fc78515c7936c998201c87485
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/06/2020
ms.locfileid: "70855135"
---
# \<serviceActivations>

一个配置元素，允许您添加用于定义映射到 Windows Communication Foundation （WCF）服务类型的虚拟服务激活设置的设置。 使用此配置元素可以在不使用 .svc 文件的情况下激活承载在 WAS/IIS 中的服务。

[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<serviceHostingEnvironment>**](servicehostingenvironment.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<serviceActivations>**  

## <a name="syntax"></a>语法

```xml
<serviceHostingEnvironment>
  <serviceActivations>
    <add factory="String"
         service="String" />
  </serviceActivations>
</serviceHostingEnvironment>
```

## <a name="attributes-and-elements"></a>特性和元素

下列各节描述了特性、子元素和父元素。

### <a name="attributes"></a>特性

无。

### <a name="child-elements"></a>子元素

|元素|说明|
|-------------|-----------------|
|[\<add>](add-of-serviceactivations.md)|添加一个指定激活服务应用程序的配置元素。|

### <a name="parent-elements"></a>父元素

|元素|说明|
|-------------|-----------------|
|[\<serviceHostingEnvironment>](servicehostingenvironment.md)|定义服务承载环境要为特定传输实例化的类型。|

## <a name="remarks"></a>注解

下面的示例演示如何在 web.config 文件中配置激活设置。

```xml
<configuration>
  <system.serviceModel>
    <serviceHostingEnvironment>
      <serviceActivations>
        <add service="GreetingService" />
      </serviceActivations>
    </serviceHostingEnvironment>
  </system.serviceModel>
</configuration>
```

使用此配置，您可以在不使用 .svc 文件的情况下激活 GreetingService。

请注意，`<serviceHostingEnvironment>` 是应用程序级配置。 必须将包含此配置的 `web.config` 放置到虚拟应用程序的根目录下。 此外， `serviceHostingEnvironment` 是 machineToApplication 可继承部分。 如果在计算机的根目录中注册了一个服务，应用程序中的每个服务都将继承此服务。

基于配置的激活支持通过 http 协议和非 http 协议进行激活。 它需要 relativeAddress 中的扩展，如 xoml 或 .xamlx。 您可以将自己的扩展名映射到已知的 buildProviders，然后就可以通过任意扩展名激活服务。 如果发生冲突，`<serviceActivations>` 节将重写 .svc 注册。

## <a name="see-also"></a>另请参阅

- <xref:System.ServiceModel.Configuration.ServiceActivationElementCollection>
- <xref:System.ServiceModel.Configuration.ServiceHostingEnvironmentSection>
- <xref:System.ServiceModel.ServiceHostingEnvironment>
