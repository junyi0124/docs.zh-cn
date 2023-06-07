---
title: 适用于 .NET 的安全编码准则
description: 要使用的设计代码。NET 强制执行的权限和其他强制，有助于防止恶意代码访问数据或执行其他操作。
ms.date: 07/15/2020
helpviewer_keywords:
- managed wrapper to native code implementation
- secure coding
- reusable components
- library code that exposes protected resources
- code, security
- code security
- secure coding, options
- components [.NET], security
- code security, options
- security-neutral code
- security [.NET], coding guidelines
ms.assetid: 4f882d94-262b-4494-b0a6-ba9ba1f5f177
ms.openlocfilehash: a05e0cec2814be88ac835d05601d5cf5f66383c3
ms.sourcegitcommit: b7a8b09828bab4e90f66af8d495ecd7024c45042
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/04/2020
ms.locfileid: "87555951"
---
# <a name="secure-coding-guidelines"></a>安全编码准则

大多数应用程序代码只能使用 .NET 实现的基础结构。 在某些情况下，需要额外的应用程序特定的安全性，或通过扩展安全系统或通过使用全新临时方法构建。

如果在代码中使用 .NET 强制的权限和其他强制，则应建立屏障，以防止恶意代码访问不需要的信息或执行其他不需要的操作。 此外，必须在使用受信任代码的所有预期方案中平衡安全性和可用性。

本概述介绍代码可以用于处理安全系统的不同方式。

## <a name="securing-resource-access"></a>保护资源访问

设计和编写代码时，尤其是在使用或调用来源未知的代码时，需要保护和限制该代码对资源的访问。 因此，请记住以下可确保代码安全的方法：

- 不使用代码访问安全性 (CAS)。

- 不使用部分信任的代码。

- 不要使用[AllowPartiallyTrustedCaller](xref:System.Security.AllowPartiallyTrustedCallersAttribute)特性 (APTCA) 。

- 不使用 .NET 远程处理。

- 不使用分布式组件对象模型 (DCOM)。

- 不使用二进制格式化程序。

不支持将代码访问安全性和安全透明代码用作部分受信任代码的安全边界。 建议在未实施其他安全措施的情况下，不要加载和执行未知来源的代码。 其他安全措施包括：

- 虚拟化

- AppContainers

- 操作系统 (OS) 用户和权限

- Hyper-V 容器

## <a name="security-neutral-code"></a>非特定于安全性的代码

非特定于安全性的代码不对任何安全系统进行显示处理。 它通过所接收的任何权限来运行。 虽然无法捕获与受保护操作（例如使用文件、网络等）关联的安全异常的应用程序 (如使用文件、网络等) 可能导致未经处理的异常，但非特定于安全性的代码仍利用 .NET 中的安全技术。

非特定于安全性的库具有你应了解的特殊性质。 假设你的库提供了使用文件或调用非托管代码的 API 元素。 如果你的代码没有相应的权限，则它不会如所述运行。 但是，即使代码具有权限，调用它的任何应用程序代码必须具有相同的权限才能正常运行。 如果调用代码没有正确的权限，则 <xref:System.Security.SecurityException> 将作为代码访问安全堆栈审核的结果显示。

## <a name="application-code-that-isnt-a-reusable-component"></a>不是可重用组件的应用程序代码

如果您的代码是不由其他代码调用的应用程序的一部分，则安全性很简单，可能不需要特殊的编码。 但请记住，恶意代码可以调用你的代码。 代码访问安全性可能会阻止恶意代码访问资源，而此类代码仍然可以读取你的字段或可能包含敏感信息的属性的值。

此外，如果你的代码接受来自 Internet 或其他不可靠来源的用户输入，则务必要小心恶意输入。

## <a name="managed-wrapper-to-native-code-implementation"></a>本机代码实现的托管包装

通常在这种情况下，某些有用功能是在你想要提供给托管代码的本机代码中实现的。 托管包装器可通过使用平台调用或 COM 互操作轻松写入。 但如果你这样做，包装器的调用方必须具有非托管代码权限才能成功。 在默认策略下，这意味着从 intranet 或 Internet 下载的代码将不能与包装一起使用。

最好仅向包装器代码提供这些权限，而不是向使用这些包装器的所有应用程序提供非托管代码权限。 如果基础功能没有公开任何资源，且实现同样也安全，则包装器只需断言其权限，这可使任何代码通过它进行调用。 当涉及资源时，安全编码应该与下一节中所述的库代码案例相同。 因为包装器可能对这些资源公开调用方，所以仔细验证本机代码的安全性是必要的，这是包装器的责任。

## <a name="library-code-that-exposes-protected-resources"></a>用于公开受保护资源的库代码

以下方法是最强大的方法，因此，如果未正确地) 进行安全编码，则可能会造成潜在的危险 (：你的库作为其他代码访问某些不可用的资源的接口，就像 .NET 类强制执行所使用资源的权限一样。 只要公开资源，你的代码首先就必须要求相应资源的权限（也就是说，必须执行安全检查），然后通常断言其权限来执行实际的操作。

## <a name="see-also"></a>请参阅

- [保护状态数据](securing-state-data.md)
- [安全性和用户输入](security-and-user-input.md)
- [安全和争用条件](security-and-race-conditions.md)
- [安全性和进行中的代码生成](security-and-on-the-fly-code-generation.md)
- [基于角色的安全性](role-based-security.md)
- [ASP.NET Core 安全性](/aspnet/core/security/)
