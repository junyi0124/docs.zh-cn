---
title: Oracle LOB
ms.date: 03/30/2017
ms.assetid: 272e8e1e-a31f-475a-8c2a-ae8e1286bdab
ms.openlocfilehash: 072e3e3514c2dd32ddff0bac941da30788feae16
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91147835"
---
# <a name="oracle-lobs"></a>Oracle LOB

用于 Oracle 的 .NET Framework 数据提供程序包括 <xref:System.Data.OracleClient.OracleLob> 类，该类用于使用 Oracle **LOB** 数据类型。  
  
 **OracleLob**可以是以下 <xref:System.Data.OracleClient.OracleType> 数据类型之一：  
  
|数据类型|说明|  
|---------------|-----------------|  
|**Blob**|一种 Oracle **BLOB** 数据类型，它包含最大大小为 4 gb 的二进制数据。 这会映射到类型为**Byte**的**数组**。|  
|**Clob**|包含字符数据的 Oracle **CLOB** 数据类型，它基于服务器上的默认字符集，最大大小为 4 gb。 这将映射到 **字符串**。|  
|**NClob**|包含字符数据的 Oracle **NCLOB** 数据类型，其最大大小为 4 gb，基于服务器上的区域字符集。 这将映射到 **字符串**。|  
  
 **OracleLob**与的不同之处在于 <xref:System.Data.OracleClient.OracleBFile> ，数据存储在服务器上而不是存储在操作系统的物理文件中。 它也可以是一个读写对象，这一点不同于 **OracleBFile**，后者始终是只读的。  
  
## <a name="creating-retrieving-and-writing-to-a-lob"></a>创建、检索和写入 LOB  

 下面的 c # 示例演示如何在 Oracle 表中创建 Lob，然后以 **OracleLob** 对象的形式检索和写入它们。 该示例演示了如何使用 <xref:System.Data.OracleClient.OracleDataReader> 对象和 **OracleLob** **读取** 和 **写入** 方法。 该示例使用 Oracle **BLOB**、 **CLOB**和 **NCLOB** 数据类型。  
  
