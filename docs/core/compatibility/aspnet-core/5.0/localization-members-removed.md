---
title: 中断性变更：本地化：ResourceManagerWithCultureStringLocalizer 类和 WithCulture 接口成员已删除
description: 了解 ASP.NET Core 5.0 中的以下中断性变更：本地化：ResourceManagerWithCultureStringLocalizer 类和 WithCulture 接口成员已删除
author: scottaddie
ms.author: scaddie
ms.date: 10/01/2020
ms.openlocfilehash: ac7723cd9b961b34b3f87a55119d421668c87417
ms.sourcegitcommit: 721c3e4bdbb1ea0bb420818ec944c538fe5c513a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/01/2020
ms.locfileid: "96437852"
---
# <a name="localization-resourcemanagerwithculturestringlocalizer-class-and-withculture-interface-member-removed"></a>本地化：ResourceManagerWithCultureStringLocalizer 类和 WithCulture 接口成员已删除

.NET 5.0 预览版 1 删除了 [ResourceManagerWithCultureStringLocalizer](/dotnet/api/microsoft.extensions.localization.resourcemanagerwithculturestringlocalizer?view=dotnet-plat-ext-3.1) 类和 [WithCulture](/dotnet/api/microsoft.extensions.localization.resourcemanagerstringlocalizer.withculture?view=dotnet-plat-ext-3.1) 方法。

有关上下文，请参阅 [aspnet/Announcements#346](https://github.com/aspnet/Announcements/issues/346) 和 [dotnet/aspnetcore#3324](https://github.com/dotnet/aspnetcore/issues/3324)。 有关此更改的讨论，请参阅 [dotnet/aspnetcore#7756](https://github.com/dotnet/aspnetcore/issues/7756)。

## <a name="version-introduced"></a>引入的版本

5.0 预览版 1

## <a name="old-behavior"></a>旧行为

`ResourceManagerWithCultureStringLocalizer` 类和 `ResourceManagerStringLocalizer.WithCulture` 方法[在 .NET Core 3.0 预览版 3 及更高版本中已过时](../../3.0.md#localization-resourcemanagerwithculturestringlocalizer-and-withculture-marked-obsolete)。

## <a name="new-behavior"></a>新行为

.NET 5.0 预览版 1 中删除了 `ResourceManagerWithCultureStringLocalizer` 类和 `ResourceManagerStringLocalizer.WithCulture` 方法。 有关所做更改的清单，请参阅 [dotnet/extensions#2562](https://github.com/dotnet/extensions/pull/2562/files) 上的拉取请求。

## <a name="reason-for-change"></a>更改原因

[ResourceManagerWithCultureStringLocalizer](/dotnet/api/microsoft.extensions.localization.resourcemanagerwithculturestringlocalizer?view=dotnet-plat-ext-3.1) 类和 [ResourceManagerStringLocalizer.WithCulture](/dotnet/api/microsoft.extensions.localization.resourcemanagerstringlocalizer.withculture?view=dotnet-plat-ext-3.1) 方法通常会让本地化用户混淆。 创建自定义 <xref:Microsoft.Extensions.Localization.IStringLocalizer> 实现时，混淆尤其严重。 这个类和方法给使用者的印象是 `IStringLocalizer` 实例应“按语言、按资源”。 实际上，实例应该仅“按资源”。 在运行时，<xref:System.Globalization.CultureInfo.CurrentUICulture%2A?displayProperty=nameWithType> 属性确定要使用的语言。

## <a name="recommended-action"></a>建议操作

停止使用 `ResourceManagerWithCultureStringLocalizer` 类和 `ResourceManagerStringLocalizer.WithCulture` 方法。

## <a name="affected-apis"></a>受影响的 API

- [ResourceManagerWithCultureStringLocalizer](/dotnet/api/microsoft.extensions.localization.resourcemanagerwithculturestringlocalizer?view=dotnet-plat-ext-3.1)
- [ResourceManagerStringLocalizer.WithCulture](/dotnet/api/microsoft.extensions.localization.resourcemanagerstringlocalizer.withculture?view=dotnet-plat-ext-3.1)

<!--

### Category

ASP.NET Core

### Affected APIs

- `T:Microsoft.Extensions.Localization.ResourceManagerWithCultureStringLocalizer`
- `Overload:Microsoft.Extensions.Localization.ResourceManagerStringLocalizer.WithCulture`

-->
