---
title: 使用 DataAdapter 更新数据源
description: 了解 DataAdapter 的 Update 方法如何将数据集中的更改解析回 ADO.NET 应用程序中的数据源。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: d1bd9a8c-0e29-40e3-bda8-d89176b72fb1
ms.openlocfilehash: e2348a3a89aa1c28856bfc21aaa25f2c8327aac7
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/02/2020
ms.locfileid: "84286179"
---
# <a name="updating-data-sources-with-dataadapters"></a>使用 DataAdapter 更新数据源

调用 `Update` 的 <xref:System.Data.Common.DataAdapter> 方法可以将 <xref:System.Data.DataSet> 中的更改解析回数据源。 与 `Update` 方法类似，`Fill` 方法将 `DataSet` 的实例和可选的 <xref:System.Data.DataTable> 对象或 `DataTable` 名称用作自变量。 `DataSet` 实例是包含已做的更改的 `DataSet`，`DataTable` 标识从其中检索这些更改的表。 如果未指定 `DataTable`，则使用 `DataTable` 中的第一个 `DataSet`。

当调用 `Update` 方法时，`DataAdapter` 会分析已做的更改并执行相应的命令（INSERT、UPDATE 或 DELETE）。 当 `DataAdapter` 遇到对 <xref:System.Data.DataRow> 所做的更改时，它将使用 <xref:System.Data.Common.DbDataAdapter.InsertCommand%2A>、<xref:System.Data.Common.DbDataAdapter.UpdateCommand%2A> 或 <xref:System.Data.Common.DbDataAdapter.DeleteCommand%2A> 来处理该更改。 这样，您就可以通过在设计时指定命令语法并在可能时通过使用存储过程来尽量提高 ADO.NET 应用程序的性能。 在调用 `Update` 之前，必须显式设置这些命令。 如果调用了 `Update` 但不存在用于特定更新的相应命令（例如，不存在用于已删除行的 `DeleteCommand`），则会引发异常。

> [!NOTE]
> 如果您要通过 SQL Server 存储过程使用 `DataAdapter` 来编辑或删除数据，请确保不要在存储过程定义中使用 SET NOCOUNT ON。 这将使返回的受影响的行数为零，`DataAdapter` 会将其解释为并发冲突。 在这种情况下，将引发 <xref:System.Data.DBConcurrencyException>。

可以使用命令参数为 `DataSet` 中每个已修改的行指定 SQL 语句或存储过程的输入和输出值。 有关详细信息，请参阅[DataAdapter 参数](dataadapter-parameters.md)。

> [!NOTE]
> 必须了解在 <xref:System.Data.DataTable> 中删除行和移除行之间的差异。 当调用 `Remove` 或 `RemoveAt` 方法时，会立即移除该行。 如果之后将 `DataTable` 或 `DataSet` 传递给 `DataAdapter` 并调用 `Update`，则不会影响后端数据源中的任何相应行。 当您使用 `Delete` 方法时，该行仍将保留在 `DataTable` 中并会标记为删除。 如果之后将 `DataTable` 或 `DataSet` 传递给 `DataAdapter` 并调用 `Update`，则会删除后端数据源中的相应行。

如果 `DataTable` 映射到单个数据库表或从单个数据库表生成，则可以利用 <xref:System.Data.Common.DbCommandBuilder> 对象为 `DeleteCommand` 自动生成 `InsertCommand`、`UpdateCommand` 和 `DataAdapter` 对象。 有关详细信息，请参阅[通过 Commandbuilder 生成命令](generating-commands-with-commandbuilders.md)。

## <a name="using-updatedrowsource-to-map-values-to-a-dataset"></a>使用 UpdatedRowSource 将值映射到数据集

通过使用 `DataTable` 对象的 `DataAdapter` 属性，您可以在调用 <xref:System.Data.Common.DbCommand.UpdatedRowSource%2A> 的 Update 方法后控制从数据源返回的值映射回 <xref:System.Data.Common.DbCommand> 的方式。 通过将 `UpdatedRowSource` 属性设置为 <xref:System.Data.UpdateRowSource> 枚举值之一，您可以控制是忽略由 `DataAdapter` 命令返回的输出参数还是将其应用于 `DataSet` 中已更改的行。 还可以指定是否将返回的第一行（如果存在）应用于 `DataTable` 中已更改的行。

