---
title: 如何：更新数据库中的行
description: 了解如何通过修改与表相关的集合中的 LINQ to SQL 对象来更新数据库中的行。 LINQ to SQL 将添加内容转换为 SQL UPDATE 命令。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: a2b5c90f-6cc3-4128-bfab-1db488d5af26
ms.openlocfilehash: f25efb91fb5a83fb1c7c109bd018c8210edaec8b
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/02/2020
ms.locfileid: "84286334"
---
# <a name="how-to-update-rows-in-the-database"></a>如何：更新数据库中的行

您可以通过修改与集合关联的对象的成员值 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] <xref:System.Data.Linq.Table%601> ，然后将更改提交到数据库来更新数据库中的行。 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)]将所做的更改转换为相应的 SQL `UPDATE` 命令。

> [!NOTE]
> 您可以重写 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)]、`Insert` 和 `Update` 数据库操作的 `Delete` 默认方法。 有关详细信息，请参阅[自定义插入、更新和删除操作](customizing-insert-update-and-delete-operations.md)。
>
> 使用 Visual Studio 的开发人员可以使用对象关系设计器来开发用于实现相同目的的存储过程。

以下步骤假定您已通过有效的 <xref:System.Data.Linq.DataContext> 连接到 Northwind 数据库。 有关详细信息，请参阅[如何：连接到数据库](how-to-connect-to-a-database.md)。

### <a name="to-update-a-row-in-the-database"></a>更新数据库中的行

1. 查询数据库中要更新的行。

2. 对得到的 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 对象中的成员值进行所需的更改。

3. 将更改提交到数据库。

## <a name="example"></a>示例

下面的示例查询数据库中编号为 11000 的订单，然后更改所得到的 `ShipName` 对象中的 `ShipVia` 和 `Order` 值。 最后，对这些成员值的更改将作为对 `ShipName` 和 `ShipVia` 列的更改提交到数据库。

[!code-csharp[System.Data.Linq.Table#2](../../../../../../samples/snippets/csharp/VS_Snippets_Data/system.data.linq.table/cs/program.cs#2)]
[!code-vb[System.Data.Linq.Table#2](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/system.data.linq.table/vb/module1.vb#2)]

## <a name="see-also"></a>另请参阅

- [如何：管理更改冲突](how-to-manage-change-conflicts.md)
- [如何：分配存储流程来执行更新、插入和删除操作（O/R 设计器）](/visualstudio/data-tools/how-to-assign-stored-procedures-to-perform-updates-inserts-and-deletes-o-r-designer)
- [进行和提交数据更改](making-and-submitting-data-changes.md)
