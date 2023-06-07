---
title: 检索标识或自动编号值
description: 了解如何在关系数据库中检索主键的标识和自动编号值，以及如何在 ADO.NET 中合并新的标识值。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: d6b7f9cb-81be-44e1-bb94-56137954876d
ms.openlocfilehash: dbbc013a5b6c83391a29109beca44120c68d827f
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/02/2020
ms.locfileid: "84286567"
---
# <a name="retrieving-identity-or-autonumber-values"></a>检索标识或自动编号值

关系数据库中的主键是始终包含唯一值的列或列的组合。 知道主键值使您可以定位包含该值的行。 关系数据库引擎（如 SQL Server、Oracle 和 Microsoft Access/Jet）支持创建可指定为主键的自动递增列。 在向表中添加行时，服务器会生成这些值。 在 SQL Server 中可以设置列的标识属性，在 Oracle 中可以创建序列，在 Microsoft Access 中可以创建 AutoNumber 列。

通过将 <xref:System.Data.DataColumn> 属性设置为 True，也可以使用 <xref:System.Data.DataColumn.AutoIncrement%2A> 来生成自动递增值。 但是，如果多个客户端应用程序独立生成自动递增值，最后会使 <xref:System.Data.DataTable> 的单独实例中出现重复的值。 让服务器生成自动递增值，以允许每个用户为每个插入的行检索生成的值，从而消除潜在的冲突。

在调用 `Update` 的 `DataAdapter` 方法期间，数据库可以将数据以输出参数的形式或以与 INSERT 语句同一批执行的 SELECT 语句返回的结果集的第一个记录的形式，发回到您的 ADO.NET 应用程序。 ADO.NET 可以检索这些值并在要更新的 <xref:System.Data.DataRow> 中更新相应的列。

某些数据库引擎（如 Microsoft Access Jet 数据库引擎）不支持输出参数，不能在单个批处理中处理多个语句。 当使用 Jet 数据库引擎时，可以通过在 `RowUpdated` 的 `DataAdapter` 事件的事件处理程序中执行单独的 SELECT 命令来检索为插入行生成的新 AutoNumber 值。

> [!NOTE]
> 使用自动递增值的替代方法是使用 <xref:System.Guid.NewGuid%2A> 对象的 <xref:System.Guid> 方法在客户端计算机上生成一个 GUID（即全局唯一标识符），插入每个新行时可以将此标识符复制到服务器。 `NewGuid` 方法使用算法生成一个 16 字节的二进制值，该算法不生成重复值的概率很高。 在 SQL Server 数据库中，GUID 存储在 `uniqueidentifier` 列中，SQL Server 可以使用 Transact-SQL `NEWID()` 函数自动生成此列。 将 GUID 用作主键会对性能产生负面影响。 SQL Server 提供对函数的支持 `NEWSEQUENTIALID()` ，该函数将生成一个不保证全局唯一但可以更有效地索引的顺序 GUID。

## <a name="retrieving-sql-server-identity-column-values"></a>检索 SQL Server“标识”列的值

使用 Microsoft SQL Server 时，可以创建通过输出参数返回插入行标识值的存储过程。 下表说明 SQL Server 中可用来检索标识列值的三个 Transact-SQL 函数。

|函数|说明|
|--------------|-----------------|
|SCOPE_IDENTITY|返回当前执行范围内的最后一个标识值。 对于多数方案，建议使用 SCOPE_IDENTITY。|
|@@IDENTITY|包含当前会话中任何表生成的最后一个标识值。 @ 可能 @IDENTITY 会受到触发器的影响，并且可能不会返回所需的标识值。|
|IDENT_CURRENT|返回在任何会话中以及任何范围内为特定表生成的最后一个标识值。|

 以下存储过程演示了如何将行插入到 "**类别**" 表中，并使用 output 参数返回由 transact-sql SCOPE_IDENTITY （）函数生成的新标识值。

```sql
CREATE PROCEDURE dbo.InsertCategory
  @CategoryName nvarchar(15),
  @Identity int OUT
AS
INSERT INTO Categories (CategoryName) VALUES(@CategoryName)
SET @Identity = SCOPE_IDENTITY()
```

