---
title: 如何：从数据库中删除行
description: 了解如何通过从与表相关的集合中删除 LINQ to SQL 对象来删除数据库中的行。 LINQ to SQL 将删除操作转换为 SQL DELETE 命令。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 2144c99b-8055-4080-a5c6-1ea14335e2a3
ms.openlocfilehash: d08621e834961e1db9312cac36bd2e69133142b5
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/02/2020
ms.locfileid: "84286386"
---
# <a name="how-to-delete-rows-from-the-database"></a>如何：从数据库中删除行

您可以通过删除与表相关的集合中的相应对象来删除数据库中的行 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 。 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)]将所做的更改转换为相应的 SQL `DELETE` 命令。

[!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 不支持且无法识别级联删除操作。 如果要在对行有约束的表中删除行，则必须完成以下任务之一：

- 在数据库的外键约束中设置 `ON DELETE CASCADE` 规则。

- 使用你自己的代码先删除阻止删除父对象的子对象。

 否则会引发异常。 请参见本主题中后面的第二个代码示例。

> [!NOTE]
> 您可以重写 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)]、`Insert` 和 `Update` 数据库操作的 `Delete` 默认方法。 有关详细信息，请参阅[自定义插入、更新和删除操作](customizing-insert-update-and-delete-operations.md)。
>
> 使用 Visual Studio 的开发人员可以使用对象关系设计器来开发用于实现相同目的的存储过程。

以下步骤假定您已通过有效的 <xref:System.Data.Linq.DataContext> 连接到 Northwind 数据库。 有关详细信息，请参阅[如何：连接到数据库](how-to-connect-to-a-database.md)。

### <a name="to-delete-a-row-in-the-database"></a>删除数据库中的行

1. 查询数据库中要删除的行。

2. 调用 <xref:System.Data.Linq.Table%601.DeleteOnSubmit%2A> 方法。

3. 将更改提交到数据库。

## <a name="example"></a>示例

这第一个代码示例查询数据库中 11000 号订单的详细信息，将这些订单详细信息标记为删除，然后将这些更改提交到数据库。

[!code-csharp[System.Data.Linq.Table#3](../../../../../../samples/snippets/csharp/VS_Snippets_Data/system.data.linq.table/cs/program.cs#3)]
[!code-vb[System.Data.Linq.Table#3](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/system.data.linq.table/vb/module1.vb#3)]

## <a name="example"></a>示例

在第二个示例中，目的是删除订单（10250 号）。 代码首先检查 `OrderDetails` 表以查看要删除的订单是否有子项。 如果订单有子项，则首先将子项标为删除，然后将订单标为删除。 <xref:System.Data.Linq.DataContext> 为实际删除设置正确的顺序，以使发送到数据库的删除命令遵守数据库约束。

[!code-csharp[DLinqCascadeWorkaround#1](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqCascadeWorkaround/cs/Program.cs#1)]
[!code-vb[DLinqCascadeWorkaround#1](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DLinqCascadeWorkaround/vb/Module1.vb#1)]

## <a name="see-also"></a>另请参阅

- [如何：管理更改冲突](how-to-manage-change-conflicts.md)
- [如何：分配存储流程来执行更新、插入和删除操作（O/R 设计器）](/visualstudio/data-tools/how-to-assign-stored-procedures-to-perform-updates-inserts-and-deletes-o-r-designer)
- [进行和提交数据更改](making-and-submitting-data-changes.md)
