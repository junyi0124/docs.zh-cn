---
title: 演练：使用 BatchBlock 和 BatchedJoinBlock 提高效率
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- Task Parallel Library, dataflows
- TPL dataflow library, improving efficiency
ms.assetid: 5beb4983-80c2-4f60-8c51-a07f9fd94cb3
ms.openlocfilehash: d9c4b2d5cfab28f10be82724f46660e4b42ce410
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94829916"
---
# <a name="walkthrough-using-batchblock-and-batchedjoinblock-to-improve-efficiency"></a>演练：使用 BatchBlock 和 BatchedJoinBlock 提高效率

TPL 数据流库提供 <xref:System.Threading.Tasks.Dataflow.BatchBlock%601?displayProperty=nameWithType> 和 <xref:System.Threading.Tasks.Dataflow.BatchedJoinBlock%602?displayProperty=nameWithType> 类，以便可以接收和缓冲一个或多个源的数据，再将缓冲的数据作为一个集合传播出去。 如果从一个或多个源收集数据，再批处理多个数据元素，就会发现这种批处理机制非常有用。 例如，假设应用使用数据流将记录插入数据库。 如果同时插入多项，而不是顺序一次插入一个，此操作可能会更高效。 本文档介绍了如何使用 <xref:System.Threading.Tasks.Dataflow.BatchBlock%601> 类，提高此类数据库插入操作的效率。 它还介绍了如何使用 <xref:System.Threading.Tasks.Dataflow.BatchedJoinBlock%602> 类，捕获程序从数据库读取数据时的结果和发生的任何异常。

[!INCLUDE [tpl-install-instructions](../../../includes/tpl-install-instructions.md)]

## <a name="prerequisites"></a>系统必备

1. 开始本演练前，请先阅读[数据流](dataflow-task-parallel-library.md)文档中的“联接块”部分。

2. 确保计算机上有 Northwind 数据库的副本 Northwind.sdf。 此文件通常位于 %Program Files%\Microsoft SQL Server Compact Edition\v3.5\Samples\\ 文件夹中。

    > [!IMPORTANT]
    > 在一些版本的 Windows 中，如果以非管理员模式运行 Visual Studio，便无法连接到 Northwind.sdf。 若要连接到 Northwind.sdf，请在“以管理员身份运行”模式下启动 Visual Studio 或 Visual Studio 开发人员命令提示  。

本演练包含以下各节：

