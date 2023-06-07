---
title: 通过部分受信任的代码使用库
description: 了解如何从部分受信任的代码使用库。 使用 AllowPartiallyTrustedCallersAttribute 属性调用共享的托管库。
ms.date: 03/30/2017
helpviewer_keywords:
- security [.NET Framework], partially trusted code
- partially trusted code
- partial trust
- AllowPartiallyTrustedCallersAttribute attribute
- code access security, partially trusted code
- APTCA
ms.assetid: dd66cd4c-b087-415f-9c3e-94e3a1835f74
ms.openlocfilehash: d3c75b4b0ab07efe46ffafbe185a686c7d5a7a45
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90556390"
---
# <a name="using-libraries-from-partially-trusted-code"></a>通过部分受信任的代码使用库

[!INCLUDE[net_security_note](../../../includes/net-security-note-md.md)]  
  
> [!NOTE]
> 本主题介绍强名称程序集的行为，并且仅适用于 [第1级](security-transparent-code-level-1.md) 程序集。 [安全透明代码、](security-transparent-code-level-2.md) .NET Framework 4 或更高版本中的级别2程序集不受强名称的影响。 有关安全系统更改的详细信息，请参阅 [安全更改](/previous-versions/dotnet/framework/security/security-changes)。  
  
 不允许未从其主机或沙盒获取完全信任的应用程序调用共享的托管库，除非库编写器明确允许它们使用 <xref:System.Security.AllowPartiallyTrustedCallersAttribute> 特性。 因此，应用程序编写器必须注意某些库在部分受信任的上下文中将不可用。 默认情况下，在部分信任 [沙箱](how-to-run-partially-trusted-code-in-a-sandbox.md) 中执行且不在完全信任程序集列表中的所有代码都是部分受信任的程序集。 如果不希望从部分受信任的上下文中执行或由部分受信任的代码调用自己的代码，则不必关心此部分的信息。 但是，如果所编写的代码必须与部分受信任的代码进行交互或从部分受信任的上下文环境进行操作，则应考虑以下因素：  
  
- 为了方便多个应用程序共享库，则必须使用强名称对库进行签名。 强名称允许你的代码可放入全局程序集缓存中或添加到沙盒处理 <xref:System.AppDomain> 的完全信任列表，并允许使用者验证实际来自你的移动代码的特定部分。  
  
- 默认情况下，强名称 [级别 1](security-transparent-code-level-1.md) 共享库自动执行完全信任的隐式 [LinkDemand](link-demands.md) ，而库编写器不需要执行任何操作。  
  
- 如果调用方不具有完全信任但仍尝试调用这样的库，则运行时会引发 <xref:System.Security.SecurityException>，且会不允许调用方链接到库。  
  
- 为了禁用自动 **LinkDemand** 并防止引发异常，你可以将 **AllowPartiallyTrustedCallersAttribute** 属性放置在共享库的程序集范围内。 此属性允许从部分受信任的托管代码调用你的库。  
  
- 被授予访问具有此属性的库的权限的部分受信任代码仍将受到由 <xref:System.AppDomain> 定义的进一步限制。  
  
- 部分受信任代码无法通过编程方式调用不具有 **AllowPartiallyTrustedCallersAttribute** 属性的库。  
  
 专用于特定应用程序的库不需要强名称或 **AllowPartiallyTrustedCallersAttribute** 属性，并且不能由应用程序外部的潜在恶意代码引用。 保护此类代码免受部分受信任的移动代码的有意或无意滥用，而无需开发人员进行任何额外操作。  
  
 你应该考虑显式启用以下代码类型的部分受信任代码的用法：  
  
- 已对安全漏洞进行了认真测试，并遵循 [安全编码准则](../../standard/security/secure-coding-guidelines.md)中所述的指导原则。  
  
- 专门为部分受信任的情况进行编写的强名称代码库。  
  
- 使用强名称签名的任何组件（无论是部分或完全受信任）都将由从 Internet 下载的代码调用。  
  
> [!NOTE]
> .NET Framework 类库中的某些类不具有 **AllowPartiallyTrustedCallersAttribute** 特性，因此不能由部分受信任的代码调用。  
  
## <a name="see-also"></a>请参阅

- [代码访问安全性](code-access-security.md)
