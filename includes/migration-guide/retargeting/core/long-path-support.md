---
ms.openlocfilehash: 67e3ff5000ebd38064ed8a57e4fe561afa31f8d8
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/01/2020
ms.locfileid: "85614347"
---
### <a name="long-path-support"></a>长路径支持

#### <a name="details"></a>详细信息

从面向 .NET Framework 4.6.2 的应用开始，支持长路径（最多 32K 个字符），并删除了 260 个字符（或 `MAX_PATH`）的路径长度限制。对于经过重新编译以面向 .NET Framework 4.6.2 的应用，之前因路径超过 260 个字符而引发 <xref:System.IO.PathTooLongException?displayProperty=fullName> 的代码路径，现在仅在以下情况下引发 <xref:System.IO.PathTooLongException?displayProperty=fullName>：

- 路径长度必须大于 <xref:System.Int16.MaxValue> (32,767) 个字符。
- 操作系统返回 `COR_E_PATHTOOLONG` 或其等同项。
对于面向 .NET Framework 4.6.1 及更早版本的应用，只要路径超过 260 个字符，运行时就会自动引发 <xref:System.IO.PathTooLongException?displayProperty=fullName>。

#### <a name="suggestion"></a>建议

对于面向 .NET Framework 4.6.2 的应用，如果无需长路径支持，可通过将以下内容添加到 `app.config` 文件的 `<runtime>` 部分来选择弃用该支持：

```xml
<runtime>
  <AppContextSwitchOverrides value="Switch.System.IO.BlockLongPaths=true" />
</runtime>
```

对于面向旧版 .NET Framework，但在 .NET Framework 4.6.2 或更高版本上运行的应用，可通过将以下内容添加到 `app.config` 文件的 `<runtime>` 部分来选择启用长路径支持：

```xml
<runtime>
  <AppContextSwitchOverrides value="Switch.System.IO.BlockLongPaths=false" />
</runtime>
```

| “属性”    | “值”       |
|:--------|:------------|
| 范围   | 次要       |
| Version | 4.6.2       |
| 类型    | 重定目标 |
