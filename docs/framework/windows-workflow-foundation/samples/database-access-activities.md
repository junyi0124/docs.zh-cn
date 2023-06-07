---
title: 数据库访问活动
ms.date: 03/30/2017
ms.assetid: 174a381e-1343-46a8-a62c-7c2ae2c4f0b2
ms.openlocfilehash: ed3f0ad3f2fd19f622c9cb0faf7d5cd864b81995
ms.sourcegitcommit: 011314e0c8eb4cf4a11d92078f58176c8c3efd2d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/09/2020
ms.locfileid: "77094639"
---
# <a name="database-access-activities"></a>数据库访问活动

数据库访问活动可用于在一个工作流内访问数据库。 这些活动允许访问数据库以检索或修改信息，并使用[ADO.NET](../../data/adonet/index.md)来访问数据库。

> [!IMPORTANT]
> 您的计算机上可能已安装这些示例。 在继续操作之前，请先检查以下（默认）目录：
>
> `<InstallDrive>:\WF_WCF_Samples`
>
> 如果此目录不存在，请参阅（下载页）下载所有 Windows Communication Foundation （WCF）和 [!INCLUDE[wf1](../../../../includes/wf1-md.md)] 示例。 此示例位于以下目录：
>
> `<InstallDrive>:\WF_WCF_Samples\WF\Scenario\ActivityLibrary\DbActivities`

## <a name="database-activities"></a>数据库活动

以下部分详细介绍了此示例中包含的活动列表。

## <a name="dbupdate"></a>DbUpdate

执行可在数据库中产生修改（插入、更新、删除和其他修改）的 SQL 查询。

此类采用异步方式执行其工作（它派生自 <xref:System.Activities.AsyncCodeActivity> 并使用其异步功能）。

通过设置提供程序固定名称 (`ProviderName`) 和连接字符串 (`ConnectionString`)，或仅使用应用程序配置文件中的连接字符串配置名称 (`ConfigFileSectionName`)，可以配置连接信息。

要执行的查询在其 `Sql` 属性中配置，并通过 `Parameters` 集合传递参数。

执行 `DbUpdate` 后，在 `AffectedRecords` 属性中返回受影响的记录的数量。

```csharp
Public class DbUpdate: AsyncCodeActivity
{
    [RequiredArgument]
    [OverloadGroup("ConnectionString")]
    [DefaultValue(null)]
    public InArgument<string> ProviderName { get; set; }

    [RequiredArgument]
    [OverloadGroup("ConnectionString")]
    [DependsOn("ProviderName")]
    [DefaultValue(null)]
    public InArgument<string> ConnectionString { get; set; }

    [RequiredArgument]
    [OverloadGroup("ConfigFileSectionName")]
    [DefaultValue(null)]
    public InArgument<string> ConfigName { get; set; }

    [DefaultValue(null)]
    public CommandType CommandType { get; set; }

    [RequiredArgument]
    public InArgument<string> Sql { get; set; }

    [DependsOn("Sql")]
    [DefaultValue(null)]
    public IDictionary<string, Argument> Parameters { get; }

    [DependsOn("Parameters")]
    public OutArgument<int> AffectedRecords { get; set; }
}
```

|参数|说明|
|-|-|
|ProviderName|ADO.NET 提供程序固定名称。 如果设置此自变量，则必须还要设置 `ConnectionString`。|
|ConnectionString|用于连接到数据库的连接字符串。 如果设置此自变量，则必须还要设置 `ProviderName`。|
|ConfigName|存储连接信息的配置文件部分的名称。 设置此自变量之后，将不再需要 `ProviderName` 和 `ConnectionString`。|
|CommandType|要执行的 <xref:System.Data.Common.DbCommand> 的类型。|
|Sql|要执行的 SQL 命令。|
|parameters|SQL 查询的参数集合。|
|AffectedRecords|最后一个操作影响的记录的数量。|

## <a name="dbqueryscalar"></a>DbQueryScalar

执行可从数据库检索单个值的查询。

此类采用异步方式执行其工作（它派生自 <xref:System.Activities.AsyncCodeActivity%601> 并使用其异步功能）。

