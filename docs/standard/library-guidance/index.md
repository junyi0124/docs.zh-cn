---
title: 开放源代码 .NET 库指南
description: 有关创建高质量的 .NET 库的开发人员最佳做法建议。
ms.date: 10/17/2018
ms.openlocfilehash: 4c76dfae6ffc39df7f15381be64e33657067d79d
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2020
ms.locfileid: "76731425"
---
# <a name="open-source-library-guidance"></a>开放源代码库指南

本指南向开发人员提供了有关创建高质量的 .NET 库的建议。 本文档重点介绍在构建 .NET 库时的操作内容和原因，还不是操作方式    。

有关优质开源 .NET 库的方方面面：

> [!div class="checklist"]
>
> * 包容性  - 优秀的 .NET 库致力于支持众多平台、编程语言和应用程序。
> * 稳定性：优秀的 .NET 系统在具有众多库的应用程序中运行的 .NET 生态系统中共存  。
> * 设计为可改进：.NET 库要随着时间的推移进行改进和演变，同时支持现有用户  。
> * 可调试：.NET 库要使用最新的工具，为用户打造卓越的调试体验  。
> * 受信任：.NET 库通过安全最佳做法发布到 NuGet，备受开发人员的信赖  。

> [!div class="nextstepaction"]
> [入门](./get-started.md)

## <a name="types-of-recommendations"></a>建议类型

每篇文章介绍四种类型的建议：“请执行”  、“请考虑”、  、“请避免”、  和“请勿”  。 建议类型表示了应遵循的程度。

应始终遵循“请执行”  建议。 例如:

✔️ 请使用 NuGet 包分发库。

在另一方面，“请考虑”建议是在一般情况下要遵循的建议，但存在该规则的合法例外，此时不遵循指南也不妨  ：

✔️ 请考虑使用 [SemVer 2.0.0](https://semver.org/) 控制 NuGet 包的版本。

“请避免”  建议是指在一般情况下不应执行的操作，但有时也可以打破规则：

❌ 请避免使用需要确切版本的 NuGet 包引用。

最后，“请勿”  建议是指在大多数情况下不得执行的操作：

❌ 请勿发布库的强名称和非强名称版本。 例如，`Contoso.Api` 和 `Contoso.Api.StrongNamed`。

>[!div class="step-by-step"]
>[下一部分](get-started.md)