然后可以将此存储过程指定为 <xref:System.Data.SqlClient.SqlDataAdapter.InsertCommand%2A> 对象的 <xref:System.Data.SqlClient.SqlDataAdapter> 的源。 <xref:System.Data.SqlClient.SqlCommand.CommandType%2A> 的 <xref:System.Data.SqlClient.SqlDataAdapter.InsertCommand%2A> 属性必须设置为 <xref:System.Data.CommandType.StoredProcedure>。 可以通过创建一个 <xref:System.Data.SqlClient.SqlParameter> 为 <xref:System.Data.ParameterDirection> 的 <xref:System.Data.ParameterDirection.Output> 来检索标识输出。 处理时 `InsertCommand` ，如果将插入命令的属性设置为或，则将返回自动递增的标识值并将其放置在当前行的 "字段**id** " 列中 <xref:System.Data.SqlClient.SqlCommand.UpdatedRowSource%2A> `UpdateRowSource.OutputParameters` `UpdateRowSource.Both` 。

如果插入命令执行的是同时包括 INSERT 语句和可返回新标识值的 SELECT 语句的批处理，则可以通过将插入命令的 `UpdatedRowSource` 属性设置为 `UpdateRowSource.FirstReturnedRecord` 来检索这个新值。

[!code-csharp[DataWorks SqlClient.RetrieveIdentityStoredProcedure#1](../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DataWorks SqlClient.RetrieveIdentityStoredProcedure/CS/source.cs#1)]
[!code-vb[DataWorks SqlClient.RetrieveIdentityStoredProcedure#1](../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DataWorks SqlClient.RetrieveIdentityStoredProcedure/VB/source.vb#1)]

## <a name="merging-new-identity-values"></a>合并新标识值

常见的方案是调用 `GetChanges` 的 `DataTable` 方法来创建仅包含已更改行的副本，并在调用 `Update` 的 `DataAdapter` 方法时使用这个新副本。 这在需要将已更改的行封送到执行更新的单独组件时十分有用。 更新后，此副本会包含随后必须合并回原始 `DataTable` 中的新标识值。 新标识值很可能会不同于 `DataTable` 中的原始值。 若要完成合并，必须保留副本中**自动增量**列的原始值，以便能够找到并更新原始行中的现有行 `DataTable` ，而不是追加包含新标识值的新行。 但在默认情况下，在调用 `Update` 的 `DataAdapter` 方法后，这些原始值会丢失，这是因为会对每个已更新的 `AcceptChanges` 隐式调用 `DataRow`。

在 `DataColumn` 更新期间，可以采用两种方法保留 `DataRow` 中 `DataAdapter` 的原始值：

- 第一种保留原始值的方法是将 `AcceptChangesDuringUpdate` 的 `DataAdapter` 属性设置为 `false`。 这会影响将要更新的 `DataRow` 中的每个 `DataTable`。 有关更多信息及代码示例，请参阅 <xref:System.Data.Common.DataAdapter.AcceptChangesDuringUpdate%2A>。

- 第二种方法是在 `RowUpdated` 的 `DataAdapter` 事件处理程序中编写代码，将 <xref:System.Data.Common.RowUpdatedEventArgs.Status%2A> 设置为 <xref:System.Data.UpdateStatus.SkipCurrentRow>。 `DataRow` 将会更新，但每个 `DataColumn` 的原始值会保留。 使用此方法可以对某些行保留原始值而不对其他行保留原始值。 例如，通过先检查 <xref:System.Data.Common.RowUpdatedEventArgs.StatementType%2A> 然后仅针对 <xref:System.Data.Common.RowUpdatedEventArgs.Status%2A> 为 <xref:System.Data.UpdateStatus.SkipCurrentRow> 的行将 `StatementType` 设置为 `Insert`，代码可以为已添加的行保留原始值，而不为已编辑或已删除的行保留原始值。

在 `DataRow` 更新期间使用这些方法中的任一方法保留 `DataAdapter` 中的原始值时，ADO.NET 会执行一系列操作将 `DataRow` 的当前值设置为由输出参数返回的新值或由结果集的第一个返回行返回的新值，同时仍在每个 `DataColumn` 中保留原始值。 首先调用 `AcceptChanges` 的 `DataRow` 方法将当前值保留为原始值，然后分配新值。 完成这些操作后，`DataRows` 属性设置为 <xref:System.Data.DataRow.RowState%2A> 的 <xref:System.Data.DataRowState.Added> 将会使其 `RowState` 属性设置为 <xref:System.Data.DataRowState.Modified>，这可能是不需要的行为。

如何将命令结果应用于每个要更新的 <xref:System.Data.DataRow> 由每个 <xref:System.Data.Common.DbCommand.UpdatedRowSource%2A> 的 <xref:System.Data.Common.DbCommand> 属性确定。 此属性设置为 `UpdateRowSource` 枚举中的一个值。

下表说明 `UpdateRowSource` 枚举值如何影响已更新行的 <xref:System.Data.DataRow.RowState%2A> 属性。

|成员名称|说明|
|-----------------|-----------------|
|<xref:System.Data.UpdateRowSource.Both>|调用 `AcceptChanges` 并将输出参数值和/或任何返回的数据集的第一行中的值放在要更新的 `DataRow` 中。 如果没有要应用的值，则 `RowState` 将为 <xref:System.Data.DataRowState.Unchanged>。|
|<xref:System.Data.UpdateRowSource.FirstReturnedRecord>|如果返回一个行，则调用 `AcceptChanges` 并将此行映射到 `DataTable` 中已更改的行，同时将 `RowState` 设置为 `Modified`。 如果未返回任何行，则不调用 `AcceptChanges`，`RowState` 将保持为 `Added`。|
|<xref:System.Data.UpdateRowSource.None>|忽略任何返回的参数或行。 不对 `AcceptChanges` 进行调用，`RowState` 将保持为 `Added`。|
|<xref:System.Data.UpdateRowSource.OutputParameters>|调用 `AcceptChanges` 并将任何输出参数映射到 `DataTable` 中已更改的行，同时将 `RowState` 设置为 `Modified`。 如果没有输出参数，则 `RowState` 将为 `Unchanged`。|

### <a name="example"></a>示例

此示例演示从 `DataTable` 中提取已更改的行，然后使用 <xref:System.Data.SqlClient.SqlDataAdapter> 更新数据源并检索新标识列值。 <xref:System.Data.SqlClient.SqlDataAdapter.InsertCommand%2A> 执行两个 Transact-SQL 语句：第一个是 INSERT 语句，第二个是使用 SCOPE_IDENTITY 函数检索标识值的 SELECT 语句。

```sql
INSERT INTO dbo.Shippers (CompanyName)
VALUES (@CompanyName);
SELECT ShipperID, CompanyName FROM dbo.Shippers
WHERE ShipperID = SCOPE_IDENTITY();
```

插入命令的 `UpdatedRowSource` 属性设置为 `UpdateRowSource.FirstReturnedRow` 并且 <xref:System.Data.MissingSchemaAction> 的 `DataAdapter` 属性设置为 `MissingSchemaAction.AddWithKey`。 填充 `DataTable` 并且代码向 `DataTable` 添加一个新行。 然后将已更改的行提取到一个新的 `DataTable` 中，将后者传递到 `DataAdapter`，它将随后更新服务器。

[!code-csharp[DataWorks SqlClient.MergeIdentity#1](../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DataWorks SqlClient.MergeIdentity/CS/source.cs#1)]
[!code-vb[DataWorks SqlClient.MergeIdentity#1](../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DataWorks SqlClient.MergeIdentity/VB/source.vb#1)]

`OnRowUpdated` 事件处理程序检查 <xref:System.Data.Common.RowUpdatedEventArgs.StatementType%2A> 的 <xref:System.Data.SqlClient.SqlRowUpdatedEventArgs> 以确定行是否为插入项。 如果是，则将 <xref:System.Data.Common.RowUpdatedEventArgs.Status%2A> 属性设置为 <xref:System.Data.UpdateStatus.SkipCurrentRow>。 该行将被更新，但会保留该行中的原始值。 在过程的正文中，调用 <xref:System.Data.DataSet.Merge%2A> 方法以将新标识值合并到原始 `DataTable` 中，最后调用 `AcceptChanges`。

[!code-csharp[DataWorks SqlClient.MergeIdentity#2](../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DataWorks SqlClient.MergeIdentity/CS/source.cs#2)]
[!code-vb[DataWorks SqlClient.MergeIdentity#2](../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DataWorks SqlClient.MergeIdentity/VB/source.vb#2)]

## <a name="retrieving-microsoft-access-autonumber-values"></a>检索 Microsoft Access Autonumber 值

本节包含一个演示如何从 Jet 4.0 数据库中检索 `Autonumber` 值的示例。 Jet 数据库引擎不支持在一个批处理中执行多个语句或使用输出参数，因此不能使用这些技术返回分配给已插入行的新 `Autonumber` 值。 但是，您可以向 `RowUpdated` 事件处理程序添加代码，该事件处理程序执行单独的 SELECT @ @IDENTITY 语句来检索新的 `Autonumber` 值。

### <a name="example"></a>示例

此示例不使用 `MissingSchemaAction.AddWithKey` 添加架构信息，而是在调用 `DataTable` 以填充 <xref:System.Data.OleDb.OleDbDataAdapter> 之前用正确的架构配置 `DataTable`。 在这种情况下，"**类别 id** " 列配置为从零开始为每个插入的行赋值，将设置 <xref:System.Data.DataColumn.AutoIncrement%2A> 为，将设置为 `true` 0，并将设置为 <xref:System.Data.DataColumn.AutoIncrementSeed%2A> <xref:System.Data.DataColumn.AutoIncrementStep%2A> -1。 然后，代码添加两个新行并使用 `GetChanges` 将已更改的行添加到传递给 `DataTable` 方法的新 `Update` 中。

[!code-csharp[DataWorks OleDb.JetAutonumberMerge#1](../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DataWorks OleDb.JetAutonumberMerge/CS/source.cs#1)]
[!code-vb[DataWorks OleDb.JetAutonumberMerge#1](../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DataWorks OleDb.JetAutonumberMerge/VB/source.vb#1)]

`RowUpdated` 事件处理程序使用相同的开放式 <xref:System.Data.OleDb.OleDbConnection> 作为 `Update` 的 `OleDbDataAdapter` 语句。 它会检查已插入行的 `StatementType` 的 <xref:System.Data.OleDb.OleDbRowUpdatedEventArgs>。 对于每个插入的行，将创建一个新的 <xref:System.Data.OleDb.OleDbCommand> ，以对连接执行 SELECT @ @IDENTITY 语句，并返回新 `Autonumber` 值（放置在的 "字段**id** " 列中） `DataRow` 。 然后将 `Status` 属性设置为 `UpdateStatus.SkipCurrentRow` 以取消对 `AcceptChanges` 的隐藏调用。 在过程的正文中，调用 `Merge` 方法以合并两个 `DataTable` 对象，最后调用 `AcceptChanges`。

[!code-csharp[DataWorks OleDb.JetAutonumberMerge#2](../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DataWorks OleDb.JetAutonumberMerge/CS/source.cs#2)]
[!code-vb[DataWorks OleDb.JetAutonumberMerge#2](../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DataWorks OleDb.JetAutonumberMerge/VB/source.vb#2)]

### <a name="retrieving-identity-values"></a>检索标识值

当列中的值必须唯一时，我们经常将该列设置为标识。 而且有时我们需要新数据的标识值。 此示例演示了如何检索标识值：

- 创建存储过程以插入数据和返回标识值。

- 执行命令以插入新数据并显示结果。

- 使用 <xref:System.Data.SqlClient.SqlDataAdapter> 以插入新数据并显示结果。

在你编译并运行该示例之前，你必须使用以下脚本创建示例数据库：

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
CREATE procedure [dbo].[CourseExtInfo] @CourseId int
as
select c.CourseID,c.Title,c.Credits,d.Name as DepartmentName
from Course as c left outer join Department as d on c.DepartmentID=d.DepartmentID
where c.CourseID=@CourseId

GO

SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
create procedure [dbo].[DepartmentInfo] @DepartmentId int,@CourseCount int output
as
select @CourseCount=Count(c.CourseID)
from course as c
where c.DepartmentID=@DepartmentId

select d.DepartmentID,d.Name,d.Budget,d.StartDate,d.Administrator
from Department as d
where d.DepartmentID=@DepartmentId

GO

SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
Create PROCEDURE [dbo].[GetDepartmentsOfSpecifiedYear]
@Year int,@BudgetSum money output
AS
BEGIN
        SELECT @BudgetSum=SUM([Budget])
  FROM [MySchool].[dbo].[Department]
  Where YEAR([StartDate])=@Year

SELECT [DepartmentID]
      ,[Name]
      ,[Budget]
      ,[StartDate]
      ,[Administrator]
  FROM [MySchool].[dbo].[Department]
  Where YEAR([StartDate])=@Year

END
GO

SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
CREATE PROCEDURE [dbo].[GradeOfStudent]
-- Add the parameters for the stored procedure here
@CourseTitle nvarchar(100),@FirstName nvarchar(50),
@LastName nvarchar(50),@Grade decimal(3,2) output
AS
BEGIN
select @Grade=Max(Grade)
from [dbo].[StudentGrade] as s join [dbo].[Course] as c on
s.CourseID=c.CourseID join [dbo].[Person] as p on s.StudentID=p.PersonID
where c.Title=@CourseTitle and p.FirstName=@FirstName
and p.LastName= @LastName
END
GO

SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
CREATE PROCEDURE [dbo].[InsertPerson]
-- Add the parameters for the stored procedure here
@FirstName nvarchar(50),@LastName nvarchar(50),
@PersonID int output
AS
BEGIN
    insert [dbo].[Person](LastName,FirstName) Values(@LastName,@FirstName)

    set @PersonID=SCOPE_IDENTITY()
END
Go

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

SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
SET ANSI_PADDING ON
GO
CREATE TABLE [dbo].[Person]([PersonID] [int] IDENTITY(1,1) NOT NULL,
[LastName] [nvarchar](50) NOT NULL,
[FirstName] [nvarchar](50) NOT NULL,
[HireDate] [datetime] NULL,
[EnrollmentDate] [datetime] NULL,
[Picture] [varbinary](max) NULL,
 CONSTRAINT [PK_School.Student] PRIMARY KEY CLUSTERED
(
[PersonID] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON) ON [PRIMARY]) ON [PRIMARY] TEXTIMAGE_ON [PRIMARY]

GO

SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
CREATE TABLE [dbo].[StudentGrade]([EnrollmentID] [int] IDENTITY(1,1) NOT NULL,
[CourseID] [nvarchar](10) NOT NULL,
[StudentID] [int] NOT NULL,
[Grade] [decimal](3, 2) NOT NULL,
 CONSTRAINT [PK_StudentGrade] PRIMARY KEY CLUSTERED
(
[EnrollmentID] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON) ON [PRIMARY]) ON [PRIMARY]

GO

SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
create view [dbo].[EnglishCourse]
as
select c.CourseID,c.Title,c.Credits,c.DepartmentID
from Course as c join Department as d on c.DepartmentID=d.DepartmentID
where d.Name=N'English'

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
SET IDENTITY_INSERT [dbo].[Person] ON

INSERT [dbo].[Person] ([PersonID], [LastName], [FirstName], [HireDate], [EnrollmentDate]) VALUES (1, N'Hu', N'Nan', NULL, CAST(0x0000A0BF00000000 AS DateTime))
INSERT [dbo].[Person] ([PersonID], [LastName], [FirstName], [HireDate], [EnrollmentDate]) VALUES (2, N'Norman', N'Laura', NULL, CAST(0x0000A0BF00000000 AS DateTime))
INSERT [dbo].[Person] ([PersonID], [LastName], [FirstName], [HireDate], [EnrollmentDate]) VALUES (3, N'Olivotto', N'Nino', NULL, CAST(0x0000A0BF00000000 AS DateTime))
INSERT [dbo].[Person] ([PersonID], [LastName], [FirstName], [HireDate], [EnrollmentDate]) VALUES (4, N'Anand', N'Arturo', NULL, CAST(0x0000A0BF00000000 AS DateTime))
INSERT [dbo].[Person] ([PersonID], [LastName], [FirstName], [HireDate], [EnrollmentDate]) VALUES (5, N'Jai', N'Damien', NULL, CAST(0x0000A0BF00000000 AS DateTime))
INSERT [dbo].[Person] ([PersonID], [LastName], [FirstName], [HireDate], [EnrollmentDate]) VALUES (6, N'Holt', N'Roger', CAST(0x000097F100000000 AS DateTime), NULL)
INSERT [dbo].[Person] ([PersonID], [LastName], [FirstName], [HireDate], [EnrollmentDate]) VALUES (7, N'Martin', N'Randall', CAST(0x00008B1A00000000 AS DateTime), NULL)
SET IDENTITY_INSERT [dbo].[Person] OFF
SET IDENTITY_INSERT [dbo].[StudentGrade] ON

INSERT [dbo].[StudentGrade] ([EnrollmentID], [CourseID], [StudentID], [Grade]) VALUES (1, N'C1045', 1, CAST(3.50 AS Decimal(3, 2)))
INSERT [dbo].[StudentGrade] ([EnrollmentID], [CourseID], [StudentID], [Grade]) VALUES (2, N'C1045', 2, CAST(3.00 AS Decimal(3, 2)))
INSERT [dbo].[StudentGrade] ([EnrollmentID], [CourseID], [StudentID], [Grade]) VALUES (3, N'C1045', 3, CAST(2.50 AS Decimal(3, 2)))
INSERT [dbo].[StudentGrade] ([EnrollmentID], [CourseID], [StudentID], [Grade]) VALUES (4, N'C1045', 4, CAST(4.00 AS Decimal(3, 2)))
INSERT [dbo].[StudentGrade] ([EnrollmentID], [CourseID], [StudentID], [Grade]) VALUES (5, N'C1045', 5, CAST(3.50 AS Decimal(3, 2)))
INSERT [dbo].[StudentGrade] ([EnrollmentID], [CourseID], [StudentID], [Grade]) VALUES (6, N'C1061', 1, CAST(4.00 AS Decimal(3, 2)))
INSERT [dbo].[StudentGrade] ([EnrollmentID], [CourseID], [StudentID], [Grade]) VALUES (7, N'C1061', 3, CAST(3.50 AS Decimal(3, 2)))
INSERT [dbo].[StudentGrade] ([EnrollmentID], [CourseID], [StudentID], [Grade]) VALUES (8, N'C1061', 4, CAST(2.50 AS Decimal(3, 2)))
INSERT [dbo].[StudentGrade] ([EnrollmentID], [CourseID], [StudentID], [Grade]) VALUES (9, N'C1061', 5, CAST(1.50 AS Decimal(3, 2)))
INSERT [dbo].[StudentGrade] ([EnrollmentID], [CourseID], [StudentID], [Grade]) VALUES (10, N'C2021', 1, CAST(2.50 AS Decimal(3, 2)))
INSERT [dbo].[StudentGrade] ([EnrollmentID], [CourseID], [StudentID], [Grade]) VALUES (11, N'C2021', 2, CAST(3.50 AS Decimal(3, 2)))
INSERT [dbo].[StudentGrade] ([EnrollmentID], [CourseID], [StudentID], [Grade]) VALUES (12, N'C2021', 4, CAST(3.00 AS Decimal(3, 2)))
INSERT [dbo].[StudentGrade] ([EnrollmentID], [CourseID], [StudentID], [Grade]) VALUES (13, N'C2021', 5, CAST(3.00 AS Decimal(3, 2)))
INSERT [dbo].[StudentGrade] ([EnrollmentID], [CourseID], [StudentID], [Grade]) VALUES (14, N'C2042', 1, CAST(2.00 AS Decimal(3, 2)))
INSERT [dbo].[StudentGrade] ([EnrollmentID], [CourseID], [StudentID], [Grade]) VALUES (15, N'C2042', 2, CAST(3.50 AS Decimal(3, 2)))
INSERT [dbo].[StudentGrade] ([EnrollmentID], [CourseID], [StudentID], [Grade]) VALUES (16, N'C2042', 3, CAST(4.00 AS Decimal(3, 2)))
INSERT [dbo].[StudentGrade] ([EnrollmentID], [CourseID], [StudentID], [Grade]) VALUES (17, N'C2042', 5, CAST(3.00 AS Decimal(3, 2)))
SET IDENTITY_INSERT [dbo].[StudentGrade] OFF
ALTER TABLE [dbo].[Course]  WITH CHECK ADD  CONSTRAINT [FK_Course_Department] FOREIGN KEY([DepartmentID])
REFERENCES [dbo].[Department] ([DepartmentID])
GO
ALTER TABLE [dbo].[Course] CHECK CONSTRAINT [FK_Course_Department]
GO
ALTER TABLE [dbo].[StudentGrade]  WITH CHECK ADD  CONSTRAINT [FK_StudentGrade_Student] FOREIGN KEY([StudentID])
REFERENCES [dbo].[Person] ([PersonID])
GO
ALTER TABLE [dbo].[StudentGrade] CHECK CONSTRAINT [FK_StudentGrade_Student]
GO
```

代码列表如下：

> [!TIP]
> 代码列表是指名为 MySchool.mdb 的 Access 数据库文件。 可以从[code.msdn.microsoft.com](https://code.msdn.microsoft.com/How-to-Retrieve-the-511acece)下载 myschool.mdb （作为完整 c # 或 Visual Basic 示例项目的一部分）。

```csharp
using System;
using System.Data;
using System.Data.OleDb;
using System.Data.SqlClient;

class Program {
   static void Main(string[] args) {
      String SqlDbConnectionString = "Data Source=(local);Initial Catalog=MySchool;Integrated Security=True;Asynchronous Processing=true;";

      InsertPerson(SqlDbConnectionString, "Janice", "Galvin");
      Console.WriteLine();

      InsertPersonInAdapter(SqlDbConnectionString, "Peter", "Krebs");
      Console.WriteLine();

      String oledbConnectionString = "Provider=Microsoft.Jet.OLEDB.4.0; Data Source=Database\\MySchool.mdb";
      InsertPersonInJet4Database(oledbConnectionString, "Janice", "Galvin");
      Console.WriteLine();

      Console.WriteLine("Please press any key to exit.....");
      Console.ReadKey();
   }

   // Using stored procedure to insert a new row and retrieve the identity value
   static void InsertPerson(String connectionString, String firstName, String lastName) {
      String commandText = "dbo.InsertPerson";

      using (SqlConnection conn = new SqlConnection(connectionString)) {
         using (SqlCommand cmd = new SqlCommand(commandText, conn)) {
            cmd.CommandType = CommandType.StoredProcedure;

            cmd.Parameters.Add(new SqlParameter("@FirstName", firstName));
            cmd.Parameters.Add(new SqlParameter("@LastName", lastName));
            SqlParameter personId = new SqlParameter("@PersonID", SqlDbType.Int);
            personId.Direction = ParameterDirection.Output;
            cmd.Parameters.Add(personId);

            conn.Open();
            cmd.ExecuteNonQuery();

            Console.WriteLine("Person Id of new person:{0}", personId.Value);
         }
      }
   }

   // Using stored procedure in adapter to insert new rows and update the identity value.
   static void InsertPersonInAdapter(String connectionString, String firstName, String lastName) {
      String commandText = "dbo.InsertPerson";
      using (SqlConnection conn = new SqlConnection(connectionString)) {
         SqlDataAdapter mySchool = new SqlDataAdapter("Select PersonID,FirstName,LastName from [dbo].[Person]", conn);

         mySchool.InsertCommand = new SqlCommand(commandText, conn);
         mySchool.InsertCommand.CommandType = CommandType.StoredProcedure;

         mySchool.InsertCommand.Parameters.Add(
             new SqlParameter("@FirstName", SqlDbType.NVarChar, 50, "FirstName"));
         mySchool.InsertCommand.Parameters.Add(
             new SqlParameter("@LastName", SqlDbType.NVarChar, 50, "LastName"));

         SqlParameter personId = mySchool.InsertCommand.Parameters.Add(new SqlParameter("@PersonID", SqlDbType.Int, 0, "PersonID"));
         personId.Direction = ParameterDirection.Output;

         DataTable persons = new DataTable();
         mySchool.Fill(persons);

         DataRow newPerson = persons.NewRow();
         newPerson["FirstName"] = firstName;
         newPerson["LastName"] = lastName;
         persons.Rows.Add(newPerson);

         mySchool.Update(persons);
         Console.WriteLine("Show all persons:");
         ShowDataTable(persons, 14);
      }
   }

   /// For a Jet 4.0 database, we need use the single statement and event handler to insert new rows and retrieve the identity value.
   static void InsertPersonInJet4Database(String connectionString, String firstName, String lastName) {
      String commandText = "Insert into Person(FirstName,LastName) Values(?,?)";
      using (OleDbConnection conn = new OleDbConnection(connectionString)) {
         OleDbDataAdapter mySchool = new OleDbDataAdapter("Select PersonID,FirstName,LastName from Person", conn);

         // Create Insert Command
         mySchool.InsertCommand = new OleDbCommand(commandText, conn);
         mySchool.InsertCommand.CommandType = CommandType.Text;

         mySchool.InsertCommand.Parameters.Add(new OleDbParameter("@FirstName", OleDbType.VarChar, 50, "FirstName"));
         mySchool.InsertCommand.Parameters.Add(new OleDbParameter("@LastName", OleDbType.VarChar, 50, "LastName"));
         mySchool.InsertCommand.UpdatedRowSource = UpdateRowSource.Both;

         DataTable persons = CreatePersonsTable();

         mySchool.Fill(persons);

         DataRow newPerson = persons.NewRow();
         newPerson["FirstName"] = firstName;
         newPerson["LastName"] = lastName;
         persons.Rows.Add(newPerson);

         DataTable dataChanges = persons.GetChanges();

         mySchool.RowUpdated += OnRowUpdated;

         mySchool.Update(dataChanges);

         Console.WriteLine("Data before merging:");
         ShowDataTable(persons, 14);
         Console.WriteLine();

         persons.Merge(dataChanges);
         persons.AcceptChanges();

         Console.WriteLine("Data after merging");
         ShowDataTable(persons, 14);
      }
   }

   static void OnRowUpdated(object sender, OleDbRowUpdatedEventArgs e) {
      if (e.StatementType == StatementType.Insert) {
         // Retrieve the identity value
         OleDbCommand cmdNewId = new OleDbCommand("Select @@IDENTITY", e.Command.Connection);
         e.Row["PersonID"] = (Int32)cmdNewId.ExecuteScalar();

         // After the status is changed, the original values in the row are preserved. And the
         // Merge method will be called to merge the new identity value into the original DataTable.
         e.Status = UpdateStatus.SkipCurrentRow;
      }
   }

   // Create the Persons table before filling.
   private static DataTable CreatePersonsTable() {
      DataTable persons = new DataTable();

      DataColumn personId = new DataColumn();
      personId.DataType = Type.GetType("System.Int32");
      personId.ColumnName = "PersonID";
      personId.AutoIncrement = true;
      personId.AutoIncrementSeed = 0;
      personId.AutoIncrementStep = -1;
      persons.Columns.Add(personId);

      DataColumn firstName = new DataColumn();
      firstName.DataType = Type.GetType("System.String");
      firstName.ColumnName = "FirstName";
      persons.Columns.Add(firstName);

      DataColumn lastName = new DataColumn();
      lastName.DataType = Type.GetType("System.String");
      lastName.ColumnName = "LastName";
      persons.Columns.Add(lastName);

      DataColumn[] pkey = { personId };
      persons.PrimaryKey = pkey;

      return persons;
   }

   private static void ShowDataTable(DataTable table, Int32 length) {
      foreach (DataColumn col in table.Columns) {
         Console.Write("{0,-" + length + "}", col.ColumnName);
      }
      Console.WriteLine();

      foreach (DataRow row in table.Rows) {
         foreach (DataColumn col in table.Columns) {
            if (col.DataType.Equals(typeof(DateTime)))
               Console.Write("{0,-" + length + ":d}", row[col]);
            else if (col.DataType.Equals(typeof(Decimal)))
               Console.Write("{0,-" + length + ":C}", row[col]);
            else
               Console.Write("{0,-" + length + "}", row[col]);
         }

         Console.WriteLine();
      }
   }
}
```

## <a name="see-also"></a>另请参阅

- [在 ADO.NET 中检索和修改数据](retrieving-and-modifying-data.md)
- [DataAdapter 和 DataReader](dataadapters-and-datareaders.md)
- [行状态和行版本](./dataset-datatable-dataview/row-states-and-row-versions.md)
- [AcceptChange 和 RejectChange](./dataset-datatable-dataview/acceptchanges-and-rejectchanges.md)
- [合并数据集内容](./dataset-datatable-dataview/merging-dataset-contents.md)
- [使用 DataAdapter 更新数据源](updating-data-sources-with-dataadapters.md)
- [ADO.NET 概述](ado-net-overview.md)