通过设置提供程序固定名称 (`ProviderName`) 和连接字符串 (`ConnectionString`)，或仅使用应用程序配置文件中的连接字符串配置名称 (`ConfigFileSectionName`)，可以配置连接信息。

要执行的查询在其 `Sql` 属性中配置，并通过 `Parameters` 集合传递参数。

执行 `DbQueryScalar` 后，将在 `Result out` 参数（类型 `TResult`，在基类 <xref:System.Activities.AsyncCodeActivity%601>中定义）中返回标量。

```csharp
public class DbQueryScalar<TResult> : AsyncCodeActivity<TResult>
{
    // public arguments
    [RequiredArgument]
    [OverloadGroup("ConnectionString")]
    [DefaultValue(null)]
    public InArgument<string> ProviderName { get; set; }

    [RequiredArgument]
    [OverloadGroup("ConnectionString")]
    [DependsOn("ProviderName")]
    [DefaultValue(null)]
    public InArgument<string> ConnectionString { get; set; }

    [RequiredArgument]
    [OverloadGroup("ConfigFileSectionName")]
    [DefaultValue(null)]
    public InArgument<string> ConfigName { get; set; }

    [DefaultValue(null)]
    public CommandType CommandType { get; set; }

    [RequiredArgument]
    public InArgument<string> Sql { get; set; }

    [DependsOn("Sql")]
    [DefaultValue(null)]
    public IDictionary<string, Argument> Parameters { get; }
}
```

|参数|说明|
|-|-|
|ProviderName|ADO.NET 提供程序固定名称。 如果设置此自变量，则必须还要设置 `ConnectionString`。|
|ConnectionString|用于连接到数据库的连接字符串。 如果设置此自变量，则必须还要设置 `ProviderName`。|
|ConfigName|存储连接信息的配置文件部分的名称。 设置此自变量之后，将不再需要 `ProviderName` 和 `ConnectionString`。|
|CommandType|要执行的 <xref:System.Data.Common.DbCommand> 的类型。|
|Sql|要执行的 SQL 命令。|
|parameters|SQL 查询的参数集合。|
|结果|执行查询后获得的标量。 此自变量的类型为 `TResult`。|

## <a name="dbquery"></a>DbQuery

执行可检索对象列表的查询。 执行查询后，将执行一个映射函数（它可以 <xref:System.Func%601><`DbDataReader`、`TResult`> 或 <xref:System.Activities.ActivityFunc%601><`DbDataReader``TResult`>）。 此映射函数在 `DbDataReader` 中获取一个记录，并将其映射到要返回的对象。

通过设置提供程序固定名称 (`ProviderName`) 和连接字符串 (`ConnectionString`)，或仅使用应用程序配置文件中的连接字符串配置名称 (`ConfigFileSectionName`)，可以配置连接信息。

要执行的查询在其 `Sql` 属性中配置，并通过 `Parameters` 集合传递参数。

使用 `DbDataReader` 检索 SQL 查询的结果。 此活动将循环访问 `DbDataReader`，并将 `DbDataReader` 中的行映射到 `TResult` 的实例。 `DbQuery` 的用户必须提供映射代码，可以通过两种方式完成此操作：使用 <xref:System.Func%601><`DbDataReader`、`TResult`> 或 <xref:System.Activities.ActivityFunc%601><`DbDataReader``TResult`>。 在第一种情况下，将在单个执行脉冲中完成映射。 因此，此方法的速度更快，但无法序列化为 XAML。 在后一种情况下，将在多个脉冲中完成映射。 因此，此方法的速度较慢，但可序列化为 XAML，并以声明方式进行创作（任何现有活动均可参与映射）。