下表说明 `UpdateRowSource` 枚举的不同值，并说明它们如何影响与 `DataAdapter` 一起使用的命令的行为。

|UpdatedRowSource 枚举|说明|
|----------------------------------|-----------------|
|<xref:System.Data.UpdateRowSource.Both>|输出参数和返回的结果集的第一行都可以映射到 `DataSet` 中已更改的行。|
|<xref:System.Data.UpdateRowSource.FirstReturnedRecord>|只有返回的结果集的第一行中的数据才可以映射到 `DataSet` 中已更改的行。|
|<xref:System.Data.UpdateRowSource.None>|忽略任何输出参数或返回的结果集中的行。|
|<xref:System.Data.UpdateRowSource.OutputParameters>|只有输出参数才可以映射到 `DataSet` 中已更改的行。|

`Update` 方法会将更改解析回数据源；但在上次填充 `DataSet` 后，其他客户端可能已修改了数据源中的数据。 若要使用当前数据刷新 `DataSet`，请使用 `DataAdapter` 和 `Fill` 方法。 新行将添加到该表中，更新的信息将并入现有行。 `Fill` 方法通过检查 `DataSet` 中行的主键值以及 `SelectCommand` 返回的行来确定是要添加新行还是更新现有行。 如果 `Fill` 方法遇到 `DataSet` 中某行的主键值与 `SelectCommand` 返回结果中某行的主键值相匹配，则它将用 `SelectCommand` 返回的行中的信息更新现有行，并将现有行的 <xref:System.Data.DataRow.RowState%2A> 设置为 `Unchanged`。 如果 `SelectCommand` 返回的行所具有的主键值与 `DataSet` 中行的任何主键值都不匹配，则 `Fill` 方法将添加 `RowState` 为 `Unchanged` 的新行。

> [!NOTE]
> 如果 `SelectCommand` 返回 OUTER JOIN 的结果，则 `DataAdapter` 不会为生成的 `PrimaryKey` 设置 `DataTable` 值。 您必须自己定义 `PrimaryKey` 以确保正确解析重复行。 有关详细信息，请参阅[定义主键](./dataset-datatable-dataview/defining-primary-keys.md)。

若要处理在调用方法时可能发生的异常 `Update` `RowUpdated` （请参阅[处理 DataAdapter 事件](handling-dataadapter-events.md)），或者可以在 `DataAdapter.ContinueUpdateOnError` 调用之前将设置为，并在 `true` `Update` `RowError` 更新完成后响应特定行的属性中存储的错误信息（请参阅[行错误信息](./dataset-datatable-dataview/row-error-information.md)），以处理在调用方法时可能发生的异常。

> [!NOTE]
> 对 `AcceptChanges` `DataSet` 、或调用 `DataTable` `DataRow` 将导致 `Original` 用的值覆盖的所有值 `DataRow` `Current` `DataRow` 。 如果修改了唯一标识该行的字段值，则在调用 `AcceptChanges` 后，`Original` 值将不再匹配数据源中的值。 在调用 `AcceptChanges` 的 Update 方法期间会对每一行自动调用 `DataAdapter`。 在调用 Update 方法期间，通过先将 `AcceptChangesDuringUpdate` 的 `DataAdapter` 属性设置为 false，或为 `RowUpdated` 事件创建一个事件处理程序并将 <xref:System.Data.Common.RowUpdatedEventArgs.Status%2A> 设置为 <xref:System.Data.UpdateStatus.SkipCurrentRow>，可以保留原始值。 有关详细信息，请参阅[合并数据集内容](./dataset-datatable-dataview/merging-dataset-contents.md)和[处理 DataAdapter 事件](handling-dataadapter-events.md)。

## <a name="example"></a>示例

