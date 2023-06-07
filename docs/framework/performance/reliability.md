---
title: 可靠性
description: 了解 .NET 中的可靠性。 防范在 .NET 中执行的主机上的异步异常，如 SQL Server。
ms.date: 03/30/2017
helpviewer_keywords:
- SQL Server [.NET Framework]
- managed code, reliability
- reliability [.NET Framework]
- writing reliable code
- code, reliability
ms.assetid: 294aa306-0afe-4cbe-b397-86ba9f1860f8
ms.openlocfilehash: 00af439a12476addbe564ecf81152a1bc435d637
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96245596"
---
# <a name="reliability"></a>可靠性

在服务器环境（如 SQL Server）中执行的代码防止发生异步异常，这一点非常重要。 文本所讨论的可靠性并不是针对 SQL Server 而言，而是针对为在 .NET Framework 版本 2.0 环境中执行的任何主机编写可靠代码而言。 SQL Server 是第一个广泛使用版本 2.0 的新可靠性功能的服务，所以将其作为示例。  
  
 在 SQL Server 中运行的代码必须使用与其他服务器环境相比更严格的可靠性准则。 这是因为 SQL Server 在资源消耗方面的稳定操作。  <xref:System.OutOfMemoryException> 和 <xref:System.Threading.ThreadAbortException> 异常在 SQL Server 环境中比较常见。 这些准则通常较少强调可靠性，更多专注于允许完全信任的托管代码面对 <xref:System.AppDomain> 级别的回收温和地失败，这是服务器维持一致性和可用性的主要方法。  
  
## <a name="in-this-section"></a>本节内容  

 [SQL Server 编程和宿主保护特性](sql-server-programming-and-host-protection-attributes.md)  
 介绍 SQL Server 如何使用 <xref:System.Security.Permissions.HostProtectionAttribute> 属性限制托管代码的执行。  
  
 [可靠性最佳做法](reliability-best-practices.md)  
 提供用于编写符合可靠性要求的代码的准则。  
  
 [受约束的执行区域](constrained-execution-regions.md)  
 介绍受约束的执行区域 (CER) 的功能和行为。  
  
## <a name="reference"></a>引用  

 <xref:System.Security.Permissions.HostProtectionAttribute>  
  
 <xref:System.Security.Permissions.HostProtectionResource>
