---
title: Oracle BFILE
ms.date: 03/30/2017
ms.assetid: 341bbf84-4734-4d44-8723-ccedee954e21
ms.openlocfilehash: d43dfccd9735ce1ab822d7b14de2abaa0940c77b
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91166594"
---
# <a name="oracle-bfiles"></a>Oracle BFILE

Oracle .NET Framework 数据提供程序包括 <xref:System.Data.OracleClient.OracleBFile> 类，该类用于使用 Oracle <xref:System.Data.OracleClient.OracleType.BFile> 数据类型。  
  
 Oracle **BFILE** 数据类型是一种 oracle **LOB** 数据类型，包含对二进制数据的引用，其最大大小为 4 gb。 Oracle **BFILE** 不同于其他 oracle **LOB** 数据类型，因为它的数据存储在操作系统而不是服务器上的物理文件中。 请注意， **BFILE** 数据类型提供对数据的只读访问。  
  
 **BFILE**数据类型的其他特性可以将其与**LOB**数据类型区分开来：  
  
- 包含非结构化数据。  
  
- 支持服务器端分块。  
  
- 使用引用复制语义。 例如，如果您在 **bfile**上执行复制操作，则只会复制 (为对文件) 的引用的 **BFILE** 定位符。 而不会复制文件中的数据。  
  
 **BFILE**数据类型应用于引用大小较大的 lob，因此，不能在数据库中存储。 与**LOB**数据类型相比，使用**BFILE**数据类型时，会涉及更多的客户端、服务器和通信开销。 如果只需获取少量的数据，则访问 **BFILE** 会更有效。 如果需要获取整个对象，访问数据库驻留的 LOB 会更加有效。  
  
 每个非 NULL **OracleBFile** 对象都与定义基础物理文件位置的两个实体相关联：  
  
1. 一个 Oracle DIRECTORY 对象，它是文件系统中一个目录的数据库别名，以及  
  
2. 基础物理文件的文件名，它位于与 DIRECTORY 对象关联的目录中。  
  
## <a name="example"></a>示例  

 下面的 c # 示例演示如何在 Oracle 表中创建 **BFILE** ，然后以 **OracleBFile** 对象的形式检索它。 该示例演示如何使用 <xref:System.Data.OracleClient.OracleDataReader> 对象以及 **OracleBFile** **Seek** 和 **Read** 方法。 请注意，若要使用此示例，必须先在 Oracle 服务器上创建一个名为 "c： \\ \bfiles" 的目录和一个名为 "MyFile.jpg" 的文件。  
  
```csharp  
using System;  
using System.IO;  
using System.Data;  
using System.Data.OracleClient;  
  
public class Sample  
{  
   public static void Main(string[] args)  
   {  
      OracleConnection connection = new OracleConnection(  
        "Data Source=Oracle8i;Integrated Security=yes");  
      connection.Open();  
  
      OracleCommand command = connection.CreateCommand();  
      command.CommandText =
        "CREATE or REPLACE DIRECTORY MyDir as 'c:\\bfiles'";  
      command.ExecuteNonQuery();  
      command.CommandText =
        "DROP TABLE MyBFileTable";  
      try {  
        command.ExecuteNonQuery();  
      }  
      catch {  
      }  
      command.CommandText =
        "CREATE TABLE MyBFileTable(col1 number, col2 BFILE)";  
      command.ExecuteNonQuery();  
      command.CommandText =
        "INSERT INTO MyBFileTable values ('2', BFILENAME('MyDir', " +  
        "'MyFile.jpg'))";  
      command.ExecuteNonQuery();  
      command.CommandText = "SELECT * FROM MyBFileTable";  
  
        byte[] buffer = new byte[100];  
  
      OracleDataReader reader = command.ExecuteReader();  
      using (reader) {  
          if (reader.Read()) {  
                OracleBFile bFile = reader.GetOracleBFile(1);  
                using (bFile) {  
                  bFile.Seek(0, SeekOrigin.Begin);  
                  bFile.Read(buffer, 0, 100);  
              }  
          }  
      }  
  
      connection.Close();  
   }  
  
}  
```  
  
## <a name="see-also"></a>请参阅

- [Oracle 和 ADO.NET](oracle-and-adonet.md)
- [ADO.NET 概述](ado-net-overview.md)
