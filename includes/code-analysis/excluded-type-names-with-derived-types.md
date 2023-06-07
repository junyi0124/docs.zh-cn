---
ms.openlocfilehash: 4125df1d64fe7f3f2eb1eb4a821ed46c8270c95f
ms.sourcegitcommit: d0990c1c1ab2f81908360f47eafa8db9aa165137
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/15/2020
ms.locfileid: "97531897"
---
### <a name="exclude-specific-types-and-their-derived-types"></a>排除特定类型及其派生类型

可以从分析中排除特定类型及其派生类型。 例如，若要指定规则不应在名为的类型中的任何方法 `MyType` 及其派生类型上运行，请将以下键-值对添加到项目中的 *editorconfig* 文件中：

```ini
dotnet_code_quality.CAXXXX.excluded_type_names_with_derived_types = MyType
```

选项值中允许的符号名称格式 (用 `|`) 分隔：

- 仅限类型名称 (包含名称的所有类型，而不管包含类型或命名空间) 。
- 符号 [文档 ID 格式](../../docs/csharp/programming-guide/xmldoc/processing-the-xml-file.md#id-strings)的完全限定名称，带有可选 `T:` 前缀。

示例:

| 选项值 | 总结 |
| --- | --- |
|`dotnet_code_quality.CAXXXX.excluded_type_names_with_derived_types = MyType` | 匹配名为的所有类型 `MyType` 及其所有派生类型。 |
|`dotnet_code_quality.CAXXXX.excluded_type_names_with_derived_types = MyType1|MyType2` | 匹配名为或及其 `MyType1` `MyType2` 所有派生类型的所有类型。 |
|`dotnet_code_quality.CAXXXX.excluded_type_names_with_derived_types = M:NS.MyType` | 匹配 `MyType` 具有给定完全限定名称及其所有派生类型的特定类型。 |
|`dotnet_code_quality.CAXXXX.excluded_type_names_with_derived_types = M:NS1.MyType1|M:NS2.MyType2` | 匹配特定的类型， `MyType1` 以及 `MyType2` 相应的完全限定名称及其所有派生类型。 |
