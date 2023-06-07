---
title: <bindings>
ms.date: 01/22/2018
ms.assetid: b62cd369-5409-4030-8490-9759a462dd3a
ms.openlocfilehash: fe8f620668e35183890b8bba1f254a74c962f8d3
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/06/2020
ms.locfileid: "74139659"
---
# \<bindings>

您可以使用 `bindings` 元素来配置 Windows Communication Foundation （WCF）的标准绑定和自定义绑定的集合。 每一项都是一个可由其唯一 `binding` 进行标识的 `name` 元素。 服务通过用 `name` 与绑定进行链接来使用绑定。 从 .NET Framework 4 开始，绑定和行为不需要具有名称。 有关默认配置和无值绑定和行为的详细信息，请参阅[WCF 服务的](../../../wcf/samples/simplified-configuration-for-wcf-services.md)[简化配置](../../../wcf/simplified-configuration.md)和简化配置。

## <a name="system-provided-bindings"></a>系统提供的绑定

系统提供的绑定可以消除 WCF 消息堆栈的复杂性。 使用系统提供的绑定的应用程序不需要对堆栈的完全控制权限。 在每个系统提供的绑定上公开的属性最适合绑定所针对的使用方案。

每个系统提供的绑定的配置节可以定义用于配置此绑定的一些配置。 每个配置均由唯一的名称进行标识。

无法将元素或特性添加到系统提供的绑定。 为此，应实现自[定义](#custom-bindings)绑定部分中所述的自定义绑定。 可以定义一个自定义绑定，该绑定将模拟系统提供的绑定，并添加用户应用程序要控制的一些设置。  

有关系统提供的绑定的列表，请参阅[系统提供的绑定](../../../wcf/system-provided-bindings.md)。

## <a name="custom-bindings"></a>自定义绑定

自定义绑定提供了对 WCF 消息堆栈的完全控制。 单个绑定按照堆栈元素在堆栈上出现的顺序来指定它们的配置元素，从而定义消息堆栈。 每个元素定义并配置堆栈的一个元素。 在每个自定义绑定中，必须有且只能有一个 `transport` 元素。 如果没有该元素，消息堆栈将是不完整的。

元素在堆栈中出现的顺序非常重要，因为在将操作应用于消息时会采用该顺序。 所需的堆栈元素顺序如下：  

1. 事务（可选）  

2. 可靠消息传送（可选）  

3. 安全（可选）  

4. 编码器  

5. Transport  

 自定义绑定由其 `name` 特性来标识。 有关自定义绑定的详细信息，请参阅[自定义绑定](../../../wcf/extending/custom-bindings.md)。

## <a name="see-also"></a>另请参阅

- <xref:System.ServiceModel.Configuration.BindingsSection?displayProperty=nameWithType>
- <xref:System.ServiceModel.Channels.Binding?displayProperty=nameWithType>
- <xref:System.ServiceModel.Channels.BindingElement?displayProperty=nameWithType>
- [绑定](../../../wcf/bindings.md)
- [自定义绑定](../../../wcf/extending/custom-bindings.md)
- [\<customBinding>](custombinding.md)
