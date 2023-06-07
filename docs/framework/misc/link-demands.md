---
title: 链接需求
description: 阅读有关链接要求的信息，这会导致实时 (JIT) 编译期间进行安全检查，并仅检查代码的直接调用程序集。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- security [.NET Framework], demands
- demanded permissions
- permissions [.NET Framework], demands
- granting permissions, demands
- code access security, demands
- user demands for permission
- caller security checks
- link demands
ms.assetid: a33fd5f9-2de9-4653-a4f0-d9df25082c4d
ms.openlocfilehash: 7f5475d5bfff8cc3c500f95b05d54daacc9b253e
ms.sourcegitcommit: c37e8d4642fef647ebab0e1c618ecc29ddfe2a0f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87855785"
---
# <a name="link-demands"></a>链接需求

[!INCLUDE[net_security_note](../../../includes/net-security-note-md.md)]  
  
 链接要求导致在实时编译过程中进行安全检查，并且只检查代码的直接调用程序集。 当代码绑定到类型引用（包括函数指针引用和方法调用）时发生链接。 如果调用程序集的权限不足以链接到代码，则加载并运行代码时将不允许该链接且将引发运行时异常。 可在继承自代码的类中重写链接要求。  
  
 对于这种类型的要求，不会执行完整的堆栈审核，并且你的代码仍容易遭受引诱攻击。 例如，如果程序集 A 中的方法受链接要求保护，则将根据程序集 B 的权限计算程序集 B 中的直接调用方。 但是，如果链接要求使用程序集 B 中的方法间接调用程序集 A 中的方法，则链接要求将不会计算程序集 C 中的方法。链接要求仅指定直接调用程序集中的直接调用方必须有权链接到您的代码。 而不指定所有调用方为了运行你的代码所必须拥有的权限。  
  
 <xref:System.Security.CodeAccessPermission.Assert%2A>、<xref:System.Security.CodeAccessPermission.Deny%2A> 和 <xref:System.Security.CodeAccessPermission.PermitOnly%2A> 堆栈审核修饰符不影响链接要求计算。  由于链接要求不执行堆栈审核，所以堆栈审核修饰符对链接要求并无影响。  
  
 如果通过[反射](../reflection-and-codedom/reflection.md)访问受链接要求保护的方法，则链接要求会检查通过反射访问的代码的直接调用方。 对于使用反射执行的方法发现和方法调用都是如此。 例如，假设代码使用反射来返回一个 <xref:System.Reflection.MethodInfo> 对象，该对象表示受链接要求保护的方法，然后将该**MethodInfo**对象传递到其他使用该对象调用原始方法的代码。 在这种情况下，链接请求检查发生两次：一次针对返回**MethodInfo**对象的代码，一次针对调用它的代码。  
  
> [!NOTE]
> 在静态类构造函数上执行的链接要求不保护构造函数，因为静态构造函数是在应用程序的代码执行路径外部由系统调用的。 因此，当链接要求应用于整个类时，它不能保护对静态构造函数的访问，尽管它确实保护类的其余部分。  
  
 下面的代码片段以声明方式指定链接到 `ReadData` 方法的任何代码必须具有 `CustomPermission` 权限。 此权限是假设的自定义权限，在 .NET Framework 中并不存在。 请求是通过将**SecurityAction**标志传递到来实现的 `CustomPermissionAttribute` 。  
  
```vb  
<CustomPermissionAttribute(SecurityAction.LinkDemand)> _  
Public Shared Function ReadData() As String  
    ' Access a custom resource.  
End Function
```  
  
```csharp  
[CustomPermissionAttribute(SecurityAction.LinkDemand)]  
public static string ReadData()  
{  
    // Access a custom resource.  
}  
```  
  
## <a name="see-also"></a>请参阅

- [特性](../../standard/attributes/index.md)
- [代码访问安全性](code-access-security.md)
