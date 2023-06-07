---
ms.openlocfilehash: c7500550cd9714a9788a7dea68af04823f000f7f
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/01/2020
ms.locfileid: "85614375"
---
### <a name="stack-traces-obtained-when-using-portable-pdbs-now-include-source-file-and-line-information-if-requested"></a>使用便携式 PDB 时获取的堆栈跟踪现在包括源文件和行信息（如果请求）

#### <a name="details"></a>详细信息

从 .NET Framework 4.7.2 开始，使用便携式 PDB 时获取的堆栈跟踪包括源文件和行信息（如果请求）。 在 .NET Framework 4.7.2 之前的版本中，即使已显式请求，使用便携式 PDB 时也不会提供源文件和行信息。

#### <a name="suggestion"></a>建议

对于面向 .NET Framework 4.7.2 的应用程序，如果不需要在使用便携式 PDB 时获取的源文件和行信息，可通过将以下内容添加到 `app.config` 文件的 `<runtime>` 部分，从而选择弃用：

```xml
<runtime>
  <AppContextSwitchOverrides value="Switch.System.Diagnostics.IgnorePortablePDBsInStackTraces=true" />
</runtime>
```

对于面向旧版 .NET Framework，但在 .NET Framework 4.7.2 或更高版本上运行的应用程序，可通过将以下内容添加到 `app.config` 文件的 `<runtime>` 部分，从而选择启用在使用便携式 PDB 时获取源文件和行信息：

```xml
<runtime>
  <AppContextSwitchOverrides value="Switch.System.Diagnostics.IgnorePortablePDBsInStackTraces=false" />
</runtime>
```

| “属性”    | “值”       |
|:--------|:------------|
| 范围   | 边缘        |
| Version | 4.7.2       |
| 类型    | 重定目标 |

#### <a name="affected-apis"></a>受影响的 API

- <xref:System.Diagnostics.StackTrace.%23ctor(System.Boolean)>
- <xref:System.Diagnostics.StackTrace.%23ctor(System.Exception,System.Boolean)>
- <xref:System.Diagnostics.StackTrace.%23ctor(System.Exception,System.Int32,System.Boolean)>