下面的示例演示如何通过显式设置 `UpdateCommand` 的 `DataAdapter` 和调用其方法来对修改的行执行更新 `Update` 。 请注意，在 UPDATE 语句的 WHERE 子句中指定的参数设置为使用 `Original` 的 `SourceColumn` 值。 这一点很重要，因为 `Current` 值可能已被修改，可能会不匹配数据源中的值。 `Original` 值是用于从数据源填充 `DataTable` 的值。

[!code-csharp[DataWorks SqlClient.DataAdapterUpdate#1](../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DataWorks SqlClient.DataAdapterUpdate/CS/source.cs#1)]
[!code-vb[DataWorks SqlClient.DataAdapterUpdate#1](../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DataWorks SqlClient.DataAdapterUpdate/VB/source.vb#1)]

## <a name="autoincrement-columns"></a>AutoIncrement 列

如果数据源中的表具有自动递增列，则可以通过以下方式填充 `DataSet` 中的列：作为存储过程的输出参数返回自动递增值并将其映射到表中的一列、返回由存储过程或 SQL 语句返回的结果集第一行中的自动递增值或者使用 `RowUpdated` 的 `DataAdapter` 事件来执行其他 SELECT 语句。 有关详细信息和示例，请参阅[检索标识或自动编号值](retrieving-identity-or-autonumber-values.md)。

## <a name="ordering-of-inserts-updates-and-deletes"></a>插入、更新和删除的排序

在许多情况下，以何种顺序向数据源发送通过 `DataSet` 所做的更改是非常重要的。 例如，如果更新了现有行的主键值，并且添加了以新主键值作为外键的新行，则务必要在处理插入之前处理更新。

可以使用 `Select` 的 `DataTable` 方法来返回仅引用具有特定 `DataRow` 的 `RowState` 数组。 然后可以将返回的 `DataRow` 数组传递给 `Update` 的 `DataAdapter` 方法来处理已修改的行。 通过指定要更新的行的子集，可以控制处理插入、更新和删除的顺序。

## <a name="example"></a>示例

例如，以下代码确保首先处理表中已删除的行，然后处理已更新的行，然后处理已插入的行。

```vb
Dim table As DataTable = dataSet.Tables("Customers")

' First process deletes.
dataSet.Update(table.Select(Nothing, Nothing, _
  DataViewRowState.Deleted))

' Next process updates.
adapter.Update(table.Select(Nothing, Nothing, _
  DataViewRowState.ModifiedCurrent))

' Finally, process inserts.
adapter.Update(table.Select(Nothing, Nothing, _
  DataViewRowState.Added))
```

```csharp
DataTable table = dataSet.Tables["Customers"];

// First process deletes.
adapter.Update(table.Select(null, null, DataViewRowState.Deleted));

// Next process updates.
adapter.Update(table.Select(null, null,
  DataViewRowState.ModifiedCurrent));

// Finally, process inserts.
adapter.Update(table.Select(null, null, DataViewRowState.Added));
```

## <a name="use-a-dataadapter-to-retrieve-and-update-data"></a>使用 DataAdapter 来检索和更新数据

您可以使用 DataAdapter 来检索和更新数据。

- 此示例使用 DataAdapter.AcceptChangesDuringFill 克隆数据库中的数据。 如果该属性设置为 false，则在填充该表时不会调用 AcceptChanges，并将新添加的行视为插入的行。 因此，此示例使用这些行将新行插到数据库中。

- 此示例使用 DataAdapter.TableMappings 来定义源表与 DataTable 之间的映射。

- 此示例使用 DataAdapter.FillLoadOption 来确定适配器从 DbDataReader 填充 DataTable 的方式。 在您创建 DataTable 时，可以通过将该属性设置为 LoadOption.Upsert 或 LoadOption.PreserveChanges 而仅将数据库中的数据写入当前版本或原始版本。

- 此示例还将通过使用 DbDataAdapter.UpdateBatchSize 执行批处理操作来更新表。

在编译并运行此示例之前，您需要创建示例数据库：

```sql
USE [master]
GO

CREATE DATABASE [MySchool]

GO

USE [MySchool]
GO

SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
CREATE TABLE [dbo].[Course]([CourseID] [nvarchar](10) NOT NULL,
[Year] [smallint] NOT NULL,
[Title] [nvarchar](100) NOT NULL,
[Credits] [int] NOT NULL,
[DepartmentID] [int] NOT NULL,
 CONSTRAINT [PK_Course] PRIMARY KEY CLUSTERED
(
[CourseID] ASC,
[Year] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON) ON [PRIMARY]) ON [PRIMARY]

GO

SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
CREATE TABLE [dbo].[Department]([DepartmentID] [int] IDENTITY(1,1) NOT NULL,
[Name] [nvarchar](50) NOT NULL,
[Budget] [money] NOT NULL,
[StartDate] [datetime] NOT NULL,
[Administrator] [int] NULL,
 CONSTRAINT [PK_Department] PRIMARY KEY CLUSTERED
(
[DepartmentID] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON) ON [PRIMARY]) ON [PRIMARY]

GO

INSERT [dbo].[Course] ([CourseID], [Year], [Title], [Credits], [DepartmentID]) VALUES (N'C1045', 2012, N'Calculus', 4, 7)
INSERT [dbo].[Course] ([CourseID], [Year], [Title], [Credits], [DepartmentID]) VALUES (N'C1061', 2012, N'Physics', 4, 1)
INSERT [dbo].[Course] ([CourseID], [Year], [Title], [Credits], [DepartmentID]) VALUES (N'C2021', 2012, N'Composition', 3, 2)
INSERT [dbo].[Course] ([CourseID], [Year], [Title], [Credits], [DepartmentID]) VALUES (N'C2042', 2012, N'Literature', 4, 2)

SET IDENTITY_INSERT [dbo].[Department] ON

INSERT [dbo].[Department] ([DepartmentID], [Name], [Budget], [StartDate], [Administrator]) VALUES (1, N'Engineering', 350000.0000, CAST(0x0000999C00000000 AS DateTime), 2)
INSERT [dbo].[Department] ([DepartmentID], [Name], [Budget], [StartDate], [Administrator]) VALUES (2, N'English', 120000.0000, CAST(0x0000999C00000000 AS DateTime), 6)
INSERT [dbo].[Department] ([DepartmentID], [Name], [Budget], [StartDate], [Administrator]) VALUES (4, N'Economics', 200000.0000, CAST(0x0000999C00000000 AS DateTime), 4)
INSERT [dbo].[Department] ([DepartmentID], [Name], [Budget], [StartDate], [Administrator]) VALUES (7, N'Mathematics', 250024.0000, CAST(0x0000999C00000000 AS DateTime), 3)
SET IDENTITY_INSERT [dbo].[Department] OFF

ALTER TABLE [dbo].[Course]  WITH CHECK ADD  CONSTRAINT [FK_Course_Department] FOREIGN KEY([DepartmentID])
REFERENCES [dbo].[Department] ([DepartmentID])
GO
ALTER TABLE [dbo].[Course] CHECK CONSTRAINT [FK_Course_Department]
GO
```

带有此代码示例的 c # 和 Visual Basic 项目可在[开发人员代码示例](https://code.msdn.microsoft.com/site/search?f%5B0%5D.Type=SearchText&f%5B0%5D.Value=How%20to%20use%20DataAdapter%20to%20retrieve%20and%20update%20data&f%5B1%5D)中找到。

```csharp
using System;
using System.Data;
using System.Data.Common;
using System.Data.SqlClient;
using System.Linq;
using CSDataAdapterOperations.Properties;

namespace CSDataAdapterOperations.Properties {
   internal sealed partial class Settings : global::System.Configuration.ApplicationSettingsBase {

      private static Settings defaultInstance = ((Settings)(global::System.Configuration.ApplicationSettingsBase.Synchronized(new Settings())));

      public static Settings Default {
         get {
            return defaultInstance;
         }
      }

      [global::System.Configuration.ApplicationScopedSettingAttribute()]
      [global::System.Configuration.DefaultSettingValueAttribute("Data Source=(local);Initial Catalog=MySchool;Integrated Security=True")]
      public string MySchoolConnectionString {
         get {
            return ((string)(this["MySchoolConnectionString"]));
         }
      }
   }
}

class Program {
   static void Main(string[] args) {
      Settings settings = new Settings();

      // Copy the data from the database.  Get the table Department and Course from the database.
      String selectString = @"SELECT [DepartmentID],[Name],[Budget],[StartDate],[Administrator]
                                     FROM [MySchool].[dbo].[Department];

                                   SELECT [CourseID],@Year as [Year],Max([Title]) as [Title],
                                   Max([Credits]) as [Credits],Max([DepartmentID]) as [DepartmentID]
                                   FROM [MySchool].[dbo].[Course]
                                   Group by [CourseID]";

      DataSet mySchool = new DataSet();

      SqlCommand selectCommand = new SqlCommand(selectString);
      SqlParameter parameter = selectCommand.Parameters.Add("@Year", SqlDbType.SmallInt, 2);
      parameter.Value = new Random(DateTime.Now.Millisecond).Next(9999);

      // Use DataTableMapping to map the source tables and the destination tables.
      DataTableMapping[] tableMappings = {new DataTableMapping("Table", "Department"), new DataTableMapping("Table1", "Course")};
      CopyData(mySchool, settings.MySchoolConnectionString, selectCommand, tableMappings);

      Console.WriteLine("The following tables are from the database.");
      foreach (DataTable table in mySchool.Tables) {
         Console.WriteLine(table.TableName);
         ShowDataTable(table);
      }

      // Roll back the changes
      DataTable department = mySchool.Tables["Department"];
      DataTable course = mySchool.Tables["Course"];

      department.Rows[0]["Name"] = "New" + department.Rows[0][1];
      course.Rows[0]["Title"] = "New" + course.Rows[0]["Title"];
      course.Rows[0]["Credits"] = 10;

      Console.WriteLine("After we changed the tables:");
      foreach (DataTable table in mySchool.Tables) {
         Console.WriteLine(table.TableName);
         ShowDataTable(table);
      }

      department.RejectChanges();
      Console.WriteLine("After use the RejectChanges method in Department table to roll back the changes:");
      ShowDataTable(department);

      DataColumn[] primaryColumns = { course.Columns["CourseID"] };
      DataColumn[] resetColumns = { course.Columns["Title"] };
      ResetCourse(course, settings.MySchoolConnectionString, primaryColumns, resetColumns);
      Console.WriteLine("After use the ResetCourse method in Course table to roll back the changes:");
      ShowDataTable(course);

      // Batch update the table.
      String insertString = @"Insert into [MySchool].[dbo].[Course]([CourseID],[Year],[Title],
                                   [Credits],[DepartmentID])
             values (@CourseID,@Year,@Title,@Credits,@DepartmentID)";
      SqlCommand insertCommand = new SqlCommand(insertString);
      insertCommand.Parameters.Add("@CourseID", SqlDbType.NVarChar, 10, "CourseID");
      insertCommand.Parameters.Add("@Year", SqlDbType.SmallInt, 2, "Year");
      insertCommand.Parameters.Add("@Title", SqlDbType.NVarChar, 100, "Title");
      insertCommand.Parameters.Add("@Credits", SqlDbType.Int, 4, "Credits");
      insertCommand.Parameters.Add("@DepartmentID", SqlDbType.Int, 4, "DepartmentID");

      const Int32 batchSize = 10;
      BatchInsertUpdate(course, settings.MySchoolConnectionString, insertCommand, batchSize);
   }

   private static void CopyData(DataSet dataSet, String connectionString, SqlCommand selectCommand, DataTableMapping[] tableMappings) {
      using (SqlConnection connection = new SqlConnection(connectionString)) {
         selectCommand.Connection = connection;

         connection.Open();

         using (SqlDataAdapter adapter = new SqlDataAdapter(selectCommand)) {adapter.TableMappings.AddRange(tableMappings);
            // If set the AcceptChangesDuringFill as the false, AcceptChanges will not be called on a
            // DataRow after it is added to the DataTable during any of the Fill operations.
            adapter.AcceptChangesDuringFill = false;

            adapter.Fill(dataSet);
         }
      }
   }

   // Roll back only one column or several columns data of the Course table by call ResetDataTable method.
   private static void ResetCourse(DataTable table, String connectionString,
       DataColumn[] primaryColumns, DataColumn[] resetColumns) {
      table.PrimaryKey = primaryColumns;

      // Build the query string
      String primaryCols = String.Join(",", primaryColumns.Select(col => col.ColumnName));
      String resetCols = String.Join(",", resetColumns.Select(col => $"Max({col.ColumnName}) as {col.ColumnName}"));

      String selectString = $"Select {primaryCols},{resetCols} from Course Group by {primaryCols}");

      SqlCommand selectCommand = new SqlCommand(selectString);

      ResetDataTable(table, connectionString, selectCommand);
   }

   // RejectChanges will roll back all changes made to the table since it was loaded, or the last time AcceptChanges
   // was called. When you copy from the database, you can lose all the data after calling RejectChanges
   // The ResetDataTable method rolls back one or more columns of data.
   private static void ResetDataTable(DataTable table, String connectionString,
       SqlCommand selectCommand) {
      using (SqlConnection connection = new SqlConnection(connectionString)) {
         selectCommand.Connection = connection;

         connection.Open();

         using (SqlDataAdapter adapter = new SqlDataAdapter(selectCommand)) {
            // The incoming values for this row will be written to the current version of each
            // column. The original version of each column's data will not be changed.
            adapter.FillLoadOption = LoadOption.Upsert;

            adapter.Fill(table);
         }
      }
   }

   private static void BatchInsertUpdate(DataTable table, String connectionString,
       SqlCommand insertCommand, Int32 batchSize) {
      using (SqlConnection connection = new SqlConnection(connectionString)) {
         insertCommand.Connection = connection;
         // When setting UpdateBatchSize to a value other than 1, all the commands
         // associated with the SqlDataAdapter have to have their UpdatedRowSource
         // property set to None or OutputParameters. An exception is thrown otherwise.
         insertCommand.UpdatedRowSource = UpdateRowSource.None;

         connection.Open();

         using (SqlDataAdapter adapter = new SqlDataAdapter()) {
            adapter.InsertCommand = insertCommand;
            // Gets or sets the number of rows that are processed in each round-trip to the server.
            // Setting it to 1 disables batch updates, as rows are sent one at a time.
            adapter.UpdateBatchSize = batchSize;

            adapter.Update(table);

            Console.WriteLine("Successfully to update the table.");
         }
      }
   }

   private static void ShowDataTable(DataTable table) {
      foreach (DataColumn col in table.Columns) {
         Console.Write("{0,-14}", col.ColumnName);
      }
      Console.WriteLine("{0,-14}", "RowState");

      foreach (DataRow row in table.Rows) {
         foreach (DataColumn col in table.Columns) {
            if (col.DataType.Equals(typeof(DateTime)))
               Console.Write("{0,-14:d}", row[col]);
            else if (col.DataType.Equals(typeof(Decimal)))
               Console.Write("{0,-14:C}", row[col]);
            else
               Console.Write("{0,-14}", row[col]);
         }
         Console.WriteLine("{0,-14}", row.RowState);
      }
   }
}
```

## <a name="see-also"></a>另请参阅

- [DataAdapter 和 DataReader](dataadapters-and-datareaders.md)
- [行状态和行版本](./dataset-datatable-dataview/row-states-and-row-versions.md)
- [AcceptChange 和 RejectChange](./dataset-datatable-dataview/acceptchanges-and-rejectchanges.md)
- [合并数据集内容](./dataset-datatable-dataview/merging-dataset-contents.md)
- [检索标识或自动编号值](retrieving-identity-or-autonumber-values.md)
- [ADO.NET 概述](ado-net-overview.md)