```csharp
public class DbQuery<TResult> : AsyncCodeActivity<IList<TResult>> where TResult : class
{
    // public arguments
    [RequiredArgument]
    [OverloadGroup("ConnectionString")]
    [DefaultValue(null)]
    public InArgument<string> ProviderName { get; set; }

    [RequiredArgument]
    [OverloadGroup("ConnectionString")]
    [DependsOn("ProviderName")]
    [DefaultValue(null)]
    public InArgument<string> ConnectionString { get; set; }

    [RequiredArgument]
    [OverloadGroup("ConfigFileSectionName")]
    [DefaultValue(null)]
    public InArgument<string> ConfigName { get; set; }

    [DefaultValue(null)]
    public CommandType CommandType { get; set; }

    [RequiredArgument]
    public InArgument<string> Sql { get; set; }

    [DependsOn("Sql")]
    [DefaultValue(null)]
    public IDictionary<string, Argument> Parameters { get; }

    [OverloadGroup("DirectMapping")]
    [DefaultValue(null)]
    public Func<DbDataReader, TResult> Mapper { get; set; }

    [OverloadGroup("MultiplePulseMapping")]
    [DefaultValue(null)]
    public ActivityFunc<DbDataReader, TResult> MapperFunc { get; set; }
}
```

|参数|说明|
|-|-|
|ProviderName|ADO.NET 提供程序固定名称。 如果设置此自变量，则必须还要设置 `ConnectionString`。|
|ConnectionString|用于连接到数据库的连接字符串。 如果设置此自变量，则必须还要设置 `ProviderName`。|
|ConfigName|存储连接信息的配置文件部分的名称。 设置此自变量之后，将不再需要 `ProviderName` 和 `ConnectionString`。|
|CommandType|要执行的 <xref:System.Data.Common.DbCommand> 的类型。|
|Sql|要执行的 SQL 命令。|
|parameters|SQL 查询的参数集合。|
|Mapper|映射函数（<xref:System.Func%601><`DbDataReader`，`TResult`>），该函数采用在执行查询后获得的 `DataReader` 中的记录，并返回要添加到 `TResult` 集合的类型 `Result` 的对象的实例。<br /><br /> 在这种情况下，将在单个执行脉冲中完成映射，但不能使用设计器以声明方式创作它。|
|MapperFunc|映射函数（<xref:System.Activities.ActivityFunc%601><`DbDataReader`，`TResult`>），该函数采用在执行查询后获得的 `DataReader` 中的记录，并返回要添加到 `TResult` 集合的类型 `Result` 的对象的实例。<br /><br /> 在这种情况下，将在多个执行脉冲中完成映射。 此函数可序列化为 XAML，并以声明方式进行创作（任何现有活动均可参与映射）。|
|结果|对象列表，这些对象是通过执行查询并对 `DataReader` 中的每个记录执行映射函数得到的。|

## <a name="dbquerydataset"></a>DbQueryDataSet

执行可返回 <xref:System.Data.DataSet> 的查询。 此类以异步方式执行其工作。 它派生自 <xref:System.Activities.AsyncCodeActivity><`TResult`> 并使用其异步功能。

通过设置提供程序固定名称 (`ProviderName`) 和连接字符串 (`ConnectionString`)，或仅使用应用程序配置文件中的连接字符串配置名称 (`ConfigFileSectionName`)，可以配置连接信息。

要执行的查询在其 `Sql` 属性中配置，并通过 `Parameters` 集合传递参数。

执行 `DbQueryDataSet` 后，`DataSet` 将返回到 `Result out` 参数（类型 `TResult`，该参数在基类 <xref:System.Activities.AsyncCodeActivity%601>中定义）。

```csharp
public class DbQueryDataSet : AsyncCodeActivity<DataSet>
{
    // public arguments
    [RequiredArgument]
    [OverloadGroup("ConnectionString")]
    [DefaultValue(null)]
    public InArgument<string> ProviderName { get; set; }

    [RequiredArgument]
    [OverloadGroup("ConnectionString")]
    [DependsOn("ProviderName")]
    [DefaultValue(null)]
    public InArgument<string> ConnectionString { get; set; }

    [RequiredArgument]
    [OverloadGroup("ConfigFileSectionName")]
    [DefaultValue(null)]
    public InArgument<string> ConfigName { get; set; }

    [DefaultValue(null)]
    public CommandType CommandType { get; set; }

    [RequiredArgument]
    public InArgument<string> Sql { get; set; }

    [DependsOn("Sql")]
    [DefaultValue(null)]
    public IDictionary<string, Argument> Parameters { get; }
}
```

