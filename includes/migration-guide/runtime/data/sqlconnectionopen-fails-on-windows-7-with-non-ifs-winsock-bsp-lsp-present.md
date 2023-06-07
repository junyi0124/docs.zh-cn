---
ms.openlocfilehash: 9ab1686f60bcdbfef5f18576be17aee8c931f9aa
ms.sourcegitcommit: cbacb5d2cebbf044547f6af6e74a9de866800985
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/05/2020
ms.locfileid: "89497633"
---
### <a name="sqlconnectionopen-fails-on-windows-7-with-non-ifs-winsock-bsp-or-lsp-present"></a>SqlConnection.Open 在存在非 IFS Winsock BSP 或 LSP 的 Windows 7 中会失败

#### <a name="details"></a>详细信息

如果在存在非 IFS Winsock BSP 或 LSP 的 Windows 7 计算机上运行，<xref:System.Data.SqlClient.SqlConnection.Open> 和 <xref:System.Data.SqlClient.SqlConnection.OpenAsync(System.Threading.CancellationToken)> 在 .NET Framework 4.5 中将会失败。若要确定是否安装了非 IFS BSP 或 LSP，请使用 <code>netsh WinSock Show Catalog</code> 命令，并查看每个返回的 <code>Winsock Catalog Provider Entry</code> 项。 如果服务标志值设置了 <code>0x20000</code> 位，提供程序将使用 IFS 句柄，并将正确运行。 如果 <code>0x20000</code> 位已清除（未设置），则它将是非 IFS BSP 或 LSP。

#### <a name="suggestion"></a>建议

此 bug 已在 .NET Framework 4.5.2 中得到修复，因此升级 .NET Framework 可避免出现此问题。 或者，可通过删除任何已安装的非 IFS Winsock LSP 来避免出现此问题。

| “属性”    | “值”       |
|:--------|:------------|
| 范围   |次要|
|Version|4.5|
|类型|运行时|

#### <a name="affected-apis"></a>受影响的 API

- <xref:System.Data.SqlClient.SqlConnection.Open?displayProperty=nameWithType>
- <xref:System.Data.SqlClient.SqlConnection.OpenAsync(System.Threading.CancellationToken)?displayProperty=nameWithType>

<!--

#### Affected APIs

- `M:System.Data.SqlClient.SqlConnection.Open`
- `M:System.Data.SqlClient.SqlConnection.OpenAsync(System.Threading.CancellationToken)`

-->