```csharp  
using System;  
using System.IO;
using System.Text;
using System.Data;
using System.Data.OracleClient;  
  
// LobExample  
public class LobExample  
{  
   public static int Main(string[] args)  
   {  
      //Create a connection.  
      OracleConnection conn = new OracleConnection(  
         "Data Source=Oracle8i;Integrated Security=yes");  
      using(conn)  
      {  
         //Open a connection.  
         conn.Open();  
         OracleCommand cmd = conn.CreateCommand();  
  
         //Create the table and schema.  
         CreateTable(cmd);  
  
         //Read example.  
         ReadLobExample(cmd);  
  
         //Write example  
         WriteLobExample(cmd);  
      }  
  
      return 1;  
   }  
  
   // ReadLobExample  
   public static void ReadLobExample(OracleCommand cmd)  
   {  
      int actual = 0;  
  
      // Table Schema:  
      // "CREATE TABLE tablewithlobs (a int, b BLOB, c CLOB, d NCLOB)";  
      // "INSERT INTO tablewithlobs values (1, 'AA', 'AAA', N'AAAA')";  
      // Select some data.  
      cmd.CommandText = "SELECT * FROM tablewithlobs";  
      OracleDataReader reader = cmd.ExecuteReader();  
      using(reader)  
      {  
         //Obtain the first row of data.  
         reader.Read();  
  
         //Obtain the LOBs (all 3 varieties).  
         OracleLob blob = reader.GetOracleLob(1);  
         OracleLob clob = reader.GetOracleLob(2);  
         OracleLob nclob = reader.GetOracleLob(3);  
  
         //Example - Reading binary data (in chunks).  
         byte[] buffer = new byte[100];  
         while((actual = blob.Read(buffer, 0, buffer.Length)) >0)  
            Console.WriteLine(blob.LobType + ".Read(" + buffer + ", " +
              buffer.Length + ") => " + actual);  
  
         // Example - Reading CLOB/NCLOB data (in chunks).  
         // Note: You can read character data as raw Unicode bytes
         // (using OracleLob.Read as in the above example).  
         // However, because the OracleLob object inherits directly
         // from the .NET stream object,
         // all the existing classes that manipluate streams can
         // also be used. For example, the
         // .NET StreamReader makes it easier to convert the raw bytes
         // into actual characters.  
         StreamReader streamreader =
           new StreamReader(clob, Encoding.Unicode);  
         char[] cbuffer = new char[100];  
         while((actual = streamreader.Read(cbuffer,
           0, cbuffer.Length)) >0)  
            Console.WriteLine(clob.LobType + ".Read(  
              " + new string(cbuffer, 0, actual) + ", " +
              cbuffer.Length + ") => " + actual);  
  
         // Example - Reading data (all at once).  
         // You could use StreamReader.ReadToEnd to obtain
         // all the string data, or simply  
         // call OracleLob.Value to obtain a contiguous allocation
         // of all the data.  
         Console.WriteLine(nclob.LobType + ".Value => " + nclob.Value);  
      }  
   }  
  
   // WriteLobExample  
   public static void WriteLobExample(OracleCommand cmd)  
   {  
      //Note: Updating LOB data requires a transaction.  
      cmd.Transaction = cmd.Connection.BeginTransaction();  
  
      // Select some data.  
      // Table Schema:  
      // "CREATE TABLE tablewithlobs (a int, b BLOB, c CLOB, d NCLOB)";  
      // "INSERT INTO tablewithlobs values (1, 'AA', 'AAA', N'AAAA')";  
      cmd.CommandText = "SELECT * FROM tablewithlobs FOR UPDATE";  
      OracleDataReader reader = cmd.ExecuteReader();  
      using(reader)  
      {  
         // Obtain the first row of data.  
         reader.Read();  
  
         // Obtain a LOB.  
         OracleLob blob = reader.GetOracleLob(1/*0:based ordinal*/);  
  
         // Perform any desired operations on the LOB
         // (read, position, and so on).  
  
         // Example - Writing binary data (directly to the backend).  
         // To write, you can use any of the stream classes, or write  
         // raw binary data using
         // the OracleLob write method. Writing character vs. binary
         // is the same;  
         // however note that character is always in terms of
         // Unicode byte counts  
         // (for example, even number of bytes - 2 bytes for every  
         // Unicode character).  
         byte[] buffer = new byte[100];  
         buffer[0] = 0xCC;  
         buffer[1] = 0xDD;  
         blob.Write(buffer, 0, 2);  
         blob.Position = 0;  
         Console.WriteLine(blob.LobType + ".Write(  
           " + buffer + ", 0, 2) => " + blob.Value);  
  
         // Example - Obtaining a temp LOB and copying data
         // into it from another LOB.  
         OracleLob templob = CreateTempLob(cmd, blob.LobType);  
         long actual = blob.CopyTo(templob);  
         Console.WriteLine(blob.LobType + ".CopyTo(  
            " + templob.Value + ") => " + actual);  
  
         // Commit the transaction now that everything succeeded.  
         // Note: On error, Transaction.Dispose is called
         // (from the using statement)  
         // and will automatically roll back the pending transaction.  
         cmd.Transaction.Commit();  
      }  
   }  
  
   // CreateTempLob  
   public static OracleLob CreateTempLob(  
     OracleCommand cmd, OracleType lobtype)  
   {  
      //Oracle server syntax to obtain a temporary LOB.  
      cmd.CommandText = "DECLARE A " + lobtype + "; "+  
                     "BEGIN "+  
                        "DBMS_LOB.CREATETEMPORARY(A, FALSE); "+  
                        ":LOC := A; "+  
                     "END;";  
  
      //Bind the LOB as an output parameter.  
      OracleParameter p = cmd.Parameters.Add("LOC", lobtype);  
      p.Direction = ParameterDirection.Output;  
  
      //Execute (to receive the output temporary LOB).  
      cmd.ExecuteNonQuery();  
  
      //Return the temporary LOB.  
      return (OracleLob)p.Value;  
   }  
  
   // CreateTable  
   public static void CreateTable(OracleCommand cmd)  
   {  
      // Table Schema:  
      // "CREATE TABLE tablewithlobs (a int, b BLOB, c CLOB, d NCLOB)";  
      // "INSERT INTO tablewithlobs VALUES (1, 'AA', 'AAA', N'AAAA')";  
      try  
      {  
         cmd.CommandText   = "DROP TABLE tablewithlobs";  
         cmd.ExecuteNonQuery();  
      }  
      catch(Exception)  
      {  
      }  
  
      cmd.CommandText =
        "CREATE TABLE tablewithlobs (a int, b BLOB, c CLOB, d NCLOB)";  
      cmd.ExecuteNonQuery();  
      cmd.CommandText =
        "INSERT INTO tablewithlobs VALUES (1, 'AA', 'AAA', N'AAAA')";  
      cmd.ExecuteNonQuery();  
   }  
}  
```  
  
## <a name="creating-a-temporary-lob"></a>创建临时 LOB  

 以下 C# 示例演示如何创建临时 LOB。  
  
```csharp  
OracleConnection conn = new OracleConnection(  
  "server=test8172; integrated security=yes;");  
conn.Open();  
  
OracleTransaction tx = conn.BeginTransaction();  
  
OracleCommand cmd = conn.CreateCommand();  
cmd.Transaction = tx;  
cmd.CommandText =
  "declare xx blob; begin dbms_lob.createtemporary(  
  xx, false, 0); :tempblob := xx; end;";  
cmd.Parameters.Add(new OracleParameter("tempblob",  
  OracleType.Blob)).Direction = ParameterDirection.Output;  
cmd.ExecuteNonQuery();  
OracleLob tempLob = (OracleLob)cmd.Parameters[0].Value;  
tempLob.BeginBatch(OracleLobOpenMode.ReadWrite);  
tempLob.Write(tempbuff,0,tempbuff.Length);  
tempLob.EndBatch();  
cmd.Parameters.Clear();  
cmd.CommandText = "myTable.myProc";  
cmd.CommandType = CommandType.StoredProcedure;
cmd.Parameters.Add(new OracleParameter(  
  "ImportDoc", OracleType.Blob)).Value = tempLob;  
cmd.ExecuteNonQuery();  
  
tx.Commit();  
```  
  
## <a name="see-also"></a>请参阅

- [Oracle 和 ADO.NET](oracle-and-adonet.md)
- [ADO.NET 概述](ado-net-overview.md)
