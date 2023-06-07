---
title: NETSDK1079：面向 .NET Core 3.0 或更高版本时，不支持 Microsoft.AspNetCore.All 包。
description: 如何解决从 Microsoft.AspNetCore.App 迁移的问题
author: dsplaisted
ms.topic: error-reference
ms.date: 10/09/2020
f1_keywords:
- NETSDK1079
ms.openlocfilehash: e0b7aba909dbd2931f755238a2de2418d294751d
ms.sourcegitcommit: 30a686fd4377fe6472aa04e215c0de711bc1c322
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2020
ms.locfileid: "94445671"
---
# <a name="netsdk1079-the-microsoftaspnetcoreall-package-is-not-supported-when-targeting-net-core-30-or-higher"></a>NETSDK1079：面向 .NET Core 3.0 或更高版本时，不支持 Microsoft.AspNetCore.All 包。

本文适用于：✔️ .NET Core SDK 3.1.100 及更高版本

在以下情况下，你可能会收到此错误消息：

- 将 ASP.NET Core 项目从 .NET Core 2.2 或更早版本重定向到 .NET Core 3.0 或更高版本。
- 该项目使用 Microsoft.AspNetCore.All 包。

已删除 Microsoft.AspNetCore.All 的 `PackageReference`。  你可能还需要为从 Microsoft.AspNetCore.All 引用但不包含在 ASP.NET Core 共享框架中的包添加包引用。  此处列出了这些包：[从 Microsoft.AspNetCore.All 迁移到 Microsoft.AspNetCore.App](/aspnet/core/fundamentals/metapackage#migrating-from-microsoftaspnetcoreall-to-microsoftaspnetcoreapp)。

另请参阅[从 ASP.NET Core 2.2 迁移到 3.0](/aspnet/core/migration/22-to-30)
