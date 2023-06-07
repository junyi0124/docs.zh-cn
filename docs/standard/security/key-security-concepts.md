---
title: 安全性的基础概念
ms.date: 07/15/2020
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- unauthorized access
- permissions [.NET]
- security [.NET], about security
ms.assetid: 3cfced4f-ea02-4e66-ae98-d69286363e98
ms.openlocfilehash: 73e4d0474810d097c5eee8b99ae30b6096ee1695
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95687542"
---
# <a name="key-security-concepts"></a>安全性的基础概念

> [!NOTE]
> 本文适用于 Windows。
>
> 有关 ASP.NET Core 的信息，请参阅 [ASP.NET Core 安全性概述](/aspnet/core/security/)。

.NET 提供基于角色的安全性，以帮助解决有关移动代码的安全性问题，并提供支持，使组件能够确定用户有权执行的操作。  
  
## <a name="type-safety-and-security"></a>类型安全和安全性  

类型安全代码只访问其有权访问的内存位置。  (此讨论，类型安全专门指内存类型安全，不应与更广泛的类型安全混淆。 ) 例如，类型安全代码无法从另一个对象的私有字段读取值。 它仅以明确定义且经允许的方式访问类型。  
  
在实时 (JIT) 编译期间，一个可选的验证过程将检查方法（该方法将被实时编译为本机代码）的元数据和 Microsoft 中间语言 (MSIL)，以验证它们是否类型安全。 如果代码有权绕过验证，则将跳过此过程。 有关验证的详细信息，请参阅[托管执行过程](../managed-execution-process.md)。  
  
虽然类型安全验证并不强制性要求运行托管代码，但类型安全在程序集隔离和安全性强制中扮演着重要角色。 代码类型安全时，公共语言运行时可以将程序集彼此完全隔离。 这种隔离可帮助确保程序集不能对彼此产生负面影响，同时可增加应用程序的可靠性。 类型安全组件可在相同进程中安全地执行，即使它们的信任级别不同。 代码不是类型安全的代码时，可能会产生意外的副作用。 例如，运行时无法阻止托管代码调用到本机（非托管）代码中和执行恶意操作。 代码是类型安全的代码时，运行时的安全强制机制可确保它不会访问本机代码，除非它有权这么做。 非类型安全的所有代码都必须被授予 <xref:System.Security.Permissions.SecurityPermission> 和传递的枚举成员 <xref:System.Security.Permissions.SecurityPermissionAttribute.SkipVerification%2A>才能运行。  
  
有关详细信息，请参阅 [Code Access Security Basics](../../framework/misc/code-access-security-basics.md)。  
  
## <a name="principal"></a>主体  

主体表示用户的标识和角色，并代表用户进行操作。 .NET 中基于角色的安全性支持三种主体：  
  
- 泛型主体表示独立于 Windows 用户和角色存在的用户和角色。  
  
- Windows 主体表示 Windows 用户及其角色（或其 Windows 组）。 Windows 主体可以模拟其他用户，这意味着当代表属于一个用户的标识时，主体可代表该用户访问资源。  
  
- 应用程序可以用任何所需的方式自定义主体。 它们可以扩展该主体的标识和角色的基本概念。  
  
有关详细信息，请参阅[主体和标识对象](principal-and-identity-objects.md)。  
  
## <a name="authentication"></a>身份验证  

身份验证是通过检查用户凭据和针对某些颁发机构对这些凭据进行验证来发现和验证主体的标识的过程。 身份验证期间获取的信息可直接为你的代码所用。 还可以使用基于 .NET 角色的安全性对当前用户进行身份验证并确定是否允许该主体访问代码。 请参阅 <xref:System.Security.Principal.WindowsPrincipal.IsInRole%2A?displayProperty=nameWithType> 方法的重载，了解有关如何对特定角色的主体进行身份验证的示例。 例如，可以使用 <xref:System.Security.Principal.WindowsPrincipal.IsInRole%28System.String%29?displayProperty=nameWithType> 重载确定当前用户是否是“Administrators”组的成员。  
  
目前使用多种身份验证机制，其中的许多机制可用于基于 .NET 角色的安全性。 最常用的一些机制包括 basic、digest、Passport、操作系统（例如，NTLM 或 Kerberos）或应用程序定义的机制。  
  
### <a name="example"></a>示例  

下面的示例要求活动主体是管理员。 `name` 参数为 `null`，这使任何身份为管理员的用户均满足该要求。  
  
> [!NOTE]
> 在 Windows Vista 中，用户帐户控制 (UAC) 决定用户的特权。 如果您是内置的 Administrators 组的成员，将为您分配两个运行时访问令牌：一个标准用户访问令牌和一个管理员访问令牌。 默认情况下，您拥有标准用户角色。 要执行需要管理员身份的代码，必须首先将你的特权从标准用户提升至管理员。 你可以通过以下方式执行此操作：右键单击应用程序图标并指示需以管理员身份运行。  
  
 [!code-cpp[Classic PrincipalPermission Example#1](../../../samples/snippets/cpp/VS_Snippets_CLR_Classic/classic PrincipalPermission Example/CPP/source.cpp#1)]
 [!code-csharp[Classic PrincipalPermission Example#1](../../../samples/snippets/csharp/VS_Snippets_CLR_Classic/classic PrincipalPermission Example/CS/source.cs#1)]
 [!code-vb[Classic PrincipalPermission Example#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR_Classic/classic PrincipalPermission Example/VB/source.vb#1)]  
  
 下面的示例演示如何确定主体的标识和该主体可用的角色。 该示例的应用可能是：确认当前用户为你允许使用你的应用程序的角色。  
  
 [!code-cpp[System.Security.Principal.WindowsBuiltInRole Example#1](../../../samples/snippets/cpp/VS_Snippets_CLR_System/system.Security.Principal.WindowsBuiltInRole Example/CPP/source.cpp#1)]
 [!code-csharp[System.Security.Principal.WindowsBuiltInRole Example#1](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.Security.Principal.WindowsBuiltInRole Example/CS/source.cs#1)]
 [!code-vb[System.Security.Principal.WindowsBuiltInRole Example#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.Security.Principal.WindowsBuiltInRole Example/VB/source.vb#1)]  
  
## <a name="authorization"></a>授权  

授权是确定是否同意主体执行所请求的操作的过程。 授权发生在身份验证之后，并使用有关主体的标识和角色的信息来确定主体可以访问的资源。 您可以使用基于 .NET 角色的安全性实现授权。

## <a name="see-also"></a>另请参阅

- [ASP.NET Core 安全性](/aspnet/core/security/)
