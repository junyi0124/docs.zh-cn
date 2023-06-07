---
ms.openlocfilehash: 12ba3bd3c9e9e00b88cab0e568a1ce0f4f8bbb05
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90606876"
---
### <a name="cspparametersparentwindowhandle-now-expects-hwnd-value"></a>CspParameters.ParentWindowHandle 现在需要 HWND 值

#### <a name="details"></a>详细信息

借助 .NET Framework 2.0 中引入的 <xref:System.Security.Cryptography.CspParameters.ParentWindowHandle> 值，应用程序可以注册父窗口句柄值，这样任何需要访问密钥的 UI（如 PIN 提示或同意对话框）将会作为指定窗口的子模式打开。从面向 .NET Framework 4.7 的应用开始，Windows 窗体应用程序可使用如下所示的代码设置 <xref:System.Security.Cryptography.CspParameters.ParentWindowHandle> 属性：

```csharp
cspParameters.ParentWindowHandle = form.Handle;
```

在先前版本的 .NET Framework 中，该值应为 <xref:System.IntPtr?displayProperty=fullName>，表示内存中驻留 [HWND](/windows/desktop/WinProg/windows-data-types#HWND) 值的位置。 在 Windows 7 和更早版本上，将属性设置为 form.Handle 不会造成任何影响，但在 Windows 8 和更高版本中，此操作会导致“<xref:System.Security.Cryptography.CryptographicException?displayProperty=fullName>：参数不正确。”

#### <a name="suggestion"></a>建议

如果应用程序要注册父窗口关系且面向 .NET Framework 4.7 或更高版本，建议使用简易窗体：

```csharp
cspParameters.ParentWindowHandle = form.Handle;
```

如果用户已确定要传递的正确值是保留 `form.Handle` 值的内存位置地址，可通过将 AppContext 开关 `Switch.System.Security.Cryptography.DoNotAddrOfCspParentWindowHandle` 设置为 `true` 来选择弃用此行为更改：

- 以编程方式在 AppContext 上设置兼容性开关，如[此处](https://devblogs.microsoft.com/dotnet/net-announcements-at-build-2015/#dotnet46)所述。
- 在 app.config 文件的 `<runtime>` 部分中添加下面的代码行：

```xml
<runtime>
 <AppContextSwitchOverrides value="Switch.System.Security.Cryptography.DoNotAddrOfCspParentWindowHandle=true"/>
</runtime>
```

相反，如果用户希望在旧版 .NET Framework 中加载应用程序时选择启用 .NET Framework 4.7 运行时上的新行为，则可将 AppContext 开关设置为 `false`。

| “属性”    | “值”       |
|:--------|:------------|
| 范围   | 次要       |
| Version | 4.7         |
| 类型    | 重定目标 |

#### <a name="affected-apis"></a>受影响的 API

- <xref:System.Security.Cryptography.CspParameters.ParentWindowHandle?displayProperty=nameWithType>
