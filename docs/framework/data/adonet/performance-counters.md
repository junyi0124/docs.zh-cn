---
title: 性能计数器
description: 使用 ADO.NET 性能计数器，通过使用 Windows 性能监视器或以编程方式监视应用程序状态及其连接资源。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 0b121b71-78f8-4ae2-9aa1-0b2e15778e57
ms.openlocfilehash: dca259ef6d8a2229349d88a9fcae3298cb8a829c
ms.sourcegitcommit: ecd9e9bb2225eb76f819722ea8b24988fe46f34c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/05/2020
ms.locfileid: "96739459"
---
# <a name="performance-counters-in-adonet"></a>ADO.NET 中的性能计数器

ADO.NET 2.0 引入了对性能计数器的扩展支持，包括对 <xref:System.Data.SqlClient> 和 <xref:System.Data.OracleClient> 的支持。 在早期版本的 ADO.NET 中提供的 <xref:System.Data.SqlClient> 性能计数器已被否决，并已替换为本主题讨论的新性能计数器。 可以使用 ADO.NET 性能计数器来监视应用程序的状态和应用程序所使用的连接资源。 可以使用 Windows 性能监视器来监视性能计数器，或使用 <xref:System.Diagnostics.PerformanceCounter> 命名空间中的 <xref:System.Diagnostics> 类以编程方式访问性能计数器。

## <a name="available-performance-counters"></a>可用的性能计数器

 当前有 14 个不同的性能计数器可用于下表中所描述的 <xref:System.Data.SqlClient> 和 <xref:System.Data.OracleClient>。 请注意，未本地化 Microsoft .NET Framework 各区域版本中的各个计数器的名称。

