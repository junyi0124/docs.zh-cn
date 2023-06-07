---
title: 选择筛选器
ms.date: 03/30/2017
ms.assetid: 67ab5af9-b9d9-4300-b3b1-41abb5a1fd10
ms.openlocfilehash: f783b6416f2330d272c4c756a3ca0cd332f3c6e7
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96276095"
---
# <a name="choosing-a-filter"></a>选择筛选器

配置路由服务时，选择正确的消息筛选器并将它们配置为允许您针对接收的消息进行完全匹配非常重要。 如果所选筛选器筛选出的匹配项太广或者配置不当，则消息会错误地进行路由。 如果筛选器的筛选范围太窄，则某些消息可能没有任何可用的有效路由。

## <a name="filter-types"></a>筛选器类型

选择由路由服务使用的筛选器时，了解每个筛选器的工作方式以及哪些信息可以作为传入消息的一部分非常重要。 例如，如果所有消息都是通过同一终结点接收的，则 Address 和 EndpointName 筛选器可能没有用，因为所有消息都与这两个筛选器匹配。

### <a name="action"></a>操作

Action 筛选器检查 <xref:System.ServiceModel.Channels.MessageHeaders.Action%2A> 属性。 如果消息中 Action 标头的内容与筛选器配置中指定的筛选器数据值相匹配，此筛选器将返回 `true`。 下面的示例定义一个 `FilterElement` ，它使用操作筛选器来匹配包含值为的操作标头的消息 `http://namespace/contract/operation/` 。

```xml
<filter name="action1" filterType="Action" filterData="http://namespace/contract/operation/" />
```

```csharp
ActionMessageFilter action1 = new ActionMessageFilter(new string[] { "http://namespace/contract/operation" });
```

如果路由的消息包含唯一 Action 标头，则应使用此筛选器。

### <a name="endpointaddress"></a>EndpointAddress

EndpointAddress 筛选器检查接收消息的 EndpointAddress。 如果消息到达的地址与筛选器配置中指定的筛选器地址完全匹配，此筛选器将返回 `true`。 下面的示例定义一个 `FilterElement` ，它使用地址筛选器来匹配发送到 "http:///vdir/s.svc/b" 的任何消息 \<hostname> 。

```xml
<filter name="address1" filterType="EndpointAddress" filterData="http://host/vdir/s.svc/b" />
```

```csharp
EndpointAddressMessageFilter address1 = new EndpointAddressMessageFilter(new EndpointAddress("http://host/vdir/s.svc/b"), false);
```

> [!NOTE]
> 需要特别注意的是，根据客户端使用的是完全限定域名、NetBIOS 名称、IP 地址还是其他名称，地址的主机名部分可能有所不同。 由于不同值可以引用同一主机，因此在执行匹配时，此比较的默认行为是不使用地址的主机名部分。
>
> 可以对此行为进行修改，以便在以编程方式配置路由服务时允许比较操作计算主机名。

如果将传入消息发送到唯一地址，则应使用此筛选器。

### <a name="endpointaddressprefix"></a>EndpointAddressPrefix

EndpointAddressPrefix 筛选器与 EndpointAddress 筛选器类似。 EndpointAddressPrefix 筛选器检查接收消息的 EndpointAddress。 但是，EndpointAddressPrefix 筛选器在匹配以筛选器配置中指定的值开头的地址时用作通配符。 下面的示例定义一个 `FilterElement` ，它使用 EndpointAddressPrefix 筛选器来匹配发送到的所有消息 `http://<hostname>/vdir*` 。

```xml
<filter name="prefix1" filterType="EndpointAddressPrefix" filterData="http://host/vdir" />
```

```csharp
PrefixEndpointAddressMessageFilter prefix1 = new PrefixEndpointAddressMessageFilter(new EndpointAddress("http://host/vdir/s.svc/b"), false);
```

> [!NOTE]
> 需要特别注意的是，根据客户端使用的是完全限定域名、NetBIOS 名称、IP 地址还是其他名称，地址的主机名部分可能有所不同。 由于不同值可以引用同一主机，因此在执行匹配时，此比较的默认行为是不使用地址的主机名部分。

如果要路由的传入消息都有一个相同的地址前缀，则应使用此筛选器。

### <a name="and"></a>AND

AND 筛选器不直接筛选消息中的值，但允许您组合两个其他筛选器来创建 `AND` 条件，其中，这两个筛选器必须匹配消息，AND 筛选器的计算结果才会为 `true`。 这样，您可以创建仅当所有子筛选器都匹配时才匹配的复杂筛选器。 下面的示例定义一个 Address 筛选器和一个 Action 筛选器，然后定义一个根据 Address 筛选器和 Action 筛选器来计算消息的 AND 筛选器。 如果 Address 筛选器和 Action 筛选器都匹配，AND 筛选器将返回 `true`。

```xml
<filter name="address1" filterType="AddressPrefix" filterData="http://host/vdir"/>
<filter name="action1" filterType="Action" filterData="http://namespace/contract/operation/"/>
<filter name="and1" filterType="And" filter1="address1" filter2="action1" />
```

