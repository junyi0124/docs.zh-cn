---
ms.openlocfilehash: 7c4b9faf25853c1c7a546f06c329f6f153eef904
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90606750"
---
### <a name="changes-in-path-normalization"></a>路径规范化中的更改

#### <a name="details"></a>详细信息

从面向 .NET Framework 4.6.2 的应用开始，运行时规范路径的方式发生了变化。路径规范化涉及修改用于标识路径或文件的字符串，使其与目标操作系统上的有效路径一致。 路径规范化通常涉及以下操作：

- 规范化处理组件和目录分隔符。
- 将当前目录应用到相对路径。
- 评估路径中的相对目录 (.) 或父目录 (..)。
- 删减指定字符。
从面向 .NET Framework 4.6.2 的应用开始，在路径规范化中默认启用以下更改：
  - 运行时在规范化处理路径时以操作系统的 [GetFullPathName](/windows/desktop/api/fileapi/nf-fileapi-getfullpathnamew) 函数为准。
- 路径规范化再也不用删减目录部分的末尾内容（如目录名称末尾的空格）。
- 支持完全信任形式的设备路径语法，包括 `\\.\` 和 `\\?\`（对于 mscorlib.dll 中的文件 I/O API）。
- 运行时不会验证设备语法路径。
- 支持使用设备语法来访问备用数据流。
这些更改会提升性能，同时允许方法访问之前无法访问的路径。 定目标到 .NET Framework 4.6.1 及更低版本、但在 .NET Framework 4.6.2 或更高版本控制下运行的应用不受此更改影响。

#### <a name="suggestion"></a>建议

对于面向 .NET Framework 4.6.2 或更高版本的应用，可通过将以下内容添加到应用程序配置文件的 `<runtime>` 部分，选择弃用此更改而使用旧版规范化：

```xml
<runtime>
  <AppContextSwitchOverrides value="Switch.System.IO.UseLegacyPathHandling=true" />
</runtime>
```

对于面向 .NET Framework 4.6.1 及更低版本，但在 .NET Framework 4.6.2 或更高版本上运行的应用，可通过将以下行添加到应用程序配置文件的 `<runtime>` 部分，启用对路径规范化的更改：

```xml
<runtime>
  <AppContextSwitchOverrides value="Switch.System.IO.UseLegacyPathHandling=false" />
</runtime>
```

| “属性”    | “值”       |
|:--------|:------------|
| 范围   | 次要       |
| Version | 4.6.2       |
| 类型    | 重定目标 |
