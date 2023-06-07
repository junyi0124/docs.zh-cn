---
title: 中断性变更：SignalR：MessagePack 中心协议选项类型已更改
description: 了解 ASP.NET Core 5.0 中的以下中断性变更：SignalR：MessagePack 中心协议选项类型已更改
author: scottaddie
ms.author: scaddie
ms.date: 10/01/2020
ms.openlocfilehash: b75dbec834699472f18b3058052274476bd9751d
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95759294"
---
# <a name="signalr-messagepack-hub-protocol-options-type-changed"></a>SignalR：MessagePack 中心协议选项类型已更改

ASP.NET Core SignalR MessagePack 中心协议选项类型已从 `IList<MessagePack.IFormatterResolver>` 更改为 [MessagePack](https://www.nuget.org/packages/MessagePack) 库的 `MessagePackSerializerOptions` 类型。

有关此更改的讨论，请参阅 [dotnet/aspnetcore#20506](https://github.com/dotnet/aspnetcore/issues/20506)。

## <a name="version-introduced"></a>引入的版本

5.0 预览版 4

## <a name="old-behavior"></a>旧行为

可以添加到选项，如下面的示例所示：

```csharp
services.AddSignalR()
    .AddMessagePackProtocol(options =>
    {
        options.FormatterResolvers.Add(MessagePack.Resolvers.StandardResolver.Instance);
    });
```

并替换选项，如下所示：

```csharp
services.AddSignalR()
    .AddMessagePackProtocol(options =>
    {
        options.FormatterResolvers = new List<MessagePack.IFormatterResolver>()
        {
            MessagePack.Resolvers.StandardResolver.Instance
        };
    });
```

## <a name="new-behavior"></a>新行为

可以添加到选项，如下面的示例所示：

```csharp
services.AddSignalR()
    .AddMessagePackProtocol(options =>
    {
        options.SerializerOptions =
            options.SerializeOptions.WithResolver(MessagePack.Resolvers.StandardResolver.Instance);
    });
```

并替换选项，如下所示：

```csharp
services.AddSignalR()
    .AddMessagePackProtocol(options =>
    {
        options.SerializerOptions = MessagePackSerializerOptions
                .Standard
                .WithResolver(MessagePack.Resolvers.StandardResolver.Instance)
                .WithSecurity(MessagePackSecurity.UntrustedData);
    });
```

## <a name="reason-for-change"></a>更改原因

此更改是迁移到 MessagePack v2.x 的一部分，已在 [aspnet/Announcements#404](https://github.com/aspnet/Announcements/issues/404) 中公布。 v2.x 库添加了一个更易于使用的选项 API，提供的功能比之前公开的 `MessagePack.IFormatterResolver` 的列表更多。

## <a name="recommended-action"></a>建议操作

此重大更改将影响在 <xref:Microsoft.AspNetCore.SignalR.MessagePackHubProtocolOptions> 上配置值的用户。 如果使用 ASP.NET Core SignalR MessagePack 中心协议并修改这些选项，请更新用法以使用新的选项 API，如上所示。

## <a name="affected-apis"></a>受影响的 API

<xref:Microsoft.AspNetCore.SignalR.MessagePackHubProtocolOptions?displayProperty=nameWithType>

<!--

### Category

ASP.NET Core

### Affected APIs

`T:Microsoft.AspNetCore.SignalR.MessagePackHubProtocolOptions`

-->