|性能计数器|说明|
|-------------------------|-----------------|
|`HardConnectsPerSecond`|每秒与数据库服务器连接的数量。|
|`HardDisconnectsPerSecond`|每秒与数据库服务器断开连接的数量。|
|`NumberOfActiveConnectionPoolGroups`|处于活动状态的唯一连接池组的数量。 此计数器由 AppDomain 中唯一连接字符串的数量控制。|
|`NumberOfActiveConnectionPools`|连接池的总数。|
|`NumberOfActiveConnections`|当前正在使用的活动连接的数量。 **注意：**  默认情况下不启用此性能计数器。 若要启用此性能计数器，请参阅 [激活默认的计数器](#ActivatingOffByDefault)。|
|`NumberOfFreeConnections`|连接池中可用连接的数量。 **注意：**  默认情况下不启用此性能计数器。 若要启用此性能计数器，请参阅 [激活默认的计数器](#ActivatingOffByDefault)。|
|`NumberOfInactiveConnectionPoolGroups`|标记为删除的唯一连接池组的数量。 此计数器由 AppDomain 中唯一连接字符串的数量控制。|
|`NumberOfInactiveConnectionPools`|最近未进行任何活动且等待被释放的非活动连接池的数量。|
|`NumberOfNonPooledConnections`|未存入池中的活动连接的数量。|
|`NumberOfPooledConnections`|由连接池基础结构管理的活动连接的数量。|
|`NumberOfReclaimedConnections`|通过垃圾回收而回收的连接的数量，其中应用程序未调用 `Close` 或 `Dispose`。 非显式关闭或释放连接会影响性能。|
|`NumberOfStasisConnections`|当前正在等待完成某项操作并因此无法供应用程序使用的连接的数量。|
|`SoftConnectsPerSecond`|从连接池中拉出的活动连接的数量。 **注意：**  默认情况下不启用此性能计数器。 若要启用此性能计数器，请参阅 [激活默认的计数器](#ActivatingOffByDefault)。|
|`SoftDisconnectsPerSecond`|被返回连接池的活动连接的数量。 **注意：**  默认情况下不启用此性能计数器。 若要启用此性能计数器，请参阅 [激活默认的计数器](#ActivatingOffByDefault)。|

### <a name="connection-pool-groups-and-connection-pools"></a>连接池组和连接池

 在使用 Windows 身份验证（集成安全性）时，必须监视 `NumberOfActiveConnectionPoolGroups` 和 `NumberOfActiveConnectionPools` 性能计数器。 这样做的原因是连接池组会映射为唯一连接字符串。 在使用集成安全性时，连接池会映射为连接字符串，此外，连接池还会为各个 Windows 标识创建单独的池。 例如，如果 Fred 和 Julie 在同一 AppDomain 中，并且二者都使用连接字符串 `"Data Source=MySqlServer;Integrated Security=true"`，则将为连接字符串创建一个连接池组，还将为 Fred 和 Julie 分别创建一个其他池。 如果 John 和 Martha 使用 SQL Server 登录名相同的连接字符串， `"Data Source=MySqlServer;User Id=lowPrivUser;Password=[PLACEHOLDER]"` 则只为 **lowPrivUser** 标识创建一个池。

<a name="ActivatingOffByDefault"></a>

### <a name="activating-off-by-default-counters"></a>激活默认情况下为关的计数器

 性能计数器 `NumberOfFreeConnections`、`NumberOfActiveConnections`、`SoftDisconnectsPerSecond` 和 `SoftConnectsPerSecond` 默认情况下为关。 将下面的信息添加到应用程序配置文件中，以启用这些信息：

```xml
<system.diagnostics>
  <switches>
    <add name="ConnectionPoolPerformanceCounterDetail"
         value="4"/>
  </switches>
</system.diagnostics>
```

## <a name="retrieving-performance-counter-values"></a>检索性能计数器的值

 下面的控制台应用程序演示如何在应用程序中检索性能计数器的值。 必须打开连接并且确保连接处于活动状态，才能为所有 ADO.NET 性能计数器返回信息。

> [!NOTE]
> 此示例使用 SQL Server 附带的 **AdventureWorks** 示例数据库。 在此示例代码中提供的连接字符串假定安装了数据库，该数据库在实例名为 SqlExpress 的本地计算机上；还假定你已创建了 SQL Server 登录，此登录与连接字符串中提供的登录相匹配。 如果使用仅允许 Windows 身份验证的默认安全设置来配置服务器，则需要启用 SQL Server 登录。 可以根据您的环境需要修改连接字符串。

### <a name="example"></a>示例

```vb
Option Explicit On
Option Strict On

Imports System.Data.SqlClient
Imports System.Diagnostics
Imports System.Runtime.InteropServices

Class Program

    Private PerfCounters(9) As PerformanceCounter
    Private connection As SqlConnection = New SqlConnection

    Public Shared Sub Main()
        Dim prog As Program = New Program
        ' Open a connection and create the performance counters.
        prog.connection.ConnectionString = _
           GetIntegratedSecurityConnectionString()
        prog.SetUpPerformanceCounters()
        Console.WriteLine("Available Performance Counters:")

        ' Create the connections and display the results.
        prog.CreateConnections()
        Console.WriteLine("Press Enter to finish.")
        Console.ReadLine()
    End Sub

    Private Sub CreateConnections()
        ' List the Performance counters.
        WritePerformanceCounters()

        ' Create 4 connections and display counter information.
        Dim connection1 As SqlConnection = New SqlConnection( _
           GetIntegratedSecurityConnectionString)
        connection1.Open()
        Console.WriteLine("Opened the 1st Connection:")
        WritePerformanceCounters()

        Dim connection2 As SqlConnection = New SqlConnection( _
           GetSqlConnectionStringDifferent)
        connection2.Open()
        Console.WriteLine("Opened the 2nd Connection:")
        WritePerformanceCounters()

        Console.WriteLine("Opened the 3rd Connection:")
        Dim connection3 As SqlConnection = New SqlConnection( _
           GetSqlConnectionString)
        connection3.Open()
        WritePerformanceCounters()

        Dim connection4 As SqlConnection = New SqlConnection( _
           GetSqlConnectionString)
        connection4.Open()
        Console.WriteLine("Opened the 4th Connection:")
        WritePerformanceCounters()

        connection1.Close()
        Console.WriteLine("Closed the 1st Connection:")
        WritePerformanceCounters()

        connection2.Close()
        Console.WriteLine("Closed the 2nd Connection:")
        WritePerformanceCounters()

        connection3.Close()
        Console.WriteLine("Closed the 3rd Connection:")
        WritePerformanceCounters()

        connection4.Close()
        Console.WriteLine("Closed the 4th Connection:")
        WritePerformanceCounters()
    End Sub

    Private Enum ADO_Net_Performance_Counters
        NumberOfActiveConnectionPools
        NumberOfReclaimedConnections
        HardConnectsPerSecond
        HardDisconnectsPerSecond
        NumberOfActiveConnectionPoolGroups
        NumberOfInactiveConnectionPoolGroups
        NumberOfInactiveConnectionPools
        NumberOfNonPooledConnections
        NumberOfPooledConnections
        NumberOfStasisConnections
        ' The following performance counters are more expensive to track.
        ' Enable ConnectionPoolPerformanceCounterDetail in your config file.
        '     SoftConnectsPerSecond
        '     SoftDisconnectsPerSecond
        '     NumberOfActiveConnections
        '     NumberOfFreeConnections
    End Enum

    Private Sub SetUpPerformanceCounters()
        connection.Close()
        Me.PerfCounters(9) = New PerformanceCounter()

        Dim instanceName As String = GetInstanceName()
        Dim apc As Type = GetType(ADO_Net_Performance_Counters)
        Dim i As Integer = 0
        Dim s As String = ""
        For Each s In [Enum].GetNames(apc)
            Me.PerfCounters(i) = New PerformanceCounter()
            Me.PerfCounters(i).CategoryName = ".NET Data Provider for SqlServer"
            Me.PerfCounters(i).CounterName = s
            Me.PerfCounters(i).InstanceName = instanceName
            i = (i + 1)
        Next
    End Sub

    Private Declare Function GetCurrentProcessId Lib "kernel32.dll" () As Integer

    Private Function GetInstanceName() As String
        'This works for Winforms apps.
        Dim instanceName As String = _
           System.Reflection.Assembly.GetEntryAssembly.GetName.Name

        ' Must replace special characters like (, ), #, /, \\
        Dim instanceName2 As String = _
           AppDomain.CurrentDomain.FriendlyName.ToString.Replace("(", "[") _
           .Replace(")", "]").Replace("#", "_").Replace("/", "_").Replace("\\", "_")

        'For ASP.NET applications your instanceName will be your CurrentDomain's
        'FriendlyName. Replace the line above that sets the instanceName with this:
        'instanceName = AppDomain.CurrentDomain.FriendlyName.ToString.Replace("(", "[") _
        '    .Replace(")", "]").Replace("#", "_").Replace("/", "_").Replace("\\", "_")

        Dim pid As String = GetCurrentProcessId.ToString
        instanceName = (instanceName + ("[" & (pid & "]")))
        Console.WriteLine("Instance Name: {0}", instanceName)
        Console.WriteLine("---------------------------")
        Return instanceName
    End Function

    Private Sub WritePerformanceCounters()
        Console.WriteLine("---------------------------")
        For Each p As PerformanceCounter In Me.PerfCounters
            Console.WriteLine("{0} = {1}", p.CounterName, p.NextValue)
        Next
        Console.WriteLine("---------------------------")
    End Sub

    Private Shared Function GetIntegratedSecurityConnectionString() As String
        ' To avoid storing the connection string in your code,
        ' you can retrieve it from a configuration file.
        Return ("Data Source=.\SqlExpress;Integrated Security=True;" &
          "Initial Catalog=AdventureWorks")
    End Function

    Private Shared Function GetSqlConnectionString() As String
        ' To avoid storing the connection string in your code,
        ' you can retrieve it from a configuration file.
        Return ("Data Source=.\SqlExpress;User Id=LowPriv;Password=[PLACEHOLDER];" &
          "Initial Catalog=AdventureWorks")
    End Function

    Private Shared Function GetSqlConnectionStringDifferent() As String
        ' To avoid storing the connection string in your code,
        ' you can retrieve it from a configuration file.
        Return ("Initial Catalog=AdventureWorks;Data Source=.\SqlExpress;" & _
          "User Id=LowPriv;Password=[PLACEHOLDER];")
    End Function
End Class
```

```csharp
using System;
using System.Data.SqlClient;
using System.Diagnostics;
using System.Runtime.InteropServices;

class Program
{
    PerformanceCounter[] PerfCounters = new PerformanceCounter[10];
    SqlConnection connection = new SqlConnection();

    static void Main()
    {
        Program prog = new Program();
        // Open a connection and create the performance counters.
        prog.connection.ConnectionString =
           GetIntegratedSecurityConnectionString();
        prog.SetUpPerformanceCounters();
        Console.WriteLine("Available Performance Counters:");

        // Create the connections and display the results.
        prog.CreateConnections();
        Console.WriteLine("Press Enter to finish.");
        Console.ReadLine();
    }

    private void CreateConnections()
    {
        // List the Performance counters.
        WritePerformanceCounters();

        // Create 4 connections and display counter information.
        SqlConnection connection1 = new SqlConnection(
              GetIntegratedSecurityConnectionString());
        connection1.Open();
        Console.WriteLine("Opened the 1st Connection:");
        WritePerformanceCounters();

        SqlConnection connection2 = new SqlConnection(
              GetSqlConnectionStringDifferent());
        connection2.Open();
        Console.WriteLine("Opened the 2nd Connection:");
        WritePerformanceCounters();

        SqlConnection connection3 = new SqlConnection(
              GetSqlConnectionString());
        connection3.Open();
        Console.WriteLine("Opened the 3rd Connection:");
        WritePerformanceCounters();

        SqlConnection connection4 = new SqlConnection(
              GetSqlConnectionString());
        connection4.Open();
        Console.WriteLine("Opened the 4th Connection:");
        WritePerformanceCounters();

        connection1.Close();
        Console.WriteLine("Closed the 1st Connection:");
        WritePerformanceCounters();

        connection2.Close();
        Console.WriteLine("Closed the 2nd Connection:");
        WritePerformanceCounters();

        connection3.Close();
        Console.WriteLine("Closed the 3rd Connection:");
        WritePerformanceCounters();

        connection4.Close();
        Console.WriteLine("Closed the 4th Connection:");
        WritePerformanceCounters();
    }

    private enum ADO_Net_Performance_Counters
    {
        NumberOfActiveConnectionPools,
        NumberOfReclaimedConnections,
        HardConnectsPerSecond,
        HardDisconnectsPerSecond,
        NumberOfActiveConnectionPoolGroups,
        NumberOfInactiveConnectionPoolGroups,
        NumberOfInactiveConnectionPools,
        NumberOfNonPooledConnections,
        NumberOfPooledConnections,
        NumberOfStasisConnections
        // The following performance counters are more expensive to track.
        // Enable ConnectionPoolPerformanceCounterDetail in your config file.
        //     SoftConnectsPerSecond
        //     SoftDisconnectsPerSecond
        //     NumberOfActiveConnections
        //     NumberOfFreeConnections
    }

    private void SetUpPerformanceCounters()
    {
        connection.Close();
        this.PerfCounters = new PerformanceCounter[10];
        string instanceName = GetInstanceName();
        Type apc = typeof(ADO_Net_Performance_Counters);
        int i = 0;
        foreach (string s in Enum.GetNames(apc))
        {
            this.PerfCounters[i] = new PerformanceCounter();
            this.PerfCounters[i].CategoryName = ".NET Data Provider for SqlServer";
            this.PerfCounters[i].CounterName = s;
            this.PerfCounters[i].InstanceName = instanceName;
            i++;
        }
    }

    [DllImport("kernel32.dll", SetLastError = true)]
    static extern int GetCurrentProcessId();

    private string GetInstanceName()
    {
        //This works for Winforms apps.
        string instanceName =
            System.Reflection.Assembly.GetEntryAssembly().GetName().Name;

        // Must replace special characters like (, ), #, /, \\
        string instanceName2 =
            AppDomain.CurrentDomain.FriendlyName.ToString().Replace('(', '[')
            .Replace(')', ']').Replace('#', '_').Replace('/', '_').Replace('\\', '_');

        // For ASP.NET applications your instanceName will be your CurrentDomain's
        // FriendlyName. Replace the line above that sets the instanceName with this:
        // instanceName = AppDomain.CurrentDomain.FriendlyName.ToString().Replace('(','[')
        // .Replace(')',']').Replace('#','_').Replace('/','_').Replace('\\','_');

        string pid = GetCurrentProcessId().ToString();
        instanceName = instanceName + "[" + pid + "]";
        Console.WriteLine("Instance Name: {0}", instanceName);
        Console.WriteLine("---------------------------");
        return instanceName;
    }

    private void WritePerformanceCounters()
    {
        Console.WriteLine("---------------------------");
        foreach (PerformanceCounter p in this.PerfCounters)
        {
            Console.WriteLine("{0} = {1}", p.CounterName, p.NextValue());
        }
        Console.WriteLine("---------------------------");
    }

    private static string GetIntegratedSecurityConnectionString()
    {
        // To avoid storing the connection string in your code,
        // you can retrieve it from a configuration file.
        return @"Data Source=.\SqlExpress;Integrated Security=True;" +
          "Initial Catalog=AdventureWorks";
    }
    private static string GetSqlConnectionString()
    {
        // To avoid storing the connection string in your code,
        // you can retrieve it from a configuration file.
        return @"Data Source=.\SqlExpress;User Id=LowPriv;Password=[PLACEHOLDER];" +
          "Initial Catalog=AdventureWorks";
    }

    private static string GetSqlConnectionStringDifferent()
    {
        // To avoid storing the connection string in your code,
        // you can retrieve it from a configuration file.
        return @"Initial Catalog=AdventureWorks;Data Source=.\SqlExpress;" +
          "User Id=LowPriv;Password=[PLACEHOLDER];";
    }
}
```

## <a name="see-also"></a>另请参阅

- [连接到数据源](connecting-to-a-data-source.md)
- [OLE DB、ODBC 和 Oracle 连接池](ole-db-odbc-and-oracle-connection-pooling.md)
- [ASP.NET 性能计数器](/previous-versions/aspnet/fxk122b4(v=vs.100))
- [运行时分析](../../debug-trace-profile/runtime-profiling.md)
- [监视性能阈值简介](/previous-versions/visualstudio/visual-studio-2008/bd20x32d(v=vs.90))
- [ADO.NET 概述](ado-net-overview.md)
