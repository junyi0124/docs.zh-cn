---
title: 合并数据集内容
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: e5e9309a-3ebb-4a9c-9d78-21c4e2bafc5b
ms.openlocfilehash: abc9183666602a7ef369e690e3ae499f8c7b8b11
ms.sourcegitcommit: d2e1dfa7ef2d4e9ffae3d431cf6a4ffd9c8d378f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/07/2019
ms.locfileid: "70784394"
---
# <a name="merging-dataset-contents"></a>合并数据集内容

您可以使用 <xref:System.Data.DataSet.Merge%2A> 方法将 <xref:System.Data.DataSet>、<xref:System.Data.DataTable> 或 <xref:System.Data.DataRow> 数组的内容合并到现有的 `DataSet` 中。 若干因素和选项会影响将新数据合并到现有 `DataSet` 中的方式。

## <a name="primary-keys"></a>主键

如果从合并接收新数据和架构的表具有主键，则传入数据中的新行将匹配具有与传入数据中的这些行相同 <xref:System.Data.DataRowVersion.Original> 主键值的现有行。 如果传入架构中的列匹配现有架构的相应列，则会修改现有行中的数据。 将会忽略或添加与现有架构不匹配的列，具体根据 <xref:System.Data.Common.DataAdapter.MissingSchemaAction%2A> 参数而定。 主键值与任何现有行都不匹配的新行将追加到现有表中。

如果传入行或现有行的行状态为 <xref:System.Data.DataRowState.Added>，则将使用 <xref:System.Data.DataRowVersion.Current> 行的 `Added` 主键值来匹配他们的主键值，这是因为不存在 `Original` 行版本。

如果传入表和现有表包含一个具有相同名称但不同数据类型的列，将引发异常，并引发 <xref:System.Data.DataSet.MergeFailed> 的 `DataSet` 事件。 如果传入表和现有表都具有已定义的键，但是主键针对不同的列，将引发异常，并引发 `MergeFailed` 的 `DataSet` 事件。

如果从合并中接收新数据的表不具有主键，传入数据中的新行将无法与该表中的现有行匹配，而这些新行则会追加到现有表。

## <a name="table-names-and-namespaces"></a>表名称和命名空间

可以选择为 <xref:System.Data.DataTable> 对象分配 <xref:System.Data.DataTable.Namespace%2A> 属性值。 如果分配了 <xref:System.Data.DataTable.Namespace%2A> 值，则一个 <xref:System.Data.DataSet> 可以包含多个具有相同 <xref:System.Data.DataTable> 值的 <xref:System.Data.DataTable.TableName%2A> 对象。 在合并操作期间，<xref:System.Data.DataTable.TableName%2A> 和 <xref:System.Data.DataTable.Namespace%2A> 都用于标识合并目标。 如果未分配 <xref:System.Data.DataTable.Namespace%2A>，则只使用 <xref:System.Data.DataTable.TableName%2A> 来标识合并目标。

> [!NOTE]
> 这种行为在 .NET Framework 2.0 版中发生了变化。 在 1.1 版中，虽然支持命名空间，但会在合并操作过程中被忽略。 因此，使用 <xref:System.Data.DataSet> 属性值的 <xref:System.Data.DataTable.Namespace%2A> 将具有不同的行为，具体取决于您运行哪个版本的 .NET Framework。 例如，假设您有两个 `DataSets`，其中包含具有相同 `DataTables` 属性值但不同 <xref:System.Data.DataTable.TableName%2A> 属性值的 <xref:System.Data.DataTable.Namespace%2A>。 在 .NET Framework 1.1 版中，在合并两个 <xref:System.Data.DataTable.Namespace%2A> 对象时，将会忽略不同的 <xref:System.Data.DataSet> 名称。 但从 2.0 版开始，合并操作会导致在目标 `DataTables` 中创建两个新的 <xref:System.Data.DataSet>。 原始的 `DataTables` 将不受合并的影响。

## <a name="preservechanges"></a>PreserveChanges

在将 `DataSet`、`DataTable` 或 `DataRow` 数组传递给 `Merge` 方法时，可以包括可选参数用以指定是否在现有 `DataSet` 中保留更改以及如何处理在传入数据中发现的新架构元素。 在传入数据后面，这些数据的第一个参数是一个布尔型标志 <xref:System.Data.LoadOption.PreserveChanges>，它指定是否在现有 `DataSet` 中保留更改。 如果 `PreserveChanges` 标志设置为 `true`，则传入值不会覆盖现有行的 `Current` 行版本中的现有值。 如果 `PreserveChanges` 标志设置为 `false`，则传入值将覆盖现有行的 `Current` 行版本中的现有值。 如果未指定 `PreserveChanges` 标志，默认情况下它将设置为 `false`。 有关行版本的详细信息，请参阅[行状态和行版本](row-states-and-row-versions.md)。

如果 `PreserveChanges` 为 `true`，则现有行的 <xref:System.Data.DataRowVersion.Current> 行版本中将保持现有行中的数据，而现有行的 <xref:System.Data.DataRowVersion.Original> 行版本中的数据将由传入行的 `Original` 行版本中的数据覆盖。 现有行的 <xref:System.Data.DataRow.RowState%2A> 设置为 <xref:System.Data.DataRowState.Modified>。 存在以下例外：

