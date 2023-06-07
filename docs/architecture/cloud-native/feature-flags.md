---
title: 功能标志
description: 利用 Azure 应用 Config 实现云本机应用程序中的功能标志
author: robvet
ms.date: 05/13/2020
ms.openlocfilehash: ee45c9f187b056887ea6dd3a08da508afca51987
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91158092"
---
# <a name="feature-flags"></a>功能标志

第1章介绍了云本机的 affirmed，这就是速度和灵活性。 用户需要快速响应、创新的功能和零停机时间。 `Feature flags` 是一种新式部署技术，有助于提高云本机应用程序的灵活性。 使用这些功能，您可以将新功能部署到生产环境中，但限制其可用性。 通过使用开关，你可以为特定用户激活一项新功能，而无需重新启动应用程序或部署新代码。 它们将新功能的发布与代码部署分离。

功能标志是基于条件逻辑构建的，它在运行时控制用户功能的可见性。 在现代的云本机系统中，通常会将新功能部署到生产环境中，但使用有限的受众进行测试。 随着置信度的增加，此功能可以以增量方式向更广泛的受众推出。

功能标志的其他用例包括：

- 将高级功能限制为愿意支付更高订阅费用的特定客户组。
- 通过快速停用问题功能来稳定系统，避免回滚或即时修补程序的风险。
- 禁用高峰使用期间资源消耗较高的可选功能。
- `experimental feature releases`向小型用户段进行验证，以验证可行性和普及程度。

功能标志还可促进 `trunk-based` 开发。 这是一种源控制分支模型，开发人员在其中协作处理单个分支中的功能。 此方法可最大程度地降低合并大量长时间运行的功能分支的风险和复杂性。 功能在激活前不可用。

## <a name="implementing-feature-flags"></a>实现功能标志

功能标志的核心是一个简单的引用 `decision object` 。 它返回或的布尔状态 `on` `off` 。 该标志通常包装一个封装功能功能的代码块。 标志的状态确定是否为给定用户执行代码块。 图10-11 显示了实现。

```csharp
if (featureFlag) {
    // Run this code block if the featureFlag value is true
} else {
    // Run this code block if the featureFlag value is false
}
```

**图 10-11** -简单的功能标志实现。

请注意，此方法如何将决策逻辑与功能代码分隔开来。

第1章介绍了 `Twelve-Factor App` 。 建议在应用程序可执行代码中保留外部配置设置。 如果需要，可以从外部源中读取设置。 功能标志配置值还应独立于其基本代码。 通过在单独的存储库中外部化标记配置，可以更改标记状态，而无需修改和重新部署应用程序。

[Azure 应用配置](/azure/azure-app-configuration/overview) 提供了一个用于功能标志的集中式存储库。 利用它，你可以定义不同种类的功能标志，并快速、自信地操作其状态。 将应用配置客户端库添加到应用程序以启用功能标志功能。 支持各种编程语言框架。

可以在 [ASP.NET Core 服务](/azure/azure-app-configuration/use-feature-flags-dotnet-core)中轻松实现功能标志。 安装 .NET 功能管理库和应用配置提供程序使您能够以声明方式向代码添加功能标志。 它们启用 `FeatureGate` 了属性，这样就无需在代码库中手动编写 if 语句。

在 Startup 类中配置后，可以在控制器、操作或中间件级别添加功能标志功能。 图10-12 提供控制器和操作实现：

```csharp
[FeatureGate(MyFeatureFlags.FeatureA)]
public class ProductController : Controller
{
    ...
}
```

```csharp
[FeatureGate(MyFeatureFlags.FeatureA)]
public IActionResult UpdateProductStatus()
{
    return ObjectResult(ProductDto);
}
```

**图 10-12** -控制器和操作中的功能标志实现。

如果禁用功能标志，则在没有响应正文的状态代码) ，用户将收到 404 (找不到。

功能标志还可以直接注入 c # 类中。 图10-13 显示了功能标志注入：

```csharp
public class ProductController : Controller
{
    private readonly IFeatureManager _featureManager;

    public ProductController(IFeatureManager featureManager)
    {
        _featureManager = featureManager;
    }
}
```

**图 10-13** -将功能标志注入到类中。

功能管理库管理后台的功能标志生命周期。 例如，若要最大程度地减少对配置存储的大量调用，库将在指定的持续时间内缓存标记状态。 它们可以保证请求调用期间标记状态不相等。 它们还提供 `Point-in-time snapshot` 。 您可以重新构造任何键-值的历史记录，并在过去七天内的任何时刻提供其过去的值。

>[!div class="step-by-step"]
>[上一页](devops.md)
>[下一页](infrastructure-as-code.md)