- [创建控制台应用](#creating)

- [定义 Employee 类](#employeeClass)

- [定义员工数据库操作](#operations)

- [不使用缓冲将员工数据添加到数据库](#nonBuffering)

- [使用缓冲将员工数据添加到数据库](#buffering)

- [使用已缓冲联接读取数据库中的员工数据](#bufferedJoin)

- [完整示例](#complete)

<a name="creating"></a>

## <a name="creating-the-console-application"></a>创建控制台应用

1. 在 Visual Studio 中，创建 Visual C# 或 Visual Basic“控制台应用程序”  项目。 在本文档中，该项目名为 `DataflowBatchDatabase`。

2. 在项目中，添加对 System.Data.SqlServerCe.dll 和 System.Threading.Tasks.Dataflow.dll 的引用。

3. 确保 Form1.cs（对于 Visual Basic，则为 Form1.vb）包含以下 `using`（Visual Basic 中为 `Imports`）语句。

    [!code-csharp[TPLDataflow_BatchDatabase#1](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_batchdatabase/cs/dataflowbatchdatabase.cs#1)]
    [!code-vb[TPLDataflow_BatchDatabase#1](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpldataflow_batchdatabase/vb/dataflowbatchdatabase.vb#1)]

4. 将以下数据成员添加到 `Program` 类。

    [!code-csharp[TPLDataflow_BatchDatabase#2](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_batchdatabase/cs/dataflowbatchdatabase.cs#2)]
    [!code-vb[TPLDataflow_BatchDatabase#2](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpldataflow_batchdatabase/vb/dataflowbatchdatabase.vb#2)]

<a name="employeeClass"></a>

## <a name="defining-the-employee-class"></a>定义 Employee 类

向 `Program` 类添加 `Employee` 类。

[!code-csharp[TPLDataflow_BatchDatabase#3](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_batchdatabase/cs/dataflowbatchdatabase.cs#3)]
[!code-vb[TPLDataflow_BatchDatabase#3](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpldataflow_batchdatabase/vb/dataflowbatchdatabase.vb#3)]

`Employee` 类包含下面三个属性：`EmployeeID`、`LastName` 和 `FirstName`。 这些属性对应于 Northwind 数据库中 `Employees` 表的 `Employee ID`、`Last Name` 和 `First Name` 列。 在展示的此示例中，`Employee` 类还定义了 `Random` 方法，用于创建属性值为随机值的 `Employee` 对象。

<a name="operations"></a>

## <a name="defining-employee-database-operations"></a>定义员工数据库操作

向 `Program` 添加 `InsertEmployees`、`GetEmployeeCount` 和 `GetEmployeeID` 方法。

[!code-csharp[TPLDataflow_BatchDatabase#4](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_batchdatabase/cs/dataflowbatchdatabase.cs#4)]
[!code-vb[TPLDataflow_BatchDatabase#4](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpldataflow_batchdatabase/vb/dataflowbatchdatabase.vb#4)]

`InsertEmployees` 方法将新员工记录添加到数据库。 `GetEmployeeCount` 方法检索 `Employees` 表中的条目数。 `GetEmployeeID` 方法检索与所提供姓名匹配的首位员工的标识符。 这三个方法全都需要对 Northwind 数据库使用连接字符串，并使用 `System.Data.SqlServerCe` 命名空间中的功能与数据库进行通信。

<a name="nonBuffering"></a>

## <a name="adding-employee-data-to-the-database-without-using-buffering"></a>不使用缓冲将员工数据添加到数据库

向 `Program` 类添加 `AddEmployees` 和 `PostRandomEmployees` 方法。

[!code-csharp[TPLDataflow_BatchDatabase#5](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_batchdatabase/cs/dataflowbatchdatabase.cs#5)]
[!code-vb[TPLDataflow_BatchDatabase#5](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpldataflow_batchdatabase/vb/dataflowbatchdatabase.vb#5)]

`AddEmployees` 方法使用数据流将随机员工数据添加到数据库。 此方法创建 <xref:System.Threading.Tasks.Dataflow.ActionBlock%601> 对象，以调用 `InsertEmployees` 方法将员工条目添加到数据库。 然后，`AddEmployees` 方法调用 `PostRandomEmployees` 方法，将多个 `Employee` 对象发布到 <xref:System.Threading.Tasks.Dataflow.ActionBlock%601> 对象。 然后，`AddEmployees` 方法等待所有插入操作完成。

<a name="buffering"></a>

## <a name="using-buffering-to-add-employee-data-to-the-database"></a>使用缓冲将员工数据添加到数据库

向 `Program` 类添加 `AddEmployeesBatched` 方法。

[!code-csharp[TPLDataflow_BatchDatabase#6](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_batchdatabase/cs/dataflowbatchdatabase.cs#6)]
[!code-vb[TPLDataflow_BatchDatabase#6](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpldataflow_batchdatabase/vb/dataflowbatchdatabase.vb#6)]

此方法类似于 `AddEmployees`，不同之处在于它还先使用 <xref:System.Threading.Tasks.Dataflow.BatchBlock%601> 类缓冲多个 `Employee` 对象，然后再将这些对象发送给 <xref:System.Threading.Tasks.Dataflow.ActionBlock%601> 对象。 由于 <xref:System.Threading.Tasks.Dataflow.BatchBlock%601> 类将多个元素以集合形式传播出去，因此 <xref:System.Threading.Tasks.Dataflow.ActionBlock%601> 对象被修改为对 `Employee` 对象的数组执行操作。 与 `AddEmployees` 方法类似，`AddEmployeesBatched` 调用 `PostRandomEmployees` 方法发布多个 `Employee` 对象；不同之处在于，`AddEmployeesBatched` 将这些对象发布到 <xref:System.Threading.Tasks.Dataflow.BatchBlock%601> 对象。 `AddEmployeesBatched` 方法还等待所有插入操作完成。

<a name="bufferedJoin"></a>

## <a name="using-buffered-join-to-read-employee-data-from-the-database"></a>使用已缓冲联接读取数据库中的员工数据

向 `Program` 类添加 `GetRandomEmployees` 方法。

[!code-csharp[TPLDataflow_BatchDatabase#7](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_batchdatabase/cs/dataflowbatchdatabase.cs#7)]
[!code-vb[TPLDataflow_BatchDatabase#7](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpldataflow_batchdatabase/vb/dataflowbatchdatabase.vb#7)]

此方法将随机员工信息打印到控制台中。 它会创建多个随机 `Employee` 对象，并调用 `GetEmployeeID` 方法检索每个对象的唯一标识符。 由于 `GetEmployeeID` 方法在找不到与给定姓氏和名字匹配的员工时抛出异常，因此 `GetRandomEmployees` 方法使用 <xref:System.Threading.Tasks.Dataflow.BatchedJoinBlock%602> 类存储 `GetEmployeeID` 成功调用对应的 `Employee` 对象，以及失败调用对应的 <xref:System.Exception?displayProperty=nameWithType> 对象。 此示例中的 <xref:System.Threading.Tasks.Dataflow.ActionBlock%601> 对象对保留 `Employee` 对象列表和 <xref:System.Exception> 对象列表的 <xref:System.Tuple%602> 对象执行操作。 如果收到的 `Employee` 和 <xref:System.Exception> 对象数总和等于批大小，<xref:System.Threading.Tasks.Dataflow.BatchedJoinBlock%602> 对象就会传播出此类数据。

<a name="complete"></a>

## <a name="the-complete-example"></a>完整示例

以下示例显示了完整的代码。 `Main` 方法比较执行批量数据库插入和非批量数据库插入所需的时间。 它还展示了如何使用已缓冲联接，读取数据库中的员工数据并报告错误。

[!code-csharp[TPLDataflow_BatchDatabase#100](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_batchdatabase/cs/dataflowbatchdatabase.cs#100)]
[!code-vb[TPLDataflow_BatchDatabase#100](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpldataflow_batchdatabase/vb/dataflowbatchdatabase.vb#100)]

## <a name="see-also"></a>另请参阅

- [数据流](dataflow-task-parallel-library.md)
