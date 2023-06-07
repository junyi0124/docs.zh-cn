---
ms.openlocfilehash: 946e71f2f466664c8f9fcf4811288ce693a872eb
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/01/2020
ms.locfileid: "85617786"
---
### <a name="workflow-xaml-checksums-for-symbols-changed-from-sha1-to-sha256"></a>符号的工作流 XAML 校验和从 SHA1 更改为 SHA256

#### <a name="details"></a>详细信息

为支持使用 Visual Studio 进行调试，工作流运行时使用哈希算法生成工作流 XAML 文件的校验和。 在 .NET Framework 4.6.2 和早期版本中，工作流校验和哈希使用 MD5 算法，这会在启用 FIPS 的系统上导致问题。 从 .NET Framework 4.7 开始，默认算法已更改为 SHA1。 从 .NET Framework 4.8 开始，默认算法已更改为 SHA256。

#### <a name="suggestion"></a>建议

如果由于校验和失败，代码无法加载工作流实例或找不到相应符号，请尝试将 `AppContext` 开关“Switch.System.Activities.UseSHA1HashForDebuggerSymbols”设置为 `true`。 在代码中：

```csharp
System.AppContext.SetSwitch("Switch.System.Activities.UseSHA1HashForDebuggerSymbols", true);
```

或在配置中：

```xml
<configuration>
  <runtime>
    <AppContextSwitchOverrides value="Switch.System.Activities.UseSHA1HashForDebuggerSymbols=true" />
  </runtime>
</configuration>
```

| “属性”    | “值”       |
|:--------|:------------|
| 范围   | 次要       |
| Version | 4.8         |
| 类型    | 重定目标 |
