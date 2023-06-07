---
ms.openlocfilehash: 771238c53dc97f4cf4068968f3c68500ba9f87da
ms.sourcegitcommit: 0802ac583585110022beb6af8ea0b39188b77c43
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96032263"
---
### <a name="caching-microsoftextensionscachingsqlserver-uses-new-sqlclient-package"></a>缓存：Microsoft.Extensions.Caching.SqlServer 使用新 SqlClient 包

`Microsoft.Extensions.Caching.SqlServer` 包将使用新的 `Microsoft.Data.SqlClient` 包，而不是 `System.Data.SqlClient` 包。 此更改可能会导致轻微的行为中断性变更。 有关详细信息，请参阅[引入新的 Microsoft.Data.SqlClient](https://devblogs.microsoft.com/dotnet/introducing-the-new-microsoftdatasqlclient/)。

#### <a name="version-introduced"></a>引入的版本

3.0

#### <a name="old-behavior"></a>旧行为

`Microsoft.Extensions.Caching.SqlServer` 包已使用过 `System.Data.SqlClient` 包。

#### <a name="new-behavior"></a>新行为

`Microsoft.Extensions.Caching.SqlServer` 现在使用 `Microsoft.Data.SqlClient` 包。

#### <a name="reason-for-change"></a>更改原因

`Microsoft.Data.SqlClient` 是从 `System.Data.SqlClient` 生成的新包。 从现在开始，所有新功能的工作都在该包中进行。

#### <a name="recommended-action"></a>建议操作

客户无需担心此中断性变更，除非他们使用 `Microsoft.Extensions.Caching.SqlServer` 包返回的类型，并将它们强制转换为 `System.Data.SqlClient` 类型。 例如，如果有人将 `DbConnection` 强制转换为[旧的 SqlConnection 类型](xref:System.Data.SqlClient.SqlConnection)，则需要将转换更改为新的 `Microsoft.Data.SqlClient.SqlConnection` 类型。

#### <a name="category"></a>类别

ASP.NET Core

#### <a name="affected-apis"></a>受影响的 API

None

<!-- 

#### Affected APIs

Not detectable via API analysis

-->
