---
title: REF CURSOR 示例
ms.date: 03/30/2017
ms.assetid: c257da03-c6c9-4cf8-b591-b7740a962c40
ms.openlocfilehash: b45ef971ccb6b785988cc351d02be9e0844f6e11
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91200532"
---
# <a name="ref-cursor-examples"></a>REF CURSOR 示例

REF CURSOR 示例包括下列三个 Visual Basic 示例，演示如何使用 REF CURSOR。  
  
|示例|说明|  
|------------|-----------------|  
|[OracleDataReader 中的 REF CURSOR 参数](ref-cursor-parameters-in-an-oracledatareader.md)|此示例执行一个 PL/SQL 存储过程，返回 REF CURSOR 参数，并将值作为 <xref:System.Data.OracleClient.OracleDataReader> 读取。|  
|[使用 OracleDataReader 从多个 REF CURSOR 中检索数据](retrieving-data-from-multiple-ref-cursors.md)|此示例执行一个 PL/SQL 存储过程，该存储过程返回两个 REF CURSOR 参数，并使用 **OracleDataReader**读取值。|  
|[使用一个或多个 REF CURSOR 填充数据集](filling-a-dataset-using-one-or-more-ref-cursors.md)|此示例执行一个 PL/SQL 存储过程，返回两个 REF CURSOR 参数，并使用返回的行填充 <xref:System.Data.DataSet>。|  
  
 要使用这些示例，可能需要创建 Oracle 表，并且必须创建 PL/SQL 包和包正文。  
  
## <a name="creating-the-oracle-tables"></a>创建 Oracle 表  

 这些示例使用 Oracle Scott/Tiger 架构中定义的表。 大多数 Oracle 安装均包括 Oracle Scott/Tiger 架构。 如果此架构不存在，可以使用 {OracleHome}\rdbms\admin\scott.sql 中的 SQL 命令文件创建供这些示例使用的表和索引。  
  
## <a name="creating-the-oracle-package-and-package-body"></a>创建 Oracle 包和包正文  

 这些示例要求服务器上存在以下 PL/SQL 包和包正文。 在 Oracle 服务器上创建以下 Oracle 包。  
  
```sql
CREATE OR REPLACE PACKAGE CURSPKG AS
    TYPE T_CURSOR IS REF CURSOR;
    PROCEDURE OPEN_ONE_CURSOR (N_EMPNO IN NUMBER,
                               IO_CURSOR IN OUT T_CURSOR);
    PROCEDURE OPEN_TWO_CURSORS (EMPCURSOR OUT T_CURSOR,
                                DEPTCURSOR OUT T_CURSOR);  
END CURSPKG;  
/
```  
  
 在 Oracle 服务器上创建下面的 Oracle 包正文。  
  
```sql
CREATE OR REPLACE PACKAGE BODY CURSPKG AS  
    PROCEDURE OPEN_ONE_CURSOR (N_EMPNO IN NUMBER,  
                               IO_CURSOR IN OUT T_CURSOR)  
    IS
        V_CURSOR T_CURSOR;
    BEGIN
        IF N_EMPNO <> 0
        THEN  
             OPEN V_CURSOR FOR
             SELECT EMP.EMPNO, EMP.ENAME, DEPT.DEPTNO, DEPT.DNAME
                  FROM EMP, DEPT
                  WHERE EMP.DEPTNO = DEPT.DEPTNO
                  AND EMP.EMPNO = N_EMPNO;  
  
        ELSE
             OPEN V_CURSOR FOR
             SELECT EMP.EMPNO, EMP.ENAME, DEPT.DEPTNO, DEPT.DNAME
                  FROM EMP, DEPT
                  WHERE EMP.DEPTNO = DEPT.DEPTNO;  
  
        END IF;  
        IO_CURSOR := V_CURSOR;
    END OPEN_ONE_CURSOR;
  
    PROCEDURE OPEN_TWO_CURSORS (EMPCURSOR OUT T_CURSOR,  
                                DEPTCURSOR OUT T_CURSOR)  
    IS
        V_CURSOR1 T_CURSOR;
        V_CURSOR2 T_CURSOR;
    BEGIN
        OPEN V_CURSOR1 FOR SELECT * FROM EMP;  
        OPEN V_CURSOR2 FOR SELECT * FROM DEPT;  
        EMPCURSOR  := V_CURSOR1;
        DEPTCURSOR := V_CURSOR2;
    END OPEN_TWO_CURSORS;
END CURSPKG;  
/  
```  
  
## <a name="see-also"></a>请参阅

- [Oracle REF CURSOR](oracle-ref-cursors.md)
- [ADO.NET 概述](ado-net-overview.md)
