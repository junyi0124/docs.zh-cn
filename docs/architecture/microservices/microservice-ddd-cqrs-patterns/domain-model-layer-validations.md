---
title: 在域模型层中设计验证
description: 适用于容器化 .NET 应用程序的 .NET 微服务体系结构 | 了解域模型验证的关键概念。
ms.date: 10/08/2018
ms.openlocfilehash: 18c8350d0bf514a8a01a210a2a2a6d8f73317580
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94820626"
---
# <a name="design-validations-in-the-domain-model-layer"></a>在域模型层中设计验证

在 DDD 中，验证规则可以看作不变量。 聚合的主要责任是对该聚合内所有实体跨状态更改来执行不变量。

域实体应始终为有效的实体。 应始终为 true 的对象存在一定数量的不变量。 例如，订单项对象始终得有一个必须为正整数的数量，以及项目名称和价格。 因此，执行不变量是域实体（尤其是聚合根）的职责，而实体对象应不能在无效的情况下存在。 不变量规则只需表达为协定，当它们遭违反时会引发异常或通知。

这背后的原因是，很多 bug 因对象处于它们不应处于的状态而发生。

比如说，我们现在有一个采用 UserProfile 的 SendUserCreationEmailService...我们可以如何理性解释该服务中其名称不为 null？ 是不是要再次检查？ 或者更有可能...不需要花精力检查而是“期待更好的发生”- 希望其他人在将其发送给你之前花时间进行了验证。 当然，使用 TDD 时，我们首先应该编写的一个测试是，我是否向客户发送了会引发错误的 null 名称。 但是，一旦我们开始反复编写这些类型的测试，我们就会认识到...“等一下，如果我们从不允许名称成为 null，我们就不会有所有这些测试”。

## <a name="implement-validations-in-the-domain-model-layer"></a>在域模型层中实现验证

验证通常在域实体构造函数或可以更改该实体的方法中实现。 有多种方法来实现验证，例如验证失败时验证数据和引发的异常。 还有更多高级的模式，例如使用验证的规范模式和通知模式来返回一系列错误，而不是在异常发生时返回每个验证的异常。

### <a name="validate-conditions-and-throw-exceptions"></a>验证条件和引发异常

下面的代码示例通过引发异常显示域实体中最简单的验证方法。 在本部分末尾的引用表中，可以看到指向基于之前讨论的模式的更多高级实现。

```csharp
public void SetAddress(Address address)
{
    _shippingAddress = address?? throw new ArgumentNullException(nameof(address));
}
```

更好的示例将演示，需要确保内部状态未更改，或者方法的所有变化都已发生。 例如，下面的实现将使对象保持处于无效状态：

```csharp
public void SetAddress(string line1, string line2,
    string city, string state, int zip)
{
    _shippingAddress.line1 = line1 ?? throw new ...
    _shippingAddress.line2 = line2;
    _shippingAddress.city = city ?? throw new ...
    _shippingAddress.state = (IsValid(state) ? state : throw new …);
}
```

如果状态的值无效，则第一个地址行和城市已更改。 这可能使地址无效。

类似的方法可用于实体的构造函数中，引发异常来确保实体在创建后是有效的。

### <a name="use-validation-attributes-in-the-model-based-on-data-annotations"></a>在模型中根据数据注释使用验证属性