```csharp
EndpointAddressMessageFilter address1 = new EndpointAddressMessageFilter(new EndpointAddress("http://host/vdir/s.svc/b"), false);
ActionMessageFilter action1 = new ActionMessageFilter(new string[] { "http://namespace/contract/operation" });
StrictAndMessageFilter and1=new StrictAndMessageFilter(address1, action1);
```

如果您必须组合多个筛选器中的逻辑来确定应进行匹配的条件，则应使用此筛选器。 例如，如果您有多个目标，而这些目标只能接收发送到特定地址的操作和消息的某些组合，则可以使用 AND 筛选器组合必要的 Action 筛选器和 Address 筛选器。

### <a name="custom"></a>自定义

选择自定义筛选器类型时，必须提供一个 customType 值，该值包含包含要用于此筛选器的 **MessageFilter** 实现的程序集的类型。 此外，filterData 必须包含自定义筛选器在计算消息时可能需要的任何值。 下面的示例定义一个 `FilterElement`，它使用 `CustomAssembly.MyCustomMsgFilter` MessageFilter 实现。

```xml
<filter name="custom1" filterType="Custom" customType="CustomAssembly.MyCustomMsgFilter, CustomAssembly" filterData="Custom Data" />
```

```csharp
MyCustomMsgFilter custom1=new MyCustomMsgFilter("Custom Data");
```

如果需要对随提供的筛选器未涵盖的消息执行自定义匹配逻辑 [!INCLUDE[netfx_current_short](../../../../includes/netfx-current-short-md.md)] ，则必须创建作为 **MessageFilter** 类的实现的自定义筛选器。 例如，您可以创建这样的自定义筛选器：比较传入消息中的字段与作为配置提供给该筛选器的已知值列表，或者对特定消息元素进行哈希处理然后检查该值以确定该筛选器应返回 `true` 还是 `false`。

### <a name="endpointname"></a>EndpointName

EndpointName 筛选器检查接收消息的终结点的名称。 下面的示例定义一个 `FilterElement` ，它使用终结点筛选器来路由 "SvcEndpoint" 上收到的消息。

```xml
<filter name="name1" filterType="Endpoint" filterData="SvcEndpoint" />
```

```csharp
EndpointNameMessageFilter name1 = new EndpointNameMessageFilter("SvcEndpoint");
```

如果路由服务公开多个指定的服务终结点，则此筛选器非常有用。 例如，您可能公开了路由服务用来接收消息的两个终结点；一个终结点由需要实时处理其消息的优先级客户使用，而另一个终结点接收不具有时效性的消息。

虽然您可能经常使用完整地址匹配来确定接收消息的终结点，但是改为使用定义的终结点名称后，会非常简便快捷而且通常不容易出错，尤其在使用配置文件配置路由服务时（此时终结点名称是必需的特性）。

### <a name="matchall"></a>MatchAll

MatchAll 筛选器匹配所有接收的消息。 如果您必须始终将所有接收的消息路由到特定终结点（例如存储所有已接收消息的副本的日志记录服务），则此筛选器非常有用。 下面的示例定义一个 `FilterElement`，它使用 MatchAll 筛选器。

```xml
<filter name="matchAll1" filterType="MatchAll" />
```

```csharp
MatchAllMessageFilter matchAll1 = new MatchAllMessageFilter();
```

### <a name="xpath"></a>XPath

使用 XPath 筛选器可以指定用于检查消息中的特定元素的 XPath 查询。 XPath 筛选是一个功能强大的筛选选项，通过它可以直接检查消息中的任何 XML 可寻址条目；但是该筛选要求您非常了解要接收的消息的结构。 下面的示例定义一个 `FilterElement` ，它使用 XPath 筛选器来检查消息中是否有 "ns" 命名空间前缀所引用的命名空间中名为 "element" 的元素。

```xml
<filter name="xpath1" filterType="XPath" filterData="//ns:element" />
```

```csharp
XPathMessageFilter xpath1=new XPathMessageFilter("//ns:element");
```

如果您知道要接收的消息包含特定值，则此筛选器非常有用。 例如，如果您要承载同一服务的两个版本，并且知道发送到较新版本服务的消息在自定义标头中包含唯一值，则您可以创建这样的筛选器：使用 XPath 导航到此标头，并比较标头中存在的值与筛选器配置中指定的另一个值来确定筛选器是否匹配。

由于 XPath 查询通常包含唯一命名空间（通常为很长或复杂的字符串值），因此 XPath 筛选器允许您使用命名空间表来定义命名空间的唯一前缀。 有关命名空间表的详细信息，请参阅 [消息筛选器](message-filters.md)。

有关设计 XPath 查询的详细信息，请参阅 [Xpath 语法](/previous-versions/dotnet/netframework-4.0/ms256471(v=vs.100))。

## <a name="see-also"></a>另请参阅

- [消息筛选器](message-filters.md)
- [如何：使用筛选器](how-to-use-filters.md)
