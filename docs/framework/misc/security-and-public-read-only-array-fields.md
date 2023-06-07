---
title: 安全和公共只读数组字段
description: 请阅读为什么应避免使用公共只读数组字段来定义应用程序的边界行为或安全性。
ms.date: 03/30/2017
helpviewer_keywords:
- security [.NET Framework], public read-only array fields
ms.assetid: 3df28dee-2a9f-40ff-9852-bfdbe59c27f3
ms.openlocfilehash: 5e499f8052306cd1ad063c9f44a2a0f1d0b365ef
ms.sourcegitcommit: c37e8d4642fef647ebab0e1c618ecc29ddfe2a0f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87855733"
---
# <a name="security-and-public-read-only-array-fields"></a>安全和公共只读数组字段

[!INCLUDE[net_security_note](../../../includes/net-security-note-md.md)]

永远不要使用托管库中的只读公共数组字段来定义应用程序的边界行为或安全性，因为可以修改只读公共数组字段。  
  
## <a name="remarks"></a>备注  

某些 .NET 类包含包含特定于平台的边界参数的只读公共字段。 例如， <xref:System.IO.Path.InvalidPathChars> 字段是描述文件路径字符串中不允许使用的字符的数组。 许多类似的字段在 .NET 中都存在。  
  
 类似公共只读字段的值 <xref:System.IO.Path.InvalidPathChars> 可以通过代码或共享代码应用程序域的代码进行修改。  不应使用类似于这样的只读公共数组字段来定义应用程序的边界行为。  如果这样做，恶意代码可以更改边界定义，并以意外的方式使用您的代码。  
  
 在 .NET Framework 版本2.0 及更高版本中，应使用返回新数组的方法，而不是使用公共数组字段。  例如，应使用方法，而不是使用 <xref:System.IO.Path.InvalidPathChars> 字段 <xref:System.IO.Path.GetInvalidPathChars%2A> 。  
  
 请注意，.NET Framework 类型不使用公共字段在内部定义边界类型。  相反，.NET Framework 使用单独的私有字段。  更改这些公共字段的值不会改变 .NET Framework 类型的行为。  
  
## <a name="see-also"></a>另请参阅

- [安全编码准则](../../standard/security/secure-coding-guidelines.md)
