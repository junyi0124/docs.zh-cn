---
title: 使用应用程序域
description: 使用应用程序域为公共语言运行时 (CLR) 提供隔离单元。 应用程序域在进程中创建和运行。
ms.date: 03/30/2017
helpviewer_keywords:
- application domains, about
- common language runtime, application domains
- runtime, application domains
ms.assetid: c6d99815-e022-4d2c-9420-1d7ab5b9d504
ms.openlocfilehash: 36bfc60a55c8b0374a7e542b590aa7c030ceaed6
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96272262"
---
# <a name="using-application-domains"></a>使用应用程序域

应用程序域为公共语言运行时提供隔离单元。 它们在进程中创建和运行。 应用程序域通常由运行时主机创建，运行时主机是一种应用程序，负责向进程加载运行时，并在应用程序域内执行用户代码。 运行时主机创建进程和默认应用程序域，并在其中运行托管代码。 运行时主机包括 ASP.NET、Microsoft Internet Explorer 和 Windows Shell。  
  
对大多数应用程序而言，你无需创建自己的应用程序域，运行时主机将为你创建任何所需的应用程序域。 但是，如果应用程序需要隔离代码或使用和卸载 DLL，则可以创建和配置额外的应用程序域。  
  
## <a name="in-this-section"></a>本节内容  

[如何：创建应用程序域](how-to-create-an-application-domain.md)  
说明如何以编程方式创建应用程序域。  
  
[如何：卸载应用程序域](how-to-unload-an-application-domain.md)  
说明如何以编程方式卸载应用程序域。  
  
[如何：配置应用程序域](how-to-configure-an-application-domain.md)  
提供关于配置应用程序域的简介。  
  
[从应用程序域中检索安装信息](retrieve-setup-information.md)  
说明如何从应用程序域检索安装信息。  
  
[如何：将程序集加载到应用程序域中](how-to-load-assemblies-into-an-application-domain.md)  
说明如何将程序集加载到应用程序域中。  
  
[如何：从程序集获得类型和成员信息](../reflection-and-codedom/get-type-member-information.md)  
说明如何检索关于程序集的信息。  
  
[卷影复制程序集](shadow-copy-assemblies.md)  
说明卷影复制如何允许对正在使用的程序集进行更新，以及如何配置卷影复制。  
  
[如何：接收第一机会异常通知](how-to-receive-first-chance-exception-notifications.md)  
说明在公共语言运行时开始搜索异常处理程序之前，可如何接收已引发异常的通知。  
  
[解析程序集加载](../../standard/assembly/resolve-loads.md)  
提供有关使用 <xref:System.AppDomain.AssemblyResolve?displayProperty=nameWithType> 事件处理程序集加载故障的指导。  
  
## <a name="reference"></a>参考  

<xref:System.AppDomain>  
表示应用程序域。 提供用于创建和控制应用程序域的方法。  
  
## <a name="related-sections"></a>相关章节  

[.NET 中的程序集](../../standard/assembly/index.md)  
概述程序集执行的功能。  
  
[使用程序集编程](../../standard/assembly/index.md)  
描述如何在程序集上创建、签署和设置特性。  
  
[发出动态方法和程序集](../reflection-and-codedom/emitting-dynamic-methods-and-assemblies.md)  
描述如何创建动态程序集。  
  
[应用程序域](application-domains.md)  
提供应用程序域的概念性概述。  
  
[反射概述](../reflection-and-codedom/reflection.md)  
介绍了如何使用 **Reflection** 类获取程序集的信息。