|参数|说明|
|-|-|
|ProviderName|ADO.NET 提供程序固定名称。 如果设置此自变量，则必须还要设置 `ConnectionString`。|
|ConnectionString|用于连接到数据库的连接字符串。 如果设置此自变量，则必须还要设置 `ProviderName`。|
|ConfigName|存储连接信息的配置文件部分的名称。 设置此自变量之后，将不再需要 `ProviderName` 和 `ConnectionString`。|
|CommandType|要执行的 <xref:System.Data.Common.DbCommand> 的类型。|
|Sql|要执行的 SQL 命令。|
|parameters|SQL 查询的参数集合。|
|结果|执行查询后获得的 <xref:System.Data.DataSet>。|

## <a name="configuring-connection-information"></a>配置连接信息

所有 DbActivities 共享相同的配置参数。 可以通过两种方式进行相关配置：

- `ConnectionString + InvariantName`：设置 ADO.NET 提供程序固定名称和连接字符串。

  ```csharp
  Activity dbSelectCount = new DbQueryScalar<DateTime>()
  {
      ProviderName = "System.Data.SqlClient",
      ConnectionString = @"Data Source=.\SQLExpress;
                            Initial Catalog=DbActivitiesSample;
                            Integrated Security=True",
      Sql = "SELECT GetDate()"
  };
  ```

- `ConfigName`：设置包含连接信息的配置部分的名称。

  ```xml
  <connectionStrings>
      <add name="DbActivitiesSample"
            providerName="System.Data.SqlClient"
            connectionString="Data Source=.\SQLExpress;Initial Catalog=DbActivitiesSample;Integrated Security=true"/>
    </connectionStrings>
  ```

- 在活动中：

  ```csharp
  Activity dbSelectCount = new DbQueryScalar<int>()
  {
      ConfigName = "DbActivitiesSample",
      Sql = "SELECT COUNT(*) FROM Roles"
  };
  ```

## <a name="running-this-sample"></a>运行此示例

### <a name="setup-instructions"></a>设置说明

此示例使用一个数据库。 此示例提供了一个安装和加载脚本 (Setup.cmd)。 必须使用命令提示执行该文件。

Setup.cmd 脚本调用 CreateDb.sql 脚本文件，该文件包含可执行下列操作的 SQL 命令：

- 创建一个名为 DbActivitiesSample 的数据库。

- 创建 Roles 表。

- 创建 Employees 表。

- 将 3 个记录插入到 Roles 表中。

- 将 12 个记录插入到 Employees 表中。

##### <a name="to-run-setupcmd"></a>运行 Setup.cmd

1. 打开命令提示符。

2. 转到 DbActivities 示例文件夹。

3. 键入 "setup .cmd"，然后按 ENTER。

    > [!NOTE]
    > Setup.cmd 尝试将此示例安装在您本地计算机的 SqlExpress 实例中。 如果您需要在其他 SQL Server 实例中安装它，请将 Setup.cmd 改为使用新的实例名称。

##### <a name="to-uninstall-the-sample-database"></a>卸载示例数据库

1. 在命令提示中运行示例文件夹中的 Cleanup.cmd。

##### <a name="to-run-the-sample"></a>运行示例

1. 在 Visual Studio 2010 中打开解决方案

2. 若要编译解决方案，请按 CTRL+SHIFT+B。

3. 若要运行示例而不进行调试，按 Ctrl+F5。

> [!IMPORTANT]
> 您的计算机上可能已安装这些示例。 在继续操作之前，请先检查以下（默认）目录：
>
> `<InstallDrive>:\WF_WCF_Samples`
>
> 如果此目录不存在，请参阅[.NET Framework 4 的 Windows Communication Foundation （wcf）和 Windows Workflow Foundation （WF）示例](https://www.microsoft.com/download/details.aspx?id=21459)以下载所有 WINDOWS COMMUNICATION FOUNDATION （wcf）和 [!INCLUDE[wf1](../../../../includes/wf1-md.md)] 示例。 此示例位于以下目录：
>
> `<InstallDrive>:\WF_WCF_Samples\WF\Scenario\ActivityLibrary\DbActivities`
