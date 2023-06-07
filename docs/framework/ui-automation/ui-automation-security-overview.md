---
title: UI 自动化安全性概述
description: 阅读有关 Microsoft UI 自动化安全模型的概述。 了解用户帐户控制、需要更高特权的任务和清单文件。
ms.date: 03/30/2017
helpviewer_keywords:
- UI Automation, security model
- security model, UI Automation
ms.assetid: 1d853695-973c-48ae-b382-4132ae702805
ms.openlocfilehash: d483f282db8ce8e5653d6d83361fa44df05f63f5
ms.sourcegitcommit: 87cfeb69226fef01acb17c56c86f978f4f4a13db
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/24/2020
ms.locfileid: "87163147"
---
# <a name="ui-automation-security-overview"></a>UI 自动化安全性概述

> [!NOTE]
> 本文档适用于想要使用 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 命名空间中定义的托管 <xref:System.Windows.Automation> 类的 .NET Framework 开发人员。 有关 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]的最新信息，请参阅 [Windows 自动化 API：UI 自动化](/windows/win32/winauto/entry-uiauto-win32)。

本概述介绍了 [!INCLUDE[TLA#tla_uiautomation](../../../includes/tlasharptla-uiautomation-md.md)] Windows Vista 中的安全模式。

<a name="User_Account_Control"></a>

## <a name="user-account-control"></a>用户帐户控制

安全性是 Windows Vista 的主要重点，而革新就是使用户能够以标准（非管理员）用户身份运行，而无需阻止运行需要更高特权的应用程序和服务。

在 Windows Vista 中，大多数应用程序都提供了标准或管理令牌。 如果无法将应用程序标识为管理应用程序，则默认情况下会作为标准应用程序启动。 在可以启动标识为管理的应用程序之前，Windows Vista 会提示用户同意以提升的身份运行应用程序。 即使用户是本地“Administrators”组的成员，也会在默认情况下显示同意提示，这是因为管理员以标准用户身份运行，直到需要管理凭据的应用程序或系统组件请求授予运行权限。

<a name="Tasks_Requiring_Higher_Privileges"></a>

## <a name="tasks-requiring-higher-privileges"></a>需要更高特权的任务

当用户尝试执行需要管理特权的任务时，Windows Vista 会显示一个对话框，要求用户同意继续。 此对话框受到跨进程通信保护，因此恶意软件不能模拟用户输入。 同样，其他进程通常无法访问桌面登录屏幕。

UI 自动化客户端必须与其他进程通信，其中某些进程可能正在以更高特权级别运行。 客户端还可能需要访问对其他进程通常不可视的系统对话框。 因此， [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 客户端必须受到系统信任并且必须使用特殊特权运行。

要获得信任以便与以更高特权级别运行的应用程序通信，必须签名应用程序。

<a name="Manifest_Files"></a>

## <a name="manifest-files"></a>清单文件

若要获取对受保护的系统 UI 的访问权限，必须使用包含标记中属性的清单文件生成应用程序 `uiAccess` `requestedExecutionLevel` ，如下所示：

```xml
<trustInfo xmlns="urn:schemas-microsoft-com:asm.v3">
  <security>
    <requestedPrivileges>
      <requestedExecutionLevel
        level="highestAvailable"
        uiAccess="true" />
    </requestedPrivileges>
  </security>
</trustInfo>
```

此代码中的 `level` 属性值只是一个示例。

`uiAccess`默认为 "false";也就是说，如果省略该属性，或如果没有程序集清单，则应用程序将无法访问受保护的 UI。
