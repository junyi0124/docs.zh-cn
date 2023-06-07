---
title: 请查看安装的 .NET Framework 安全更新和修补程序
description: 了解如何确定计算机上安装了哪些 .NET Framework 安全更新和修补程序。
ms.date: 11/27/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- updates, determining for .NET Framework
- .NET Framework, determining updates
ms.assetid: 53c7b5f7-d47a-402a-b194-7244a696a88b
ms.openlocfilehash: 5c7bf48d5786530a9bcb69fb7cf605ac2c80a4eb
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2020
ms.locfileid: "79181267"
---
# <a name="how-to-determine-which-net-framework-security-updates-and-hotfixes-are-installed"></a>如何确定安装了哪些 .NET Framework 安全更新和修补程序

本文介绍如何找出计算机上安装了哪些 .NET Framework 安全更新和修补程序。

> [!NOTE]
> 本文中介绍的所有方法均需要具有管理权限的帐户。

## <a name="use-registry-editor"></a>使用注册表编辑器

安装在计算机上的各个版本 .NET Framework 的已安装安全更新和修补程序都列在 Windows 注册表中。 可以使用注册表编辑器 (regedit.exe) 程序查看此信息  。

1. 打开程序 **regedit.exe**。 在 Windows 8 和更高版本中，右键单击“开始”  ![Windows 键徽标的屏幕截图](./media/how-to-determine-which-net-framework-updates-are-installed/windows-keyboard-logo.png "Windowskeyboardlogo")，然后选择“运行”  。 在“打开”对话框中，输入“regedit.exe”并选择“确定”    。

2. 在注册表编辑器中，打开以下子项：

     **HKEY_LOCAL_MACHINE\SOFTWARE\Wow6432Node\Microsoft\Updates**

     已安装的更新在标识适用的 .NET Framework 版本的子项下方列出。 每个更新均由知识库 (KB) 编号进行标识。

在注册表编辑器中，.NET Framework 版本和每个版本已安装的更新都保存在不同的子项中。 若要了解如何检测已安装的版本号，请参阅[如何：确定安装了哪些 .NET Framework 版本](how-to-determine-which-versions-are-installed.md)。

## <a name="query-the-registry-using-code"></a>使用代码查询注册表

以下示例以编程方式确定已安装在计算机上的 .NET Framework 安全更新和修补程序：

[!code-csharp[ListUpdates](../../../samples/snippets/csharp/VS_Snippets_CLR/listupdates/cs/program.cs)]
[!code-vb[ListUpdates](../../../samples/snippets/visualbasic/VS_Snippets_CLR/listupdates/vb/program.vb)]

该示例生成类似下面内容的输出：

```console
Microsoft .NET Framework 4 Client Profile
  KB2468871
  KB2468871v2
  KB2478063
  KB2533523
  KB2544514
  KB2600211
  KB2600217
Microsoft .NET Framework 4 Extended
  KB2468871
  KB2468871v2
  KB2478063
  KB2533523
  KB2544514
  KB2600211
  KB2600217
```

## <a name="use-powershell-to-query-the-registry"></a>使用 PowerShell 查询注册表

以下示例介绍如何使用 PowerShell 确定已安装在计算机上的 .NET Framework 安全更新和修补程序：

```powershell
$DotNetVersions = Get-ChildItem HKLM:\SOFTWARE\WOW6432Node\Microsoft\Updates | Where-Object {$_.name -like
 "*.NET Framework*"}

ForEach($Version in $DotNetVersions){

   $Updates = Get-ChildItem $Version.PSPath
    $Version.PSChildName
    ForEach ($Update in $Updates){
       $Update.PSChildName
       }
}
```

该示例生成类似下面内容的输出：

```console
Microsoft .NET Framework 4 Client Profile
KB2468871
KB2468871v2
KB2478063
KB2533523
KB2544514
KB2600211
KB2600217
Microsoft .NET Framework 4 Extended
KB2468871
KB2468871v2
KB2478063
KB2533523
KB2544514
KB2600211
KB2600217
```

## <a name="see-also"></a>另请参阅

- [如何：确定安装了哪些 .NET Framework 版本](how-to-determine-which-versions-are-installed.md)
- [安装面向开发人员的 .NET Framework](../install/guide-for-developers.md)
- [版本和依赖关系](versions-and-dependencies.md)
