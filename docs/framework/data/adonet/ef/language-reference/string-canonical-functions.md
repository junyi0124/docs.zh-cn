---
title: 字符串规范函数
ms.date: 03/30/2017
ms.assetid: 5e2cbebd-5df3-47c7-b0e2-49a17ab22bfb
ms.openlocfilehash: 5a7889511d9e8e276ba026c37f4cd8a4aeba156c
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91173648"
---
# <a name="string-canonical-functions"></a>字符串规范函数

[!INCLUDE[esql](../../../../../../includes/esql-md.md)] 包含字符串规范函数。  
  
## <a name="remarks"></a>备注  

 下表列出了字符串 [!INCLUDE[esql](../../../../../../includes/esql-md.md)] 规范函数。  
  
|函数|描述|  
|--------------|-----------------|  
|`Concat(string1, string2)`|返回包含追加了 `string2` 的 `string1` 的字符串。<br /><br /> **参数**<br /><br /> `string1`：将 `string2` 追加到其后的字符串。<br /><br /> `string2`：追加到 `string1` 之后的字符串。<br /><br /> **返回值**<br /><br /> `String`。 如果返回值字符串的长度大于允许的最大长度，则发生错误。<br /><br /> **示例**<br /><br /> `-- The following example returns abcxyz.`<br /><br /> `Concat('abc', 'xyz')`|  
|`Contains(string, target)`|如果 `true` 包含在 `target` 中，则返回 `string`。<br /><br /> **参数**<br /><br /> `string`：在其中进行搜索的字符串。<br /><br /> `target`：所搜索的目标字符串。<br /><br /> **返回值**<br /><br /> 如果 `true` 包含在 `target` 中，则为 `string`；否则为 `false`。<br /><br /> **示例**<br /><br /> `-- The following example returns true.`<br /><br /> `Contains('abc', 'bc')`|  
|`EndsWith(string, target)`|如果 `true` 以 `target` 结尾，则返回 `string`。<br /><br /> **参数**<br /><br /> `string`：在其中进行搜索的字符串。<br /><br /> `target`：在 `string` 末尾搜索的目标字符串。<br /><br /> **返回值**<br /><br /> 如果 `True` 以 `string` 结尾，则返回 `target`；否则返回 `false`。<br /><br /> **示例**<br /><br /> `-- The following example returns true.`<br /><br /> `EndsWith('abc', 'bc')`**注意：** 如果使用的是 SQL Server 数据访问接口，则此函数将返回， `false` 前提是该字符串存储在固定长度字符串列中且 `target` 为常量。 在这种情况下，将搜索整个字符串，包括任何填充尾随空格。 一种可能的解决办法是将数据裁剪为固定长度字符串，如下面的示例中所示：`EndsWith(TRIM(string), target)`|  
|`IndexOf(target, string)`|返回 `target` 在 `string` 中的位置，如果没找到则返回 0。 返回 1 指示 `string` 的起始位置。 索引号从 1 开始。<br /><br /> **参数**<br /><br /> `target`：要搜索的字符串。<br /><br /> `string`：在其中进行搜索的字符串。<br /><br /> **返回值**<br /><br /> 一个 `Int32`。<br /><br /> **示例**<br /><br /> `-- The following example returns 4.`<br /><br /> `IndexOf('xyz', 'abcxyz')`|  
|`Left(string, length)`|返回 `length` 左侧开始的前 `string` 个字符。 如果 `string` 的长度小于 `length`，则返回整个字符串。<br /><br /> **参数**<br /><br /> `string`：`String`。<br /><br /> `length`：`Int16`、`Int32`、`Int64` 或 `Byte`。 `length` 不能小于零。<br /><br /> **返回值**<br /><br /> `String`。<br /><br /> **示例**<br /><br /> `-- The following example returns abc.`<br /><br /> `Left('abcxyz', 3)`|  
|`Length(string)`|返回字符串的 (`Int32`) 长度，以字符为单位。<br /><br /> **参数**<br /><br /> `string`：`String`。<br /><br /> **返回值**<br /><br /> 一个 `Int32`。<br /><br /> **示例**<br /><br /> `-- The following example returns 6.`<br /><br /> `Legth('abcxyz')`|  
|`LTrim(string)`|返回 `string` 且不带前导空格。<br /><br /> **参数**<br /><br /> `String`。<br /><br /> **返回值**<br /><br /> `String`。<br /><br /> **示例**<br /><br /> `-- The following example returns abc.`<br /><br /> `LTrim('   abc')`|  
|`Replace(string1, string2, string3)`|返回 `string1`，其中所有 `string2` 都替换为 `string3`。<br /><br /> **参数**<br /><br /> `String`。<br /><br /> **返回值**<br /><br /> `String`。<br /><br /> **示例**<br /><br /> `-- The following example returns abcxyz.`<br /><br /> `Concat('abc', 'xyz')`|  
|`Reverse(string)`|返回反转字符顺序的 `string`。<br /><br /> **参数**<br /><br /> `String`。<br /><br /> **返回值**<br /><br /> `String`。<br /><br /> **示例**<br /><br /> `-- The following example returns dcba.`<br /><br /> `Reverse('abcd')`|  
|`Right(string, length)`|返回中的最后一个 `length` 字符 `string` 。 如果 `string` 的长度小于 `length`，则返回整个字符串。<br /><br /> **参数**<br /><br /> `string`：`String`。<br /><br /> `length`：`Int16`、`Int32`、`Int64` 或 `Byte`。 `length` 不能小于零。<br /><br /> **返回值**<br /><br /> `String`。<br /><br /> **示例**<br /><br /> `-- The following example returns xyz.`<br /><br /> `Right('abcxyz', 3)`|  
|`RTrim(string)`|返回 `string` ，不包含尾随空格。<br /><br /> **参数**<br /><br /> `String`。<br /><br /> **返回值**<br /><br /> `String`。|  
|`Substring(string, start, length)`|返回字符串的从 `start` 位置开始、长度为 `length` 个字符的子字符串。 start 为 1 指示字符串的第一个字符。 索引号从 1 开始。<br /><br /> **参数**<br /><br /> `string`：`String`。<br /><br /> `start`：`Int16`、`Int32`、`Int64` 和 `Byte`。 `start` 不能小于一。<br /><br /> `length`：`Int16`、`Int32`、`Int64` 和 `Byte`。 `length` 不能小于零。<br /><br /> **返回值**<br /><br /> `String`。<br /><br /> **示例**<br /><br /> `-- The following example returns xyz.`<br /><br /> `Substring('abcxyz', 4, 3)`|  
|`StartsWith(string, target)`|如果 `true` 以 `string` 开头，则返回 `target`。<br /><br /> **参数**<br /><br /> `string`：在其中进行搜索的字符串。<br /><br /> `target`：在 `string` 开头搜索的目标字符串。<br /><br /> **返回值**<br /><br /> 如果 `True` 以 `string` 开头，则返回 `target`；否则返回 `false`。<br /><br /> **示例**<br /><br /> `-- The following example returns true.`<br /><br /> `StartsWith('abc', 'ab')`|  
|`ToLower(string)`|返回全部大写字符都转换为小写字符的 `string`。<br /><br /> **参数**<br /><br /> `String`。<br /><br /> **返回值**<br /><br /> `String`。<br /><br /> **示例**<br /><br /> `-- The following example returns abc.`<br /><br /> `ToLower('ABC')`|  
|`ToUpper(string)`|返回全部小写字符都转换为大写字符的 `string`。<br /><br /> **参数**<br /><br /> `String`。<br /><br /> **返回值**<br /><br /> `String`。<br /><br /> **示例**<br /><br /> `-- The following example returns ABC.`<br /><br /> `ToUpper('abc')`|  
|`Trim(string)`|返回时 `string` 不带前导空格和尾随空格。<br /><br /> **参数**<br /><br /> `String`。<br /><br /> **返回值**<br /><br /> `String`。<br /><br /> **示例**<br /><br /> `-- The following example returns abc.`<br /><br /> `Trim('      abc   ')`|  
  
 如果提供 `null` 输入，则这些函数返回 `null`。  
  
 Microsoft SQL 客户端托管提供程序中提供了等效功能。 有关详细信息，请参阅 [SqlClient for 实体框架函数](../sqlclient-for-ef-functions.md)。  
  
## <a name="see-also"></a>请参阅

- [规范函数](canonical-functions.md)
