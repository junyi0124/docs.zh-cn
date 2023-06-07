---
title: 在传统 Web 应用和单页应用之间选择
description: 了解在生成 Web 应用时，如何在传统 Web 应用和单页应用程序 (SPA) 之间进行选择。
author: ardalis
ms.author: wiwagn
no-loc:
- Blazor
- WebAssembly
ms.date: 12/01/2020
ms.openlocfilehash: c29e58fb4c748d6025b7c2fbe62a54a85cc71fba
ms.sourcegitcommit: 45c7148f2483db2501c1aa696ab6ed2ed8cb71b2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/08/2020
ms.locfileid: "96851648"
---
# <a name="choose-between-traditional-web-apps-and-single-page-apps-spas"></a>在传统 Web 应用和单页应用 (SPA) 之间选择

> “Atwood 定律：任何能够用 JavaScript 编写的应用程序，最终必将用 JavaScript 编写。”  
> \- Jeff Atwood

目前可通过两种通用方法来构建 Web 应用程序：在服务器上执行大部分应用程序逻辑的传统 Web 应用程序，以及在 Web 浏览器中执行大部分用户界面逻辑的单页应用程序 (SPA)，后者主要使用 Web API 与 Web 服务器通信。 也可以将两种方法混合使用，最简单的方法是在更大型的传统 Web 应用程序中托管一个或多个丰富 SPA 类子应用程序。

何时使用传统 Web 应用程序：

- 应用程序的客户端要求简单，甚至要求只读。

- 应用程序需在不支持 JavaScript 的浏览器中工作。

- 团队不熟悉 JavaScript 或 TypeScript 开发技术。

何时使用 SPA：

- 应用程序必须公开具有许多功能的丰富用户界面。

- 团队熟悉 JavaScript、TypeScript 或 Blazor WebAssembly 开发。

- 应用程序已为其他（内部或公共）客户端公开 API。

此外，SPA 框架还需要更强的体系结构和安全专业知识。 相较于传统 Web 应用程序，SPA 框架需要进行频繁的更新和使用新框架，因此改动更大。 相较于传统 Web 应用，SPA 应用程序在配置自动化生成和部署过程以及利用部署选项（如容器）方面的难度更大。

使用 SPA 方法改进用户体验时必须权衡这些注意事项。

## Blazor

ASP.NET Core 包含一种新模型，用于构建称为 Blazor 的丰富的、交互式且可组合的用户界面。 Blazor 服务器端使开发人员能够在服务器上使用 C# 和 Razor 生成 UI，并使 UI 使用永久性 SignalR 连接以交互方式实时连接到浏览器。 Blazor WebAssembly 为 Blazor 应用引入了另一个选项，使这些应用能够使用 WebAssembly 在浏览器中运行。 由于它是在 WebAssembly 上运行的真实 .NET，因此你可以重复使用来自应用程序的服务器端部分的代码和库。

Blazor 提供了一个全新的第三个选项，可用于评估是生成纯服务器呈现的 Web 应用程序还是 SPA。 可以使用 Blazor 生成类似于 SPA 的丰富客户端行为，而无需进行大量 JavaScript 开发。 Blazor 应用程序可以调用 API 来请求数据或执行服务器端操作。 这些应用程序可以在必要时与 JavaScript 进行互操作，以利用 JavaScript 库和框架。

在以下情况下考虑使用 Blazor 生成 Web 应用程序：

- 应用程序必须公开丰富用户界面

- 与使用 JavaScript 或 TypeScript 开发相比，团队更喜欢使用 .NET 开发

如果正在考虑将现有的 Web Forms 应用程序迁移到 .NET Core 或最新的 .NET，不妨参阅免费的电子书《[面向 Web Forms 开发人员的 Blazor](../blazor-for-web-forms-developers/index.md)》，以了解是否有必要将其迁移到 Blazor。

