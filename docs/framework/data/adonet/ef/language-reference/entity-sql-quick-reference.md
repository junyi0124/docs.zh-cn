---
title: Entity SQL 快速参考
ms.date: 03/30/2017
ms.assetid: e53dad9e-5e83-426e-abb4-be3e78e3d6dc
ms.openlocfilehash: 7ec3b6fc184b4f169d6f6489bda0ec8fa4abb4f5
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91148135"
---
# <a name="entity-sql-quick-reference"></a>Entity SQL 快速参考

本主题提供 [!INCLUDE[esql](../../../../../../includes/esql-md.md)] 查询的快速参考。 本主题中的查询基于 AdventureWorks 销售模型。  
  
## <a name="literals"></a>文本  
  
### <a name="string"></a>字符串  

 字符串分为 Unicode 字符串和非 Unicode 字符串。 Unicode 字符串前面附有 N。例如， `N'hello'` 。  
  
 下面是非 Unicode 字符串的示例：  
  
```sql  
'hello'  
--same as  
"hello"  
```  
  
 输出：  
  
|值|  
|-----------|  
|Hello|  
  
### <a name="datetime"></a>DateTime  

 在日期时间文本中，日期部分和时间部分是必须存在的。 这里没有默认值。  
  
 示例：  
  
```sql  
DATETIME '2006-12-25 01:01:00.000'
--same as  
DATETIME '2006-12-25 01:01'  
```  
  
 输出：  
  
|值|  
|-----------|  
|12/25/2006 1:01:00 AM|  
  
### <a name="integer"></a>Integer  

 整数文本可以为 Int32 (123)、UInt32 (123U)、Int64 (123L) 和 UInt64 (123UL) 类型。  
  
 示例：  
  
```sql  
--a collection of integers  
{1, 2, 3}  
```  
  
 输出：  
  
|值|  
|-----------|  
|1|  
|2|  
|3|  
  
### <a name="other"></a>其他  

 [!INCLUDE[esql](../../../../../../includes/esql-md.md)] 支持的其他文字为、Guid、二进制、浮点/双精度型、十进制和 `null`。 [!INCLUDE[esql](../../../../../../includes/esql-md.md)] 中的 null 文本视为与概念模型中的每一种其他类型兼容。  
  
## <a name="type-constructors"></a>类型构造函数  
  
### <a name="row"></a>ROW  

 [ROW](row-entity-sql.md) 构造一个匿名的结构化类型 (记录) 值，如下所示： `ROW(1 AS myNumber, ‘Name’ AS myName).`  
  
 示例：  
  
```sql  
SELECT VALUE row (product.ProductID AS ProductID, product.Name
    AS ProductName) FROM AdventureWorksEntities.Product AS product
```  
  
 输出：  
  
|ProductID|名称|  
|---------------|----------|  
|1|Adjustable Race|  
|879|All-Purpose Bike Stand|  
|712|AWC Logo Cap|  
|...|...|  
  
### <a name="multiset"></a>MULTISET  

 [多重集](multiset-entity-sql.md) 构造集合，例如：  
  
 `MULTISET(1,2,2,3)` `--same as`-`{1,2,2,3}.`  
  
 示例：  
  
```sql  
SELECT VALUE product FROM AdventureWorksEntities.Product AS product WHERE product.ListPrice IN MultiSet (125, 300)  
```  
  
 输出：  
  
|ProductID|名称|ProductNumber|…|  
|---------------|----------|-------------------|-------|  
|842|Touring-Panniers, Large|PA-T100|…|  
  
### <a name="object"></a>对象  

 [命名类型构造函数](named-type-constructor-entity-sql.md) 构造 (名) 用户定义对象，例如 `person("abc", 12)` 。  
  
 示例：  
  
```sql  
SELECT VALUE AdventureWorksModel.SalesOrderDetail (o.SalesOrderDetailID, o.CarrierTrackingNumber, o.OrderQty,
o.ProductID, o.SpecialOfferID, o.UnitPrice, o.UnitPriceDiscount,
o.rowguid, o.ModifiedDate) FROM AdventureWorksEntities.SalesOrderDetail
AS o  
```  
  
 输出：  
  
