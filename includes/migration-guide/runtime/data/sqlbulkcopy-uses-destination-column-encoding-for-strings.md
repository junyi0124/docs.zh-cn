---
ms.openlocfilehash: fd9f4f3de8f7be39242d4ff6924d480f20a1a06b
ms.sourcegitcommit: cbacb5d2cebbf044547f6af6e74a9de866800985
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/05/2020
ms.locfileid: "89497745"
---
### <a name="sqlbulkcopy-uses-destination-column-encoding-for-strings"></a>SqlBulkCopy 对字符串使用目标列编码

#### <a name="details"></a>详细信息

将数据插入列中时，<xref:System.Data.SqlClient.SqlBulkCopy?displayProperty=fullName> 使用目标列的编码而不是 <code>VARCHAR</code> 和 <code>CHAR</code> 类型的默认编码。 在目标列未使用默认编码时，此更改会消除使用此默认编码所引起的数据损坏的可能性。 在极少数情况下，如果对编码进行的更改所生成的数据过大而无法适应目标列，则现有应用程序可能会引发 SqlException 异常。

#### <a name="suggestion"></a>建议

<xref:System.Data.SqlClient.SqlBulkCopy?displayProperty=fullName> 应不再因为编码差异而损坏数据。 如果复制了接近目标列大小限制的字符串，那么可能需要预编码数据（复制该数据以检查数据是否适合目标列）或者捕获 <xref:System.Data.SqlClient.SqlException?displayProperty=fullName>。

| “属性”    | “值”       |
|:--------|:------------|
| 范围   |边缘|
|Version|4.5|
|类型|运行时|

#### <a name="affected-apis"></a>受影响的 API

- <xref:System.Data.SqlClient.SqlBulkCopy?displayProperty=nameWithType>
- <xref:System.Data.SqlClient.SqlBulkCopy.%23ctor(System.Data.SqlClient.SqlConnection)>

<!--

#### Affected APIs

- `T:System.Data.SqlClient.SqlBulkCopy`
- `M:System.Data.SqlClient.SqlBulkCopy.#ctor(System.Data.SqlClient.SqlConnection)`

-->