有关 Blazor 的详细信息，请参阅 [Blazor 入门](https://blazor.net/docs/get-started.html)。

## <a name="when-to-choose-traditional-web-apps"></a>何时选择传统 Web 应用

以下部分详细介绍前面提到的选择传统 Web 应用程序的原因。

**应用程序的客户端要求简单，可能要求只读**

对许多 Web 应用程序而言，其大部分用户的主要使用方式是只读。 只读（或以读取为主）应用程序往往比那些维护和操作大量状态的应用程序简单得多。 例如，搜索引擎可能由一个带有文本框的入口点和用于显示搜索结果的第二页组成。 匿名用户可以轻松提出请求，并且很少需要使用客户端逻辑。 同样，一般而言，博客或内容管理系统中面向公众的应用程序主要包含的内容与客户端行为关系不大。 此类应用程序容易构建为基于服务器的传统 Web 应用程序，在 Web 服务器上执行逻辑，并呈现要在浏览器中显示的 HTML。 事实上，网站的每个独特页面都有自己的 URL，搜索引擎可以将其存为书签和编入索引（默认设置，无需将此功能添加为应用程序的单独功能），这也是此类情况的一个明显优势。

**应用程序需在不支持 JavaScript 的浏览器中工作**

如需在有限或不支持 JavaScript 的浏览器中工作的 Web 应用程序，则应使用传统的 Web 应用工作流编写（或至少可以回退到此类行为）。 SPA 需要客户端 JavaScript 才能正常工作；如果没有客户端 JavaScript，SPA 不是好的选择。

**团队不熟悉 JavaScript 或 TypeScript 开发技术**

如果团队不熟悉 JavaScript 或 TypeScript，但熟悉服务器端 Web 应用程序开发，那相较于 SPA，他们交付传统 Web 应用的速度可能更快。 除非以学习 SPA 编程为目的，或需要 SPA 提供用户体验，否则对已经熟悉构建传统 Web 应用的团队而言，选择传统 Web 应用的工作效率更高。

## <a name="when-to-choose-spas"></a>何时选择 SPA

以下部分详细介绍何时为 Web 应用选择单页应用程序开发样式。

**应用程序必须公开具有许多功能的丰富用户界面**

SPA 可支持丰富客户端功能，当用户执行操作或在应用的各区域间导航时无需重新加载页面。 SPA 很少需要重新加载整个页面，因此加载速度更快，可在后台提取数据，并且对单个用户操作的响应更快。 SPA 支持增量更新，可保存尚未完成的窗体或文档，而无需用户单击按钮提交窗体。 SPA 支持丰富的客户端行为，例如拖放，比传统应用程序更容易操作。 可以将 SPA 设计为在断开连接的模式下运行，对客户端模型进行更新，并在重新建立连接后将更新最终同步回服务器。 如果应用要求包括丰富的功能，且超出了典型 HTML 窗体提供的功能，则选择 SPA 样式应用程序。

通常，SPA 需要实现内置于传统 Web 应用中的功能，例如在反映当前操作的地址栏中显示有意义的 URL（并允许用户将此 URL 存为书签或对其进行深层链接以便返回此 URL）。 SPA 还应允许用户使用浏览器的后退和前进按钮寻找用户意料之中的结果。

**团队熟悉 JavaScript 和/或 TypeScript 开发**

编写 SPA 需要熟悉 JavaScript 和/或 TypeScript 以及客户端编程技术和库。 团队应有能力像使用 Angular 一样使用 SPA 框架编写新式 JavaScript。

> ### <a name="references--spa-frameworks"></a>参考 - SPA 框架
>
> - **Angular**  
>   <https://angular.io>
> - **React**
>   <https://reactjs.org/>
> - **JavaScript 框架的比较**  
>   <https://jsreport.io/the-ultimate-guide-to-javascript-frameworks/>

**应用程序已为其他（内部或公共）客户端公开 API**

如果已提供一个 Web API 供其他客户端使用，则相较于在服务器端窗体中复制逻辑，创建一个利用这些 API 的 SPA 实现更加容易。 用户与应用程序交互时，SPA 广泛使用 Web API 来查询和更新数据。

## <a name="when-to-choose-no-locblazor"></a>何时选择 Blazor

以下部分是有关何时为 Web 应用选择 Blazor 更加详细的说明。

**应用程序必须公开丰富用户界面**

与基于 JavaScript 的 SPA 一样，Blazor 应用程序可以支持丰富的客户端行为，而无需重载页面。 这些应用程序对用户的响应更快，仅获取响应给定用户交互所需的数据（或 HTML）。 如果设计得当，可以将服务器端 Blazor 应用配置为以客户端 Blazor 应用的形式运行，只需在此功能受支持后对它进行稍加更改即可。

**与使用 JavaScript 或 TypeScript 开发相比，团队更喜欢使用 .NET 开发**

与使用 JavaScript 或 TypeScript 等客户端语言相比，许多使用 .NET 和 Razor 的开发人员的工作效率更高。 由于已经使用 .NET 开发了应用程序的服务器端，因此，使用 Blazor 可以确保团队中的每名 .NET 开发人员都可以理解，并可能会生成应用程序前端的行为。

## <a name="decision-table"></a>决策表

下面的决策表总结了在传统 Web 应用程序、SPA 或 Blazor 应用之间进行选择时要考虑的一些基本因素。

| **因素**                                           | **传统 Web 应用** | **单页面应用程序** | **Blazor 应用**  |
| ---------------------------------------------------- | ----------------------- | --------------------------- | --------------- |
| 需要团队熟悉 JavaScript/TypeScript | **最低**             | **必需**                | **最低**     |
| 支持不带脚本的浏览器                   | **支持**           | **不支持**           | **支持**   |
| 客户端应用程序行为极少             | **适合**         | **不必要**                | **可行**      |
| 丰富而复杂的用户界面要求            | **受限**             | **适合**             | **适合** |

>[!div class="step-by-step"]
>[上一页](modern-web-applications-characteristics.md)
>[下一页](architectural-principles.md)
