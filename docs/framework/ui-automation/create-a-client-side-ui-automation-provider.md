---
title: 创建客户端 UI 自动化提供程序
description: 查看有关如何创建客户端 UI 自动化提供程序的示例。 该示例为控制台窗口实现了一个简单的客户端提供程序。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- UI Automation, creating client-side provider
- client-side UI Automation provider, creating
ms.assetid: d91edaf2-be28-41ec-a508-af421cb43c3d
ms.openlocfilehash: 17a6deca2efc67d1409e89f7accf7a3b89a27807
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96276537"
---
# <a name="create-a-client-side-ui-automation-provider"></a>创建客户端 UI 自动化提供程序

> [!NOTE]
> 本文档适用于想要使用 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 命名空间中定义的托管 <xref:System.Windows.Automation> 类的 .NET Framework 开发人员。 有关 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]的最新信息，请参阅 [Windows 自动化 API：UI 自动化](/windows/win32/winauto/entry-uiauto-win32)。  
  
 本主题所包含的代码示例演示如何实现客户端 UI 自动化提供程序。  
  
## <a name="example"></a>示例  

 下面的示例代码可内置于动态链接库 (DLL) ，该动态链接库为控制台窗口实现非常简单的客户端提供程序。 此代码不具有任何有用的功能，只是用于演示设置提供程序程序集的基本步骤，该程序集可由 UI 自动化客户端应用程序进行注册。  
  
 [!code-csharp[UIAClientSideProvider_snip#101](../../../samples/snippets/csharp/VS_Snippets_Wpf/UIAClientSideProvider_snip/CSharp/CSProviderProgram.cs#101)]
 [!code-vb[UIAClientSideProvider_snip#101](../../../samples/snippets/visualbasic/VS_Snippets_Wpf/UIAClientSideProvider_snip/visualbasic/csproviderprogram.vb#101)]  
  
## <a name="see-also"></a>另请参阅

- [UI 自动化提供程序概述](ui-automation-providers-overview.md)
- [注册客户端提供程序程序集](register-a-client-side-provider-assembly.md)
