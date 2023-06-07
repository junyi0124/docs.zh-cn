---
title: '@ServiceHost'
ms.date: 03/30/2017
ms.assetid: 96ba6967-00f2-422f-9aa7-15de4d33ebf3
ms.openlocfilehash: cb425d9f4dadd97e93946a2b4cd9d059ea8504ce
ms.sourcegitcommit: 0edbeb66d71b8df10fcb374cfca4d731b58ccdb2
ms.contentlocale: zh-CN
ms.lasthandoff: 07/07/2020
ms.locfileid: "86051358"
---
# <a name="servicehost"></a>\@服务主机

将用于生成服务主机的工厂与要承载的服务以及访问或编译 .svc 文件中提供的宿主代码所需的其他编程方面相关联。

## <a name="syntax"></a>语法

```aspx-csharp
<% @ServiceHost
Service = "Service, ServiceNamespace"
Factory = "Factory, FactoryNamespace"
Debug = "Debug"
Language = "Language"
CodeBehind = "CodeBehind"
%>
```

## <a name="attributes"></a>特性

### <a name="service"></a>服务

承载的服务的 CLR 类型名称。 此名称应为实现一个或多个服务协定的类型的限定名称。

### <a name="factory"></a>Factory

用于实例化服务主机的服务主机工厂的 CLR 类型名称。 此属性是可选的。 如果未指定，则使用默认的 <xref:System.ServiceModel.Activation.ServiceHostFactory>，这将返回 <xref:System.ServiceModel.ServiceHost> 的实例。

### <a name="debug"></a>调试

指示是否应用调试符号编译 Windows Communication Foundation （WCF）服务。 `true`如果应用调试符号编译 WCF 服务，则为; 否则为。否则为 `false` 。

### <a name="language"></a>语言

指定编译文件 (.svc) 中的所有内联代码时使用的语言。 值可以表示任意。NET 支持的语言，包括 `C#` 、 `VB` 和 `JS` ，分别引用 c #、Visual Basic 和 JScript .net。 此属性是可选的。

### <a name="codebehind"></a>CodeBehind

指定实现 XML Web service 的源文件，当实现 XML Web service 的类未驻留在同一文件中，且尚未编译成程序集并放置在*\bin*目录中时，则为。

## <a name="remarks"></a>注解

<xref:System.ServiceModel.ServiceHost>用于承载服务的是 Windows Communication Foundation （WCF）编程模型中的一个扩展点。 由于 <xref:System.ServiceModel.ServiceHost> 可能属于宿主环境不应直接实例化的多态类型，因此使用工厂模式对其进行实例化。

默认实现使用 <xref:System.ServiceModel.Activation.ServiceHostFactory> 创建 <xref:System.ServiceModel.ServiceHost> 的实例。 但你可以通过在指令中指定工厂实现的 CLR 类型名称来提供自己的工厂（一个返回派生主机的工厂） `@ServiceHost` 。

若要使用自定义服务主机工厂而不是默认工厂，只需在指令中提供类型名称，如下所示 `@ServiceHost` 。

```xml
<% @ServiceHost Factory="DerivedFactory" Service="MyService" %>
```

尽可能简化工厂实现。 如果具有大量的自定义逻辑，则应将这些逻辑放入宿主而不是工厂内，这样可以获得更好的代码重用性。

例如，若要为启用支持 AJAX 的终结点 `MyService` ，请 <xref:System.ServiceModel.Activation.WebScriptServiceHostFactory> `Factory` 在指令中指定属性值（而不是默认值 <xref:System.ServiceModel.Activation.ServiceHostFactory> ）， `@ServiceHost` 如以下示例中所示：

```aspx-csharp
<% @ServiceHost
Service="MyService"
Language="C#"
Debug="true"
Factory="WebScriptServiceHostFactory"
%>
```

## <a name="see-also"></a>请参阅

- [自定义服务主机](../../../wcf/samples/custom-service-host.md)
