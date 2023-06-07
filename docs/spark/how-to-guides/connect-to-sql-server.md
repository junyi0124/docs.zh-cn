---
title: 将 .NET for Apache Spark 连接到 SQL Server
description: 了解如何从 .NET for Apache Spark 应用程序连接到 SQL Server 实例。
ms.author: nidutta
author: Niharikadutta
ms.date: 10/09/2020
ms.topic: conceptual
ms.custom: mvc,how-to
ms.openlocfilehash: 1fecd796aeefd6c5681c4c2ea623e89f3a5a3c1d
ms.sourcegitcommit: 30a686fd4377fe6472aa04e215c0de711bc1c322
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2020
ms.locfileid: "94439529"
---
# <a name="connect-net-for-apache-spark-to-sql-server"></a>将 .NET for Apache Spark 连接到 SQL Server

本文介绍如何从 [.NET for Apache Spark](https://github.com/dotnet/spark) 应用程序连接到 SQL Server 实例。

## <a name="configure-sql-server-to-grant-your-application-access"></a>配置 SQL Server 以授予应用程序访问权限

1. 将登录用户和密码添加到 SQL Server 实例的 SQL Server 身份验证。
2. 在相应的数据库级别向该登录用户授予所需的权限，如下所示：

    ![SQL Server 权限](./media/connect-external-sources/SqlServerAuth.png)

3. 请确保允许 SQL Server 的默认端口 `1433` 通过防火墙。
4. 打开 SQL 配置管理器，通过网络配置启用 TCP/IP，如下所示：

    ![启用 SQL Server TCP/IP](./media/connect-external-sources/SqlServerTCPIP.png)

    另请注意上图“协议”下的选项卡中“全部侦听”的值 。

5. 如果 `Listen All` 设置为 `No`，则针对所有必需的 IP 地址，将 TCP/IP 端口配置为 1433。 否则，请在 IPAll 中设置 TCP 端口。

    ![SQL Server TCP/IP 端口](./media/connect-external-sources/SQLServerTCPIIPPort.png)

## <a name="connect-to-sql-server-from-your-application"></a>从应用程序连接到 SQL Server

1. 使用 Microsoft JDBC Driver for SQL Server 通过你的应用程序提供数据库连接（从[此官方网站](/sql/connect/jdbc/download-microsoft-jdbc-driver-for-sql-server?view=sql-server-ver15)下载）。
2. 设置以下配置，以从应用程序连接到 SQL server 实例和数据库：
    1. **connection_url**：这是用于连接到 SQL Server 实例/数据库的 URL，格式如下：

        ```
        jdbc:sqlserver://<SQL_server_IP_address>:1433;instanceName=<instance_name>;databaseName=<database_name>;
        ```

    2. **dbtable**：要访问的表的名称。
    3. **user**：在配置 SQL Server 的步骤 1 中设置的登录用户。
    4. **password**：在配置 SQL Server 的步骤 1 中设置的用户密码。
3. 在应用程序代码中使用上述配置来读取表中的数据，如下所示：

    ```csharp
    static void Main()
    {
        SparkSession spark = SparkSession
            .Builder()
            .AppName("Connect to SQL Server")
            .GetOrCreate();

        string connection_url = "<URL to connect to SQL server instance>";
        string dbtable = "<database table to access>";
        string user = "<Login user name>";
        string password = "<Login user password>";

        DataFrame jdbcDF = spark.Read()
            .Format("jdbc")
            .Option("driver", "com.microsoft.sqlserver.jdbc.SQLServerDriver")
            .Option("url", connection_url)
            .Option("dbtable", dbtable)
            .Option("user", user)
            .Option("password", password).Load();
        jdbcDF.Show(); // Displays the content of the SQL table as a DataFrame
    }
    ```
