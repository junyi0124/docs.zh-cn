---
ms.openlocfilehash: 83644b9205d650da68c910095dd1d22a14940c44
ms.sourcegitcommit: 9b877e160c326577e8aa5ead22a937110d80fa44
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/11/2020
ms.locfileid: "97366847"
---
### <a name="exclude-specific-symbols"></a>排除特定符号

可以从分析中排除特定符号，如类型和方法。 例如，若要指定规则不应在任何名为的类型中的任何代码上运行 `MyType` ，请将以下键-值对添加到项目中的 *editorconfig* 文件中：

```ini
dotnet_code_quality.CAXXXX.excluded_symbol_names = MyType
```

选项值中允许的符号名称格式 (用 `|`) 分隔：

- 仅符号名称 (包含名称的所有符号，而不管包含类型或命名空间) 。
- 符号 [文档 ID 格式](../../docs/csharp/programming-guide/xmldoc/processing-the-xml-file.md#id-strings)的完全限定名称。 每个符号名称都需要符号类型前缀，如 `M:` 用于方法、 `T:` 类型的和 `N:` 命名空间。
- `.ctor` 对于构造函数和 `.cctor` 静态构造函数。

示例：

| 选项值 | 总结 |
| --- | --- |
|`dotnet_code_quality.CAXXXX.excluded_symbol_names = MyType` | 匹配名为的所有符号 `MyType` 。 |
|`dotnet_code_quality.CAXXXX.excluded_symbol_names = MyType1|MyType2` | 匹配名为 `MyType1` 或的所有符号 `MyType2` 。 |
|`dotnet_code_quality.CAXXXX.excluded_symbol_names = M:NS.MyType.MyMethod(ParamType)` | 将特定方法 `MyMethod` 与指定的完全限定的签名相匹配。 |
|`dotnet_code_quality.CAXXXX.excluded_symbol_names = M:NS1.MyType1.MyMethod1(ParamType)|M:NS2.MyType2.MyMethod2(ParamType)` | 将特定方法 `MyMethod1` 与 `MyMethod2` 相应的完全限定的签名相匹配。 |