|SalesOrderDetailID|CarrierTrackingNumber|OrderQty|ProductID|...|  
|------------------------|---------------------------|--------------|---------------|---------|  
|1|4911-403C-98|1|776|...|  
|2|4911-403C-98|3|777|...|  
|...|...|...|...|...|  
  
## <a name="references"></a>参考  
  
### <a name="ref"></a>REF  

 [REF](ref-entity-sql.md) 创建对实体类型实例的引用。 例如，下面的查询返回对 Orders 实体集中每一个 Order 实体的引用：  
  
```sql  
SELECT REF(o) AS OrderID FROM Orders AS o  
```  
  
 输出：  
  
|值|  
|-----------|  
|1|  
|2|  
|3|  
|...|  
  
 下面的示例使用属性提取运算符 (.) 访问实体的属性。 在使用属性提取运算符时，引用将自动被反引用。  
  
 示例：  
  
```sql  
SELECT VALUE REF(p).Name FROM
    AdventureWorksEntities.Product AS p
```  
  
 输出：  
  
|值|  
|-----------|  
|Adjustable Race|  
|All-Purpose Bike Stand|  
|AWC Logo Cap|  
|...|  
  
### <a name="deref"></a>DEREF  

 [DEREF](deref-entity-sql.md) 取消引用一个引用值，并生成该取消引用的结果。 例如，下面的查询生成 Orders 实体集中每一个 Order 的 Order 实体：`SELECT DEREF(o2.r) FROM (SELECT REF(o) AS r FROM LOB.Orders AS o) AS o2`。  
  
 示例：  
  
```sql  
SELECT VALUE DEREF(REF(p)).Name FROM
    AdventureWorksEntities.Product AS p
```  
  
 输出：  
  
|值|  
|-----------|  
|Adjustable Race|  
|All-Purpose Bike Stand|  
|AWC Logo Cap|  
|...|  
  
### <a name="createref-and-key"></a>CREATEREF 和 KEY  

 [CREATEREF](createref-entity-sql.md) 创建一个传递密钥的引用。 [KEY](key-entity-sql.md) 提取具有类型引用的表达式的键部分。  
  
 示例：  
  
```sql  
SELECT VALUE Key(CreateRef(AdventureWorksEntities.Product, row(p.ProductID)))
    FROM AdventureWorksEntities.Product AS p
```  
  
 输出：  
  
|ProductID|  
|---------------|  
|980|  
|365|  
|771|  
|...|  
  
## <a name="functions"></a>函数  
  
### <a name="canonical"></a>Canonical  

 [规范函数](canonical-functions.md)的命名空间为 Edm，如中所示 `Edm.Length("string")` 。 您无需指定命名空间，除非导入的另一个命名空间中包含与规范函数同名的函数。 如果两个命名空间有相同的函数，用户应指定完整名称。  
  
 示例：  
  
```sql  
SELECT Length(c. FirstName) AS NameLen FROM
    AdventureWorksEntities.Contact AS c
    WHERE c.ContactID BETWEEN 10 AND 12  
```  
  
 输出：  
  
|NameLen|  
|-------------|  
|6|  
|6|  
|5|  
  
### <a name="microsoft-provider-specific"></a>特定于 Microsoft 提供程序  

 命名空间中有[特定于 Microsoft 提供程序的函数](../sqlclient-for-ef-functions.md) `SqlServer` 。  
  
 示例：  
  
```sql  
SELECT SqlServer.LEN(c.EmailAddress) AS EmailLen FROM
    AdventureWorksEntities.Contact AS c WHERE
    c.ContactID BETWEEN 10 AND 12  
```  
  
 输出：  
  
|EmailLen|  
|--------------|  
|27|  
|27|  
|26|  
  
## <a name="namespaces"></a>命名空间  

 [USING](using-entity-sql.md) 指定查询表达式中使用的命名空间。  
  
 示例：  
  
```sql  
using SqlServer; LOWER('AA');  
```  
  
 输出：  
  
