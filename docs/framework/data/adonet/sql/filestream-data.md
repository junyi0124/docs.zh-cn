---
title: FILESTREAM 数据
ms.date: 03/30/2017
ms.assetid: bd8b845c-0f09-4295-b466-97ef106eefa8
ms.openlocfilehash: 87bed5dd345c240cc00b2c36aa976ec53fe63b93
ms.sourcegitcommit: d2e1dfa7ef2d4e9ffae3d431cf6a4ffd9c8d378f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/07/2019
ms.locfileid: "70794096"
---
# <a name="filestream-data"></a>FILESTREAM 数据

FILESTREAM 存储特性用于 varbinary(max) 列中存储的二进制 (BLOB) 数据。 在 FILESTREAM 之前，存储二进制数据需要特殊处理。 非结构化的数据（例如文本文档、图像和视频）通常存储在数据库之外，从而使得难以管理此类数据。

> [!NOTE]
> 您必须安装 .NET Framework 3.5 SP1（或更高版本）才能使用 SqlClient 处理 FILESTREAM 数据。

在 varbinary(max) 列上指定 FILESTREAM 属性可使 SQL Server 将数据存储在本地 NTFS 文件系统中，而不是存储在数据库文件中。 虽然数据是单独存储的，但您可以使用所支持的用于处理存储在数据库中的 varbinary(max) 数据的相同 Transact-SQL 语句。

## <a name="sqlclient-support-for-filestream"></a>SqlClient 对 FILESTREAM 的支持

用于 SQL Server <xref:System.Data.SqlClient>的 .NET Framework 数据提供程序支持使用在<xref:System.Data.SqlTypes>命名空间中定义的<xref:System.Data.SqlTypes.SqlFileStream>类读取和写入 FILESTREAM 数据。 `SqlFileStream` 继承自 <xref:System.IO.Stream> 类，该类提供了用于读写数据流的方法。 从流读取数据可将数据从流传输到一个数据结构，例如一个字节数组。 写入操作可将数据从该数据结构传输到一个流。

### <a name="creating-the-sql-server-table"></a>创建 SQL Server 表

下列 Transact-SQL 语句将创建一个名为 employees 的表并插入一行数据。 启用 FILESTREAM 存储之后，你可以将此表与下面的代码示例结合使用。 本主题末尾的 SQL Server 联机丛书中的资源链接。

```sql
CREATE TABLE employees
(
  EmployeeId INT  NOT NULL  PRIMARY KEY,
  Photo VARBINARY(MAX) FILESTREAM  NULL,
  RowGuid UNIQUEIDENTIFIER  NOT NULL  ROWGUIDCOL
  UNIQUE DEFAULT NEWID()
)
GO
Insert into employees
Values(1, 0x00, default)
GO
```

### <a name="example-reading-overwriting-and-inserting-filestream-data"></a>示例：读取、覆盖和插入 FILESTREAM 数据

下面的示例演示如何从 FILESTREAM 读取数据。 示例代码获取文件的逻辑路径，并将 `FileAccess` 设置为 `Read`，而将 `FileOptions` 设置为 `SequentialScan`。 然后，示例代码将字节从 SqlFileStream 读入到缓存区中， 最后将这些字节写入到控制台窗口。

该示例还演示如何将数据写入 FILESTREAM（其中，所有现有的数据都将被覆盖）。 示例代码获取文件的逻辑路径，然后创建 `SqlFileStream`，并将 `FileAccess` 设置为 `Write`，而将 `FileOptions` 设置为 `SequentialScan`。 将一个单字节写入到 `SqlFileStream`，从而替换文件中的任何数据。

该示例还演示了如何通过使用 Seek 方法将数据追加到文件末尾将数据写入 FILESTREAM。 示例代码获取文件的逻辑路径，然后创建 `SqlFileStream`，并将 `FileAccess` 设置为 `ReadWrite`，而将 `FileOptions` 设置为 `SequentialScan`。 示例代码使用 Seek 方法找到文件结尾，并将一个单字节追加到现有的文件中。

