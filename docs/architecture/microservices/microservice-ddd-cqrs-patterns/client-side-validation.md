---
title: 客户端验证（表示层中的验证）
description: 适用于容器化 .NET 应用程序的 .NET 微服务体系结构 | 探索客户端验证的关键概念。
ms.date: 10/08/2018
ms.openlocfilehash: 44c1e9fa280b19fcee87d4d1cdfcaa2ab9462f27
ms.sourcegitcommit: e3cbf26d67f7e9286c7108a2752804050762d02d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/09/2020
ms.locfileid: "80988696"
---
# <a name="client-side-validation-validation-in-the-presentation-layers"></a>客户端验证（表示层中的验证）

即使真实源是域模型，最终也必须在域模型级别进行验证，验证仍然可以在域模型级别（服务器端）和 UI（客户端）处理。

客户端验证极大地方便了用户。 它节省了时间，让用户不必浪费时间等待服务器往返，服务器有可能返回验证错误。 从商业角度而言，即使每次只有几分之一秒，但如果每天发生几百次，也会耗费大量的时间和成本，带来很多不必要的烦恼。 简单直接的验证能够提高用户的工作效率和投入产出比。

正是因为视图模型和域模型有所不同，所以即使视图模型验证和域模型验证可能相似，也不可能用于相同目的。 如果担心 DRY（不要自我重复原则），请想想在这种情况下代码重用也可能意味着耦合，而在企业应用程序中，比起遵循 DRY 原则，不要将服务器端耦合到客户端更为重要。

即使是使用客户端验证时，也应一直验证命令或在服务器代码中输入 DTO，因为服务器 API 可能是攻击途径。 通常情况下，最好是两项操作都进行，因为从 UX 角度，如果具有一个客户端应用程序，最好要有远见并禁止用户输入无效信息。

因此，通常要在客户端代码中验证 ViewModel。 将客户端输出的 DTO 或命令发送到服务之前，还可以对其进行验证。

客户端验证实现取决于创建的客户端应用程序类型。 如果使用 .NET 中的大多数代码验证 Web MVC Web 应用程序中的数据，情况便有所不同，使用该验证的 SPA Web 应用程序会在 JavaScript、TypeScript 或移动应用（编有代码 Xamarin 和 C#）中进行编码。

## <a name="additional-resources"></a>其他资源

### <a name="validation-in-xamarin-mobile-apps"></a>Xamarin 移动应用中进行的验证

- **验证文本输入并显示错误** \
  [https://developer.xamarin.com/recipes/ios/standard\_controls/text\_field/validate\_input/](https://developer.xamarin.com/recipes/ios/standard_controls/text_field/validate_input/)

- **验证回调** \
  <https://developer.xamarin.com/samples/xamarin-forms/XAML/ValidationCallback/>

### <a name="validation-in-aspnet-core-apps"></a>ASP.NET Core 应用中进行的验证

- **Rick Anderson。添加验证** \
  <https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/validation>

### <a name="validation-in-spa-web-apps-angular-2-typescript-javascript"></a>SPA Web 应用中进行的验证（Angular 2、TypeScript、JavaScript）

- **Ado Kukic.Angular 2 窗体验证** \
  <https://scotch.io/tutorials/angular-2-form-validation>

- **窗体验证** \
  <https://angular.io/guide/form-validation>

- **验证。** Breeze 文档。 \
  <https://breeze.github.io/doc-js/validation.html>

总之，与验证有关的最重要的概念有以下几条：

- 实体和聚合应确保它们自己的一致性，并保证“始终有效”。 聚合根保证同一聚合内的多实体间的一致性。

- 如果认为实体需要进入无效状态，请考虑使用不同的对象模型 - 例如，在创建最终的域实体之前，使用临时 DTO。

- 如果需要创建多个相关对象（如聚合），且其只有在完成创建所有对象之后才会有效，请考虑使用工厂模式。

- 在大多数情况下，客户端采用冗余验证是不错的选择，因为应用程序可以是积极主动的。

>[!div class="step-by-step"]
>[上一页](domain-model-layer-validations.md)
>[下一页](domain-events-design-implementation.md)