- 如果现有行的 `RowState` 为 `Deleted`，则此 `RowState` 将保持为 `Deleted` 而不设置为 `Modified`。 在这种情况下，传入行中的数据将仍然存储在现有行的 `Original` 行版本中，并覆盖现有行的 `Original` 行版本（除非传入行的 `RowState` 为 `Added`）。

- 如果传入行的 `RowState` 为 `Added`，则现有行的 `Original` 行版本中的数据将不会由传入行中的数据覆盖，因为传入行不具有 `Original` 行版本。

如果 `PreserveChanges` 为 `false`，则现有行中的 `Current` 和 `Original` 行版本将都由传入行中的数据覆盖，并且现有行的 `RowState` 将设置为传入行的 `RowState`。 存在以下例外：

- 如果传入行的 `RowState` 为 `Unchanged` 且现有行的 `RowState` 为 `Modified`、`Deleted` 或 `Added`，则现有行的 `RowState` 将设置为 `Modified`。

- 如果传入行的 `RowState` 为 `Added` 且现有行的 `RowState` 为 `Unchanged`、`Modified` 或 `Deleted`，则现有行的 `RowState` 将设置为 `Modified`。 此外，现有行的 `Original` 行版本中的数据不会由传入行中的数据覆盖，因为传入行不具有 `Original` 行版本。

## <a name="missingschemaaction"></a>MissingSchemaAction

您可以使用 <xref:System.Data.MissingSchemaAction> 方法的可选 `Merge` 参数来指定 `Merge` 将如何处理传入数据中不属于现有 `DataSet` 的架构元素。

下表说明用于 `MissingSchemaAction` 的选项。

|MissingSchemaAction 选项|描述|
|--------------------------------|-----------------|
|<xref:System.Data.MissingSchemaAction.Add>|将新的架构信息添加到 `DataSet` 并用传入值填充新列。 这是默认设置。|
|<xref:System.Data.MissingSchemaAction.AddWithKey>|将新的架构和主键信息添加到 `DataSet` 并用传入值填充新列。|
|<xref:System.Data.MissingSchemaAction.Error>|如果遇到不匹配的架构信息，则引发异常。|
|<xref:System.Data.MissingSchemaAction.Ignore>|忽略新的架构信息。|

## <a name="constraints"></a>约束

使用 `Merge` 方法时，所有新数据都添加到现有 `DataSet` 之前不会检查约束。 添加了数据后，会对 `DataSet` 中的当前值实施约束。 你必须确保你的代码可以处理可能因约束冲突而引发的任何异常。

设想有这样一种情况：`DataSet` 中的某一现有行是主键值为 1 的 `Unchanged`。 在与 `Modified` 主键值为 2 且 `Original` 主键值为 1 的 `Current` 传入行进行合并期间，由于 `Original` 主键值不同，现有行和传入行将被视为不匹配。 不过，完成合并和检查完约束后，将会引发一个异常，因为 `Current` 主键值违反了主键列的唯一约束。

> [!NOTE]
> 在向包含自动递增列（如标识列）的数据库表中插入行时，插入时操作返回的标识列值可能不匹配 `DataSet` 中的值，从而导致追加而不是合并返回的行。 有关详细信息，请参阅[检索标识或自动编号值](../retrieving-identity-or-autonumber-values.md)。

下面的代码示例将具有`DataSet`不同架构的两个对象`DataSet`合并为一个，其中包含两个`DataSet`传入对象的组合架构。

[!code-csharp[DataWorks DataSet.Merge#1](../../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DataWorks DataSet.Merge/CS/source.cs#1)]
[!code-vb[DataWorks DataSet.Merge#1](../../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DataWorks DataSet.Merge/VB/source.vb#1)]

下面的代码示例采用带有更新的现有 `DataSet`，并将这些更新传递给 `DataAdapter` 以便在数据源中处理。 随后将结果合并到原始 `DataSet` 中。 在拒绝导致错误的更改之后，合并的更改与 `AcceptChanges` 一起提交。

[!code-csharp[DataWorks DataSet.MergeAcceptChanges#1](../../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DataWorks DataSet.MergeAcceptChanges/CS/source.cs#1)]
[!code-vb[DataWorks DataSet.MergeAcceptChanges#1](../../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DataWorks DataSet.MergeAcceptChanges/VB/source.vb#1)]

[!code-csharp[DataWorks DataSet.MergeAcceptChanges#2](../../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DataWorks DataSet.MergeAcceptChanges/CS/source.cs#2)]
[!code-vb[DataWorks DataSet.MergeAcceptChanges#2](../../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DataWorks DataSet.MergeAcceptChanges/VB/source.vb#2)]

## <a name="see-also"></a>请参阅

- [数据集、数据表和数据视图](index.md)
- [行状态和行版本](row-states-and-row-versions.md)
- [DataAdapters 和 DataReaders](../dataadapters-and-datareaders.md)
- [在 ADO.NET 中检索和修改数据](../retrieving-and-modifying-data.md)
- [检索标识或自动编号值](../retrieving-identity-or-autonumber-values.md)
- [ADO.NET 概述](../ado-net-overview.md)