```csharp
using System;
using System.Data.SqlClient;
using System.Data.SqlTypes;
using System.Data;
using System.IO;

namespace FileStreamTest
{
    class Program
    {
        static void Main(string[] args)
        {
            SqlConnectionStringBuilder builder = new SqlConnectionStringBuilder("server=(local);integrated security=true;database=myDB");
            ReadFileStream(builder);
            OverwriteFileStream(builder);
            InsertFileStream(builder);

            Console.WriteLine("Done");
        }

        private static void ReadFileStream(SqlConnectionStringBuilder connStringBuilder)
        {
            using (SqlConnection connection = new SqlConnection(connStringBuilder.ToString()))
            {
                connection.Open();
                SqlCommand command = new SqlCommand("SELECT TOP(1) Photo.PathName(), GET_FILESTREAM_TRANSACTION_CONTEXT() FROM employees", connection);

                SqlTransaction tran = connection.BeginTransaction(IsolationLevel.ReadCommitted);
                command.Transaction = tran;

                using (SqlDataReader reader = command.ExecuteReader())
                {
                    while (reader.Read())
                    {
                        // Get the pointer for the file
                        string path = reader.GetString(0);
                        byte[] transactionContext = reader.GetSqlBytes(1).Buffer;

                        // Create the SqlFileStream
                        using (Stream fileStream = new SqlFileStream(path, transactionContext, FileAccess.Read, FileOptions.SequentialScan, allocationSize: 0))
                        {
                            // Read the contents as bytes and write them to the console
                            for (long index = 0; index < fileStream.Length; index++)
                            {
                                Console.WriteLine(fileStream.ReadByte());
                            }
                        }
                    }
                }
                tran.Commit();
            }
        }

        private static void OverwriteFileStream(SqlConnectionStringBuilder connStringBuilder)
        {
            using (SqlConnection connection = new SqlConnection(connStringBuilder.ToString()))
            {
                connection.Open();

                SqlCommand command = new SqlCommand("SELECT TOP(1) Photo.PathName(), GET_FILESTREAM_TRANSACTION_CONTEXT() FROM employees", connection);

                SqlTransaction tran = connection.BeginTransaction(IsolationLevel.ReadCommitted);
                command.Transaction = tran;

                using (SqlDataReader reader = command.ExecuteReader())
                {
                    while (reader.Read())
                    {
                        // Get the pointer for file
                        string path = reader.GetString(0);
                        byte[] transactionContext = reader.GetSqlBytes(1).Buffer;

                        // Create the SqlFileStream
                        using (Stream fileStream = new SqlFileStream(path, transactionContext, FileAccess.Write, FileOptions.SequentialScan, allocationSize: 0))
                        {
                            // Write a single byte to the file. This will
                            // replace any data in the file.
                            fileStream.WriteByte(0x01);
                        }
                    }
                }
                tran.Commit();
            }
        }

        private static void InsertFileStream(SqlConnectionStringBuilder connStringBuilder)
        {
            using (SqlConnection connection = new SqlConnection(connStringBuilder.ToString()))
            {
                connection.Open();

                SqlCommand command = new SqlCommand("SELECT TOP(1) Photo.PathName(), GET_FILESTREAM_TRANSACTION_CONTEXT() FROM employees", connection);

                SqlTransaction tran = connection.BeginTransaction(IsolationLevel.ReadCommitted);
                command.Transaction = tran;

                using (SqlDataReader reader = command.ExecuteReader())
                {
                    while (reader.Read())
                    {
                        // Get the pointer for file
                        string path = reader.GetString(0);
                        byte[] transactionContext = reader.GetSqlBytes(1).Buffer;

                        using (Stream fileStream = new SqlFileStream(path, transactionContext, FileAccess.ReadWrite, FileOptions.SequentialScan, allocationSize: 0))
                        {
                            // Seek to the end of the file
                            fileStream.Seek(0, SeekOrigin.End);

                            // Append a single byte
                            fileStream.WriteByte(0x01);
                        }
                    }
                }
                tran.Commit();
            }

        }
    }
}
```

对于另一个示例，请参阅[如何在文件流列中存储和提取二进制数据](https://www.codeproject.com/Articles/32216/How-to-store-and-fetch-binary-data-into-a-file-str)。

## <a name="resources-in-sql-server-books-online"></a>SQL Server 联机丛书中的资源

FILESTREAM 的完整文档位于 SQL Server 联机丛书的下列部分中。

|主题|描述|
|-----------|-----------------|
|[FILESTREAM （SQL Server）](/sql/relational-databases/blob/filestream-sql-server)|介绍何时使用 FILESTREAM 存储以及该存储如何将 SQL Server 数据库引擎与 NTFS 文件系统集成。|
|[为 FILESTREAM 数据创建客户端应用程序](/sql/relational-databases/blob/create-client-applications-for-filestream-data)|介绍用于处理 FILESTREAM 数据的 Windows API 函数。|
|[FILESTREAM 和其他 SQL Server 功能](/sql/relational-databases/blob/filestream-compatibility-with-other-sql-server-features)|提供将 FILESTREAM 数据与 SQL Server 的其他功能一起使用时的注意事项、准则和限制。|

## <a name="see-also"></a>请参阅

- [SQL Server 数据类型和 ADO.NET](sql-server-data-types.md)
- [在 ADO.NET 中检索和修改数据](../retrieving-and-modifying-data.md)
- [代码访问安全性和 ADO.NET](../code-access-security.md)
- [SQL Server 二进制和大值数据](sql-server-binary-and-large-value-data.md)
- [ADO.NET 概述](../ado-net-overview.md)
