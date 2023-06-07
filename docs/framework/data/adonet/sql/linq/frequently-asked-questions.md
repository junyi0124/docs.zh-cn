---
title: 常见问题
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 252ed666-0679-4eea-b71b-2f14117ef443
ms.openlocfilehash: 3cc879e97438138554f1d39cf588e01bfbba28a6
ms.sourcegitcommit: 7bc6887ab658550baa78f1520ea735838249345e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/03/2020
ms.locfileid: "75634693"
---
# <a name="frequently-asked-questions"></a>常见问题

以下各节介绍了实现 LINQ 时可能会遇到的一些常见问题。

其他问题在[疑难解答](troubleshooting.md)中得到了解决。

## <a name="cannot-connect"></a>无法连接

问： 我无法连接到数据库。

答： 请确保您的连接字符串正确且您的 SQL Server 实例正在运行。 另请注意，[!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 要求启用命名管道协议。 有关详细信息，请参阅[通过演练学习](learning-by-walkthroughs.md)。

## <a name="changes-to-database-lost"></a>对数据库的更改丢失

问： 我对数据库中的数据进行了更改，但是在重新运行应用程序时更改已丢失。

答： 请确保您调用了 <xref:System.Data.Linq.DataContext.SubmitChanges%2A> 来将结果保存到数据库。

## <a name="database-connection-open-how-long"></a>数据库连接：可以打开多长时间？

问： 我的数据库连接可以保持打开状态多长时间？

答： 一个连接通常会一直保持打开状态，直至您使用完查询结果为止。 如果您需要花时间处理所有结果并且愿意对结果进行缓存，请将 <xref:System.Linq.Enumerable.ToList%2A> 应用于查询。 在每个对象仅处理一次的常见方案中，流模型比 `DataReader` 和 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 都更为适合。

连接使用的准确详细信息与以下内容有关：

- 使用连接对象构造 <xref:System.Data.Linq.DataContext> 时的连接状态。

- 连接字符串设置（例如，启用多活动结果集 (MARS)）。 有关详细信息，请参阅[多个活动结果集 (MARS)](../multiple-active-result-sets-mars.md)。

## <a name="updating-without-querying"></a>在不进行查询的情况下更新

问： 是否可以在不先查询数据库的情况下更新表数据？

答： 虽然 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 没有基于集的更新命令，但是可以使用以下方法之一进行更新而无需先进行查询：

- 使用 <xref:System.Data.Linq.DataContext.ExecuteCommand%2A> 发送 SQL 代码。

- 创建对象的新实例，并初始化所有影响更新的当前值（字段）。 然后使用 <xref:System.Data.Linq.DataContext> 将对象附加到 <xref:System.Data.Linq.Table%601.Attach%2A> 并修改您要更改的字段。

## <a name="unexpected-query-results"></a>意外的查询结果

问： 我的查询返回了意外的结果。 如何检查所发生的情况？

答： [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 提供了几种工具用于检查其生成的 SQL 代码。 其中最重要的工具就是 <xref:System.Data.Linq.DataContext.Log%2A>。 有关详细信息，请参阅[调试支持](debugging-support.md)。

## <a name="unexpected-stored-procedure-results"></a>意外的存储过程结果

问： 我有一个存储过程，其返回值由 `MAX()` 进行计算。 当我将该存储过程拖到 O/R 设计器图面时，返回值是不正确的。

答： [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 提供了两种方法来通过存储过程返回数据库生成的值：

- 通过命名输出结果。

- 通过显式指定输出参数。

下面是错误输出的示例。 因为 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 无法映射结果，所以始终返回 0：

```sql
create procedure proc2

as

begin

select max(i) from t where name like 'hello'

end
```

下面是使用输出参数获得正确输出的示例：

```sql
create procedure proc2

@result int OUTPUT

as

select @result = MAX(i) from t where name like 'hello'

go
```

下面是通过命名输出结果获得正确输出的示例：

```sql
create procedure proc2

as

begin

select nax(i) AS MaxResult from t where name like 'hello'

end
```

有关详细信息，请参阅[使用存储过程自定义操作](customizing-operations-by-using-stored-procedures.md)。

## <a name="serialization-errors"></a>序列化错误

问： 当我尝试序列化时，出现以下错误： "Type ' ChangeTracker + StandardChangeTracker ' .。。未标记为可序列化。 "

答： [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 中的代码生成支持 <xref:System.Runtime.Serialization.DataContractSerializer> 序列化， 而不支持 <xref:System.Xml.Serialization.XmlSerializer> 或 <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter>。 有关详细信息，请参阅[序列化](serialization.md)。

## <a name="multiple-dbml-files"></a>多个 DBML 文件

问： 如果我有多个 DBML 文件共享一些公用的表，我会收到一个编译器错误消息。

答： 将对象关系设计器的**上下文命名空间**和**实体命名空间**属性设置为每个 DBML 文件的非重复值。 此方法可以避免名称/命名空间冲突。

## <a name="avoiding-explicit-setting-of-database-generated-values-on-insert-or-update"></a>避免在插入或更新时显式设置数据库生成的值

问： 我的一个数据库表具有一个默认为 SQL `DateCreated` 的 `Getdate()` 列。 在我试图使用 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 插入新记录时，该值会设置为 `NULL`。 我希望其设置为数据库默认值。

答： [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 会自动为标识（自动增加）和 rowguidcol（数据库生成的 GUID）以及时间戳列处理这种情况。 在其他情况下，应手动设置 <xref:System.Data.Linq.Mapping.ColumnAttribute.IsDbGenerated%2A>=`true` 并 <xref:System.Data.Linq.Mapping.ColumnAttribute.AutoSync%2A>=<xref:System.Data.Linq.Mapping.AutoSync.Always>/<xref:System.Data.Linq.Mapping.AutoSync.OnInsert>/属性。

## <a name="multiple-dataloadoptions"></a>多个 DataLoadOptions

问： 是否可以指定其他加载选项而不覆盖原先的选项？

答： 可以。 不会覆盖原先的选项，如下面的示例中所示：

```vb
Dim dlo As New DataLoadOptions()
dlo.LoadWith(Of Order)(Function(o As Order) o.Customer)
dlo.LoadWith(Of Order)(Function(o As Order) o.OrderDetails)
```

```csharp
DataLoadOptions dlo = new DataLoadOptions();
dlo.LoadWith<Order>(o => o.Customer);
dlo.LoadWith<Order>(o => o.OrderDetails);
```

## <a name="errors-using-sql-compact-35"></a>使用 SQL Compact 3.5 时的错误

问： 将表拖出 SQL Server Compact 3.5 数据库时出错。

答： 对象关系设计器不支持 SQL Server Compact 3.5，不过 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 运行时也是如此。 在这种情况下，必须创建您自己的实体类并添加合适的属性。

## <a name="errors-in-inheritance-relationships"></a>继承关系中的错误

问： 我使用了对象关系设计器中的工具箱继承形状连接两个实体，但却出现错误。

答： 仅创建关系是不够的。 还必须提供其他信息，例如鉴别器列、基类鉴别器值和派生类鉴别器值。

## <a name="provider-model"></a>提供程序模型

问： 是否有公共提供程序模型可用？

答： 没有任何公共提供程序模型可用。 目前 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 仅支持 SQL Server 和 SQL Server Compact 3.5。

## <a name="sql-injection-attacks"></a>SQL 注入式攻击

问： [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 如何防范 SQL 注入式攻击？

答： 对于通过串联用户输入而组成的传统 SQL 查询，SQL 注入已成为重大风险。 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 通过在查询中使用 <xref:System.Data.SqlClient.SqlParameter> 来避免这种注入。 用户输入会转换为参数值。 此方法可防止通过客户输入使用恶意命令。

## <a name="changing-read-only-flag-in-dbml-files"></a>更改 DBML 文件中的只读标志

问： 如何在从 DBML 文件创建对象模型时消除某些属性中的设置器？

答： 对于此高级方案，请执行以下步骤：

1. 在 .dbml 文件中，通过将 <xref:System.Data.Linq.ITable.IsReadOnly%2A> 标志更改为 `True` 来修改属性。

2. 添加一个分部类。 为只读成员创建一个带参数的构造函数。

3. 检查默认的 <xref:System.Data.Linq.Mapping.UpdateCheck> 值 (<xref:System.Data.Linq.Mapping.UpdateCheck.Never>) 以确定该值对于您的应用程序是否正确。

    > [!CAUTION]
    > 如果你使用的是 Visual Studio 中的对象关系设计器，则可能会覆盖你所做的更改。

## <a name="aptca"></a>APTCA

问： System.Data.Linq 是否标记为供部分受信任的代码使用？

答： 是的，System.web 程序集位于用 <xref:System.Security.AllowPartiallyTrustedCallersAttribute> 属性标记的 .NET Framework 程序集中。 如果没有此标记，.NET Framework 中的程序集将仅供完全受信任的代码使用。

允许部分受信任的调用方 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 中的主要方案是允许从 Web 应用程序访问 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 程序集，其中的*信任*配置为 Medium。

## <a name="mapping-data-from-multiple-tables"></a>映射来自多个表的数据

问： 我的实体中的数据来自多个表。 如果映射这些数据？

答： 您可以在数据库中创建一个视图并将实体映射到该视图。 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 会为视图生成 SQL，与它为表生成 SQL 相同。

> [!NOTE]
> 这种情况下的视图用法有一些限制。 当基础视图支持在 <xref:System.Data.Linq.Table%601> 上执行的操作时，此方法最为安全。 只有您知道要执行的操作。 例如，大多数应用程序都是只读的，而另一个就前往数字 `Create`仅通过对视图使用存储过程来 /`Update`/`Delete` 操作。

## <a name="connection-pooling"></a>连接池

问： 是否具有对 <xref:System.Data.Linq.DataContext> 池有所帮助的构造？

答： 请不要试图重用 <xref:System.Data.Linq.DataContext> 的实例。 每个 <xref:System.Data.Linq.DataContext> 都会保持对应一个特定编辑/查询会话的状态（包括标识缓存）。 若要获取基于数据库当前状态的新实例，请使用新的 <xref:System.Data.Linq.DataContext>。

你仍可以使用基础 ADO.NET 连接池。 有关详细信息，请参阅 [SQL Server 连接池 (ADO.NET)](../../sql-server-connection-pooling.md)。

## <a name="second-datacontext-is-not-updated"></a>第二个 DataContext 未更新

问： 我使用 <xref:System.Data.Linq.DataContext> 的一个实例存储数据库中的值。 但是，相同数据库上的另一个 <xref:System.Data.Linq.DataContext> 未反映更新的值。 第二个 <xref:System.Data.Linq.DataContext> 实例似乎返回缓存的值。

答： 此行为是设计使然。 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 会继续返回您在第一个实例中看到的相同实例/值。 在进行更新时使用开放式并发。 原始数据用于检查当前数据库状态，以确定该数据实际上仍未更改。 如果该数据已更改，则会发生冲突，您的应用程序必须解决该冲突。 您的应用程序可以选择将原始状态重置为当前数据库状态并尝试再次更新。 有关详细信息，请参阅[如何：管理更改冲突](how-to-manage-change-conflicts.md)。

您也可以将 <xref:System.Data.Linq.DataContext.ObjectTrackingEnabled%2A> 设置为 false，这样可以关闭缓存和更改跟踪。 然后便可以在每次查询时检索最新值。

## <a name="cannot-call-submitchanges-in-read-only-mode"></a>无法在只读模式下调用 SubmitChanges

问： 当我试图在只读模式下调用 <xref:System.Data.Linq.DataContext.SubmitChanges%2A> 时收到错误消息。

答： 只读模式关闭了上下文跟踪更改的功能。

## <a name="see-also"></a>另请参阅

- [引用](reference.md)
- [疑难解答](troubleshooting.md)
- [LINQ to SQL 中的安全性](security-in-linq-to-sql.md)
