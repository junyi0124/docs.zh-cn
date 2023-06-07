---
title: Oracle 序列
ms.date: 03/30/2017
ms.assetid: 27cd371d-8252-414d-b5b2-5d31fa44b585
ms.openlocfilehash: 5e979a0a6750a654a69522d1fb10cdfa7242b893
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91189157"
---
# <a name="oracle-sequences"></a>Oracle 序列

用于 Oracle 的 .NET Framework 数据提供程序支持在使用 <xref:System.Data.OracleClient.OracleDataAdapter> 执行插入后检索服务器生成的 Oracle 序列键值。  
  
 SQL Server 和 Oracle 支持创建可指派为主键的自动递增列。 在向表中添加行时，服务器会生成这些值。 在 SQL Server 中，您可以设置列的标识属性；在 Oracle 中，您可以创建一个序列。 SQL Server 中自动递增列和 Oracle 中序列的区别是：  
  
- 在 SQL Server 中，将某列标记为自动递增列，SQL Server 会在您插入新行时为该列自动生成新值。  
  
- 在 Oracle 中，您创建一个序列以便为表中的列生成新值，但序列和表或列之间不存在直接链接。 Oracle 序列是一个类似表或存储过程的对象。  
  
 在 Oracle 数据库中创建序列时，您可以定义其初始值以及值之间的增量。 在提交新行之前，您也可以查询序列中的新值。 这意味着在你将新行插入数据库之前，代码可识别新行的键值。  
  
 有关使用 SQL Server 和 ADO.NET 创建自动递增列的详细信息，请参阅 [检索标识或自动编号值](retrieving-identity-or-autonumber-values.md) 和 [创建自动增量列](./dataset-datatable-dataview/creating-autoincrement-columns.md)。  
  
## <a name="example"></a>示例  

 下面的 C# 示例演示如何从 Oracle 数据库检索新序列值。 此示例引用用于提交新行的 INSERT INTO 查询中的序列，然后返回使用 Oracle10g 中引入的 RETURNING 子句生成的序列值。 此示例可使用 ADO.NET 自动递增功能在 <xref:System.Data.DataTable> 中添加一系列挂起的新行，以生成“占位符”主键值。 注意：ADO.NET 为新行生成的增量值仅是一个“占位符”。 这意味着数据库生成的值可能与 ADO.NET 生成的值不同。  
  
 在将挂起的插入行提交给数据库之前，此示例显示行的内容。 然后，代码将创建一个新的 <xref:System.Data.OracleClient.OracleDataAdapter> 对象并设置其 <xref:System.Data.OracleClient.OracleDataAdapter.InsertCommand%2A> 和 <xref:System.Data.OracleClient.OracleDataAdapter.UpdateBatchSize%2A> 属性。 此示例还提供使用输出参数返回服务器生成的值的逻辑。 然后，此示例将执行更新以提交挂起的行并显示 <xref:System.Data.DataTable> 的内容。  
  
```csharp  
public void OracleSequence(String connectionString)  
{  
   String insertString =
      "INSERT INTO SequenceTest_Table (ID, OtherColumn)" +  
      "VALUES (SequenceTest_Sequence.NEXTVAL, :OtherColumn)" +  
      "RETURNING ID INTO :ID";  
  
   using (OracleConnection conn = new OracleConnection(connectionString))  
   {  
      //Open a connection.  
      conn.Open();  
      OracleCommand cmd = conn.CreateCommand();  
  
      // Prepare the database.  
      cmd.CommandText = "DROP SEQUENCE SequenceTest_Sequence";  
      try { cmd.ExecuteNonQuery(); } catch { }  
  
      cmd.CommandText = "DROP TABLE SequenceTest_Table";  
      try { cmd.ExecuteNonQuery(); } catch { }  
  
      cmd.CommandText = "CREATE TABLE SequenceTest_Table " +  
                     "(ID int PRIMARY KEY, OtherColumn varchar(255))";  
      cmd.ExecuteNonQuery();  
  
      cmd.CommandText = "CREATE SEQUENCE SequenceTest_Sequence " +  
                        "START WITH 100 INCREMENT BY 5";  
      cmd.ExecuteNonQuery();  
  
      DataTable testTable = new DataTable();  
      DataColumn column = testTable.Columns.Add("ID", typeof(int));  
      column.AutoIncrement = true;  
      column.AutoIncrementSeed = -1;  
      column.AutoIncrementStep = -1;  
      testTable.PrimaryKey = new DataColumn[] { column };  
      testTable.Columns.Add("OtherColumn", typeof(string));  
      for (int rowCounter = 1; rowCounter <= 15; rowCounter++)  
      {  
         testTable.Rows.Add(null, "Row #" + rowCounter.ToString());  
      }  
  
      Console.WriteLine("Before Update => ");  
      foreach (DataRow row in testTable.Rows)  
      {  
         Console.WriteLine("   {0} - {1}", row["ID"], row["OtherColumn"]);  
      }  
      Console.WriteLine();  
  
      cmd.CommandText =
        "SELECT ID, OtherColumn FROM SequenceTest_Table";  
      OracleDataAdapter da = new OracleDataAdapter(cmd);  
      da.InsertCommand = new OracleCommand(insertString, conn);  
      da.InsertCommand.Parameters.Add(":ID", OracleType.Int32, 0, "ID");  
      da.InsertCommand.Parameters[0].Direction = ParameterDirection.Output;  
      da.InsertCommand.Parameters.Add(":OtherColumn", OracleType.VarChar, 255, "OtherColumn");  
      da.InsertCommand.UpdatedRowSource = UpdateRowSource.OutputParameters;  
      da.UpdateBatchSize = 10;  
  
      da.Update(testTable);  
  
      Console.WriteLine("After Update => ");  
      foreach (DataRow row in testTable.Rows)  
      {  
         Console.WriteLine("   {0} - {1}", row["ID"], row["OtherColumn"]);  
      }  
      // Close the connection.  
      conn.Close();  
   }  
}  
```  
  
## <a name="see-also"></a>请参阅

- [Oracle 和 ADO.NET](oracle-and-adonet.md)
- [ADO.NET 概述](ado-net-overview.md)
