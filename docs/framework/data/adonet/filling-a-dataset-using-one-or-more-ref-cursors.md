---
title: 使用一个或多个 REF CURSOR 填充数据集
ms.date: 03/30/2017
dev_langs:
- vb
ms.assetid: 99863e79-5b00-467e-a105-4ffa42de3ff7
ms.openlocfilehash: 07de2e708cd3e69bff05be0c73e41696d715609c
ms.sourcegitcommit: ecd9e9bb2225eb76f819722ea8b24988fe46f34c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/05/2020
ms.locfileid: "96739706"
---
# <a name="filling-a-dataset-using-one-or-more-ref-cursors"></a>使用一个或多个 REF CURSOR 填充数据集

此 Microsoft Visual Basic 示例执行一个 PL/SQL 存储过程，返回两个 REF CURSOR 参数，并使用返回的行填充 <xref:System.Data.DataSet>。

```vb
Private Sub Button1_Click(ByVal sender As Object, _
  ByVal e As System.EventArgs) Handles Button1.Click

  Dim connString As New String(_
    "Data Source=Oracle9i;User ID=scott;Password=[PLACEHOLDER];")
  Dim ds As New DataSet()
    Using conn As New OracleConnection(connString)
    Dim cmd As New OracleCommand()

    cmd.Connection = conn
    cmd.CommandText = "CURSPKG.OPEN_TWO_CURSORS"
    cmd.CommandType = CommandType.StoredProcedure
    cmd.Parameters.Add(New OracleParameter( _
      "EMPCURSOR", OracleType.Cursor)).Direction = _
      ParameterDirection.Output
    cmd.Parameters.Add(New OracleParameter( _
      "DEPTCURSOR", OracleType.Cursor)).Direction = _
       ParameterDirection.Output

    Dim da As New OracleDataAdapter(cmd)
    da.TableMappings.Add("Table", "Emp")
    da.TableMappings.Add("Table1", "Dept")
    da.Fill(ds)

    ds.Relations.Add("EmpDept", ds.Tables("Dept").Columns("Deptno"), _
      ds.Tables("Emp").Columns("Deptno"), False)

    DataGrid1.DataSource = ds.Tables("Dept")
  End Using
```

## <a name="see-also"></a>请参阅

- [Oracle REF CURSOR](oracle-ref-cursors.md)
- [ADO.NET 概述](ado-net-overview.md)
