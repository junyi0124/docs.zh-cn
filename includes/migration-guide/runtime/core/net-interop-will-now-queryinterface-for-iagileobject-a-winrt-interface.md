---
ms.openlocfilehash: 65fa5d8629ce8e426cf1623590a056e5cad0b91f
ms.sourcegitcommit: cbacb5d2cebbf044547f6af6e74a9de866800985
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/05/2020
ms.locfileid: "89497433"
---
### <a name="net-interop-will-now-queryinterface-for-iagileobject-a-winrt-interface"></a>.NET Interop 现将为 IAgileObject 的 QueryInterface（WinRT 接口）

#### <a name="details"></a>详细信息

自 .NET Framework 4.8 起，使用带有 .NET 委托的 WinRT 事件时，Windows 将为 IAgileObject 进行 QI。  在以前版本的 .NET Framework 中，运行时进行该 QI 将失败，并且无法订阅事件。<ul><li>[x] Quirked</li></ul>

#### <a name="suggestion"></a>建议

如果为 IAgileObject 启用 QI 会中断执行，则可以通过设置以下配置来禁用此代码。 <h4>方法 1：环境变量</h4> 设置以下环境变量：COMPLUS_DisableCCWSupportIAgileObject=1 此方法会影响任何继承此环境变量的环境。 这可能只是一个控制台会话，如果在全局范围内设置环境变量，它可能会影响整个计算机。 环境变量名称不区分大小写。 <h4>方法 2：注册表</h4> 使用注册表编辑器 (regedit.exe)，查找以下某个子项：HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft.NETFramework HKEY_CURRENT_USER\SOFTWARE\Microsoft.NETFramework 然后添加以下内容：值名称：DisableCCWSupportIAgileObject 类型：DWORD（32 位）值（也称为 REG_WORD）值：1 可以使用 Windows REG.EXE 工具从命令行或脚本环境添加此值。 例如：<pre><code class="lang-console">reg add HKLM\SOFTWARE\Microsoft\.NETFramework /v DisableCCWSupportIAgileObject /t REG_DWORD /d 1&#13;&#10;</code></pre>在这种情况下，使用 <code>HKLM</code> 而不使用 <code>HKEY_LOCAL_MACHINE</code>。 使用 <code>reg add /?</code> 查看有关此语法的帮助内容。 注册表值名称不区分大小写。

| “属性”    | “值”       |
|:--------|:------------|
| 范围   |边缘|
|Version|4.8|
|类型|运行时|

#### <a name="affected-apis"></a>受影响的 API

无法通过 API 分析检测到。

<!--

#### Affected APIs

Not detectable via API analysis.

-->
