---
ms.openlocfilehash: 72d48d1daa85b6891c122f2fcc5279642253b926
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/01/2020
ms.locfileid: "85614387"
---
### <a name="workflow-checksums-changed-from-md5-to-sha1"></a>工作流校验和已从 MD5 更改为 SHA1

#### <a name="details"></a>详细信息

为支持使用 Visual Studio 进行调试，工作流运行时使用哈希算法为工作流实例生成校验和。 在 .NET Framework 4.6.2 和早期版本中，工作流校验和哈希使用 MD5 算法，这会在启用 FIPS 的系统上导致问题。 从 .NET Framework 4.7 开始，算法为 SHA1。 如果代码保留了这些校验和，它们将是不兼容的。

#### <a name="suggestion"></a>建议

如果代码由于校验和失败而无法加载工作流实例，请尝试将 `AppContext` 开关 &quot;Switch.System.Activities.UseMD5ForWFDebugger&quot; 设置为 true。在代码中：

```csharp
System.AppContext.SetSwitch("Switch.System.Activities.UseMD5ForWFDebugger", true);
```

或在配置中：

```xml
<configuration>
  <runtime>
    <AppContextSwitchOverrides value="Switch.System.Activities.UseMD5ForWFDebugger=true" />
  </runtime>
</configuration>
```

| “属性”    | “值”       |
|:--------|:------------|
| 范围   | 次要       |
| Version | 4.7         |
| 类型    | 重定目标 |