数据注释与 Required 或 MaxLength 特性一样，可用于配置 EF Core 数据库字段属性（[表映射](infrastructure-persistence-layer-implementation-entity-framework-core.md#table-mapping)部分对此进行了详细说明）。自 .NET Framework 中的 EF 4.x 以来，可使用数据注释进行实体验证，但[它们不再适用于 EF Core 中的实体验证](https://github.com/dotnet/efcore/issues/3680)（<xref:System.ComponentModel.DataAnnotations.IValidatableObject.Validate%2A?displayProperty=nameWithType> 方法也是如此）。

在模型绑定期间（即，在像往常一样调用控制器的操作之前），数据注释和 <xref:System.ComponentModel.DataAnnotations.IValidatableObject> 接口仍然可用于模型验证，但该模型应该是一个视图模型或 DTO，因此这属于 MVC 或 API 方面的问题，而不是域模型方面的问题。

弄清楚概念上的差别后，如果你的操作收到实体类对象参数，你仍然可以在实体类中使用数据注释和 `IValidatableObject` 进行验证，虽然并不建议这样做。 在这种情况下，将在模型绑定期间（就在调用操作之前）进行验证，你可以检查控制器的 ModelState.IsValid 属性以检查结果，但是，验证操作发生在控制器中，而不是在将实体对象永久保存在 DbContext 中之前，这一点也与自 EF 4.x 以来的版本不同。

你仍然可以通过重写 DbContext 的 SaveChanges 方法，在实体类中使用数据注释和 `IValidatableObject.Validate` 方法来实现自定义验证。

在 [GitHub 上的此注释](https://github.com/dotnet/efcore/issues/3680#issuecomment-155502539)中，可以看到用于验证 `IValidatableObject` 实体的示例实现。 该示例不执行基于属性的验证，但应该可以在同一替代机制中使用反射来轻松实现此类验证。

但是，从 DDD 的角度来看，最好通过在实体行为方法中使用异常，或者通过实现规范和通知模式来执行验证规则，从而使域模型保持精简。

在将接受输入的 ViewModel 类（而非域实体）中在应用程序层上使用数据注释来允许 UI 层中的模型验证很有意义。 但是，这不应在域模型内排除验证时完成。

### <a name="validate-entities-by-implementing-the-specification-pattern-and-the-notification-pattern"></a>通过实现规范模式和通知模式来验证实体

最后，域模型中实现验证的更复杂方法是，通过结合使用通知模式来实现规范模式，如后面列出的其他资源中所述。

值得一提的是，还可使用这些模式中一个 - 例如通过控制语句进行手动验证，但使用通知模式来堆叠并返回验证错误列表。

### <a name="use-deferred-validation-in-the-domain"></a>在域中使用延迟的验证

有数种方法来处理域中的延迟验证。 [实现域驱动设计](https://www.amazon.com/Implementing-Domain-Driven-Design-Vaughn-Vernon/dp/0321834577)一书中，Vaughn Vernon 在验证部分讨论了这些。

### <a name="two-step-validation"></a>双重验证

此外考虑双重验证。 在命令数据传输对象 (DTO) 上使用字段级别验证，在实体内使用域级别验证。 通过返回结果对象而非异常来让处理验证错误变得更容易，以便执行此操作。

例如通过将字段验证用于数据注释，你不用重复验证定义。 然而在 DTO（例如命令和 Viewmodel）的情况下，执行可以既位于服务器端又位于客户端端。

## <a name="additional-resources"></a>其他资源

- **Rachel Appel。ASP.NET Core MVC 模型验证简介** \
  <https://docs.microsoft.com/aspnet/core/mvc/models/validation>

- **Rick Anderson。添加验证** \
  <https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/validation>

- **Martin Fowler。Replacing Throwing Exceptions with Notification in Validations** \（在验证中将引发异常替换为通知）\
  <https://martinfowler.com/articles/replaceThrowWithNotification.html>

- **Specification and Notification Patterns** \（规范和通知模式）\
  <https://www.codeproject.com/Tips/790758/Specification-and-Notification-Patterns>

- **Lev Gorodinski.Validation in Domain-Driven Design (DDD)**  \（域驱动设计 (DDD) 中的验证）\
  <http://gorodinski.com/blog/2012/05/19/validation-in-domain-driven-design-ddd/>

- **Colin Jack。Domain Model Validation** \（域模型验证）\
  <https://colinjack.blogspot.com/2008/03/domain-model-validation.html>

- **Jimmy Bogard。Validation in a DDD world** \（DDD 中的验证）\
  <https://lostechies.com/jimmybogard/2009/02/15/validation-in-a-ddd-world/>

> [!div class="step-by-step"]
> [上一页](enumeration-classes-over-enum-types.md)
> [下一页](client-side-validation.md)
