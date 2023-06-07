---
title: 疑难解答
ms.date: 03/30/2017
ms.assetid: 8cd4401c-b12c-4116-a421-f3dcffa65670
ms.openlocfilehash: 0ac71d9a55e92161f24deb490b8df6148bfc840c
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91202183"
---
# <a name="troubleshooting"></a>疑难解答

下面的信息揭示您在 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 应用程序中可能遇到的一些问题，并提供建议以避免这些问题或减少这些问题的影响。  
  
 其他问题在 [常见问题](frequently-asked-questions.md)中得到了解决。  
  
## <a name="unsupported-standard-query-operators"></a>不支持的标准查询运算符  

 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 不支持某些标准查询运算符方法（例如，<xref:System.Linq.Enumerable.ElementAt%2A>）。 因此，编译后的项目仍然可能产生运行时错误。 有关详细信息，请参阅 [标准查询运算符转换](standard-query-operator-translation.md)。  
  
## <a name="memory-issues"></a>内存问题  

 如果查询涉及内存中集合和 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] <xref:System.Data.Linq.Table%601> ，则该查询可能在内存中执行，具体取决于指定这两个集合的顺序。 如果该查询必须在内存中执行，则需要检索数据库表中的数据。  
  
 此方法的效率十分低下，并可能占用大量内存和处理器时间。 请尽量避免这种多域查询。  
  
## <a name="file-names-and-sqlmetal"></a>文件名和 SQLMetal  

 若要指定一个输入文件名，请将该名称作为输入文件添加到命令行。 不支持在连接字符串中包含文件名（使用 **/conn** 选项）。 有关详细信息，请参阅 [SqlMetal.exe（代码生成工具）](../../../../tools/sqlmetal-exe-code-generation-tool.md)。  
  
## <a name="class-library-projects"></a>类库项目  

 对象关系设计器在项目的文件中创建一个连接字符串 `app.config` 。 在类库项目中，不使用 `app.config` 文件。 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 使用在设计时文件中提供的连接字符串。 更改 `app.config` 中的值不会更改应用程序连接到的数据库。  
  
## <a name="cascade-delete"></a>级联删除  

 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 不支持且无法识别级联删除操作。 如果要在表中删除一个具有约束的行，必须执行以下操作之一：  
  
- 在数据库的外键约束中设置 `ON DELETE CASCADE` 规则。  
  
- 使用你自己的代码先删除阻止删除父对象的子对象。  
  
 否则，将引发 <xref:System.Data.SqlClient.SqlException> 异常。  
  
 有关详细信息，请参阅 [如何：从数据库中删除行](how-to-delete-rows-from-the-database.md)。  
  
## <a name="expression-not-queryable"></a>不可查询的表达式  

 如果收到错误消息“表达式 [表达式] 不可查询；是否缺少程序集引用?” ，请确保满足以下要求：  
  
- 应用程序面向 .NET Compact Framework 3.5。  
  
- 您具有对 `System.Core.dll` 和 `System.Data.Linq.dll` 的引用。  
  
- 你有 `Imports` (Visual Basic) 或 `using` (c # ) 指令用于 <xref:System.Linq> 和 <xref:System.Data.Linq> 。  
  
## <a name="duplicatekeyexception"></a>DuplicateKeyException  

 在调试 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 项目的过程中，可能会遍历某个实体的关系。 这样做会使这些项进入缓存，而 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 会检测到这些项的存在。 如果随后试图执行 <xref:System.Data.Linq.Table%601.Attach%2A> 或 <xref:System.Data.Linq.Table%601.InsertOnSubmit%2A>，或者执行一个生成具有相同键的多个行的类似方法，则会引发 <xref:System.Data.Linq.DuplicateKeyException>。  
  
## <a name="string-concatenation-exceptions"></a>字符串串联异常  

 不支持映射到 `[n]text` 和其他 `[n][var]char` 的操作数的串联。 映射到两个不同类型集的字符串的串联会引发异常。 有关详细信息，请参阅 [System.string 方法](system-string-methods.md)。  
  
## <a name="skip-and-take-exceptions-in-sql-server-2000"></a>SQL Server 2000 中的 Skip 和 Take 异常  

 在对 SQL Server 2000 数据库使用 <xref:System.Data.Linq.Mapping.ColumnAttribute.IsPrimaryKey%2A> 或 <xref:System.Linq.Queryable.Take%2A> 时，必须使用标识成员 (<xref:System.Linq.Queryable.Skip%2A>)。 查询必须针对单个表（即，不是联接），或必须为 <xref:System.Linq.Queryable.Distinct%2A>、<xref:System.Linq.Queryable.Except%2A>、<xref:System.Linq.Queryable.Intersect%2A> 或 <xref:System.Linq.Queryable.Union%2A> 操作，且不得包含 <xref:System.Linq.Queryable.Concat%2A> 操作。 有关详细信息，请参阅 [标准查询运算符转换](standard-query-operator-translation.md)中的 "SQL Server 2000 支持" 部分。  
  
 此要求不适用于 SQL Server 2005。  
  
## <a name="groupby-invalidoperationexception"></a>GroupBy InvalidOperationException  

 如果在按 <xref:System.Linq.Enumerable.GroupBy%2A> 表达式进行分组的 `boolean` 查询（如 `group x by (Phone==@phone)`）中有一个列值为 null，则会引发此异常。 由于表达式为，因此会将 `boolean` 该键推断为 `boolean` ，而不是 `nullable` `boolean` 。 当转换的比较产生 null 时，会尝试将分配给 `nullable` `boolean` `boolean` ，并引发异常。  
  
 若要避免发生这种情况（假定您希望将 null 视为 false），请使用如下方式：  
  
 `GroupBy="(Phone != null) && (Phone=@Phone)"`  
  
## <a name="oncreated-partial-method"></a>OnCreated() 分部方法  

 每次调用对象构造函数时都会调用生成的方法 `OnCreated()`，这包括以下这种情况，即 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 调用构造函数以生成原始值的副本。 如果您在自己的分部类中实现 `OnCreated()` 方法，请考虑此行为。  
  
## <a name="see-also"></a>请参阅

- [调试支持](debugging-support.md)
- [常见问题解答](frequently-asked-questions.md)