|值|  
|-----------|  
|aa|  
  
## <a name="paging"></a>分页  

 可以通过在[ORDER by](order-by-entity-sql.md)子句中声明[SKIP](skip-entity-sql.md)和[LIMIT](limit-entity-sql.md)子子句来表示分页。  
  
 示例：  
  
```sql  
SELECT c.ContactID as ID, c.LastName AS Name FROM
    AdventureWorks.Contact AS c ORDER BY c.ContactID SKIP 9 LIMIT 3;  
```  
  
 输出：  
  
|ID|名称|  
|--------|----------|  
|10|Adina|  
|11|Agcaoili|  
|12|Aguilar|  
  
## <a name="grouping"></a>分组  

 [分组依据](group-by-entity-sql.md) 指定由查询返回的对象 ([选择](select-entity-sql.md) 要放置) 表达式的组。  
  
 示例：  
  
```sql  
SELECT VALUE name FROM AdventureWorksEntities.Product AS P
    GROUP BY P.Name HAVING MAX(P.ListPrice) > 5  
```  
  
 输出：  
  
|name|  
|----------|  
|LL Mountain Seat Assembly|  
|ML Mountain Seat Assembly|  
|HL Mountain Seat Assembly|  
|...|  
  
## <a name="navigation"></a>导航  

 关系导航运算符用于导航从一个实体（起始端）到另一个实体（结束端）的关系。 [导航](navigate-entity-sql.md)采用 \<namespace> 限定为的关系类型。 \<relationship type name> \<T>如果结束端的基数为1，则导航返回 Ref。 如果结束端的基数为 n，将返回<引用> 的集合 \<T> 。  
  
 示例：  
  
```sql  
SELECT a.AddressID, (SELECT VALUE DEREF(v) FROM
    NAVIGATE(a, AdventureWorksModel.FK_SalesOrderHeader_Address_BillToAddressID) AS v)
    FROM AdventureWorksEntities.Address AS a  
```  
  
 输出：  
  
|AddressID|  
|---------------|  
|1|  
|2|  
|3|  
|...|  
  
## <a name="select-value-and-select"></a>SELECT VALUE 和 SELECT  
  
### <a name="select-value"></a>SELECT VALUE  

 [!INCLUDE[esql](../../../../../../includes/esql-md.md)] 提供了 SELECT VALUE 子句以跳过隐式行构造。 SELECT VALUE 子句中只能指定一项。 使用此类子句时，将不会围绕 SELECT 子句中的项构造行包装器，并且可生成所需形状的集合，例如： `SELECT VALUE a` 。  
  
 示例：  
  
```sql  
SELECT VALUE p.Name FROM AdventureWorksEntities.Product AS p
```  
  
 输出：  
  
|名称|  
|----------|  
|Adjustable Race|  
|All-Purpose Bike Stand|  
|AWC Logo Cap|  
|...|  
  
### <a name="select"></a>SELECT  

 [!INCLUDE[esql](../../../../../../includes/esql-md.md)] 还提供了用于构造任意行的行构造函数。 SELECT 接受投影中的一个或多个元素，并生成含有字段的数据记录，例如：`SELECT a, b, c`。  
  
 示例：  
  
 SELECT p.Name, p.ProductID FROM AdventureWorksEntities.Product as p Output:  
  
|名称|ProductID|  
|----------|---------------|  
|Adjustable Race|1|  
|All-Purpose Bike Stand|879|  
|AWC Logo Cap|712|  
|...|...|  
  
## <a name="case-expression"></a>CASE 表达式  

 [Case 表达式](case-entity-sql.md)计算一组布尔表达式的值以确定结果。  
  
 示例：  
  
```sql  
CASE WHEN AVG({25,12,11}) < 100 THEN TRUE ELSE FALSE END  
```  
  
 输出：  
  
|值|  
|-----------|  
|TRUE|  
  
## <a name="see-also"></a>请参阅

- [实体 SQL 引用](entity-sql-reference.md)
- [Entity SQL 概述](entity-sql-overview.md)
