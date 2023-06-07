---
title: 查询表达式
description: '了解 F # 编程语言中对 LINQ 的查询表达式支持。'
ms.date: 08/15/2020
ms.openlocfilehash: b2380bbc448aca06a40896582f3d4e7f701c6184
ms.sourcegitcommit: fe8877e564deb68d77fa4b79f55584ac8d7e8997
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/17/2020
ms.locfileid: "90720473"
---
# <a name="query-expressions"></a>查询表达式

利用查询表达式，您可以查询数据源并将数据置于所需的窗体中。 查询表达式为 F # 中的 LINQ 提供支持。

## <a name="syntax"></a>语法

```fsharp
query { expression }
```

## <a name="remarks"></a>备注

查询表达式是一种类似于序列表达式的计算表达式。 就像通过在序列表达式中提供代码来指定序列一样，通过在查询表达式中提供代码来指定一组数据。 在序列表达式中， `yield` 关键字标识作为结果序列的一部分返回的数据。 在查询表达式中， `select` 关键字执行相同的功能。 除了 `select` 关键字外，F # 还支持多个查询运算符，这一点非常类似于 SQL SELECT 语句的各个部分。 下面是一个简单查询表达式的示例，以及连接到 Northwind OData 源的代码。

```fsharp
// Use the OData type provider to create types that can be used to access the Northwind database.
// Add References to FSharp.Data.TypeProviders and System.Data.Services.Client
open Microsoft.FSharp.Data.TypeProviders

type Northwind = ODataService<"http://services.odata.org/Northwind/Northwind.svc">
let db = Northwind.GetDataContext()

// A query expression.
let query1 =
    query {
        for customer in db.Customers do
            select customer
    }

// Print results
query1
|> Seq.iter (fun customer -> printfn "Company: %s Contact: %s" customer.CompanyName customer.ContactName)
```

在上面的代码示例中，查询表达式位于大括号中。 表达式中的代码含义为，返回查询结果中数据库的 Customers 表中的每个客户。 查询表达式返回实现和的类型 <xref:System.Linq.IQueryable%601> ， <xref:System.Collections.Generic.IEnumerable%601> 因此可以使用 [Seq 模块](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-seqmodule.html) （如示例所示）来循环访问。

每个计算表达式类型都是从生成器类生成的。 查询计算表达式的生成器类为 `QueryBuilder` 。 有关详细信息，请参阅 [计算表达式](computation-expressions.md) 和 [QueryBuilder 类](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-linq-querybuilder.html)。

## <a name="query-operators"></a>查询运算符

利用查询运算符，您可以指定查询的详细信息，例如对要返回的记录添加条件或指定结果的排序顺序。 查询源必须支持查询运算符。 如果尝试使用不受支持的查询运算符， `System.NotSupportedException` 将引发。

查询表达式中只允许使用可以转换为 SQL 的表达式。 例如，使用查询运算符时，表达式中不允许使用函数调用 `where` 。

表1显示了可用的查询运算符。 此外，请参阅 Table2，这将比较 SQL 查询和本主题后面的等效 F # 查询表达式。 某些类型提供程序不支持某些查询运算符。 特别是，由于 OData 的限制，OData 类型提供程序在支持的查询运算符中受到限制。

此表假定数据库采用以下格式：

![显示示例数据库的关系图。](./media/query-expressions/student-course-database.png)

接下来的表中的代码还采用以下数据库连接代码。 项目应添加对 System.web、Fsharp.core 和 Fsharp.data.typeproviders 程序集的引用。 本主题末尾包含了用于创建此数据库的代码。

```fsharp
open System
open Microsoft.FSharp.Data.TypeProviders
open System.Data.Linq.SqlClient
open System.Linq
open Microsoft.FSharp.Linq

type schema = SqlDataConnection< @"Data Source=SERVER\INSTANCE;Initial Catalog=MyDatabase;Integrated Security=SSPI;" >

let db = schema.GetDataContext()

// Needed for some query operator examples:
let data = [ 1; 5; 7; 11; 18; 21]
```

### <a name="table-1-query-operators"></a>表 1. 查询运算符

<table style="width:100%">
  <tr>
    <th>运算符</th>
    <th>说明</th>
  </tr>
  <tr>
  <td><code>contains</code></td>
<td>确定所选元素是否包含指定的元素。<br/><br/>

<pre><code class="lang-fsharp">query {
    for student in db.Student do
    select student.Age.Value
    contains 11
}
</code></pre>

</td>
</tr>

<tr>
  <td><code>count</code></td><td>返回选定元素的数目。<br/><br/>

<pre><code class="lang-fsharp">query {
    for student in db.Student do
    select student
    count
}
</code></pre>

</td></tr>
<tr>
<td><code>last</code></td><td>选择迄今为止选定的最后一个元素。<br/><br/>

<pre><code class="lang-fsharp">query {
    for number in data do
    last
}
</code></pre>

</td></tr>
<tr>
<td><code>lastOrDefault</code></td><td>选择迄今为止选定的最后一个元素，如果未找到元素，则选择默认值。<br/><br/>

<pre><code class="lang-fsharp">query {
    for number in data do
    where (number &lt; 0)
    lastOrDefault
}
</code></pre>

</td></tr><tr>
<td><code>exactlyOne</code></td><td>选择到目前为止选择的单个特定元素。 如果存在多个元素，则会引发异常。<br/><br/>

<pre><code class="lang-fsharp">query {
    for student in db.Student do
    where (student.StudentID = 1)
    select student
    exactlyOne
}
</code></pre>

</td></tr><tr>
<td><code>exactlyOneOrDefault</code></td><td>选择迄今为止选定的单个特定元素，如果未找到该元素，则选择默认值。<br/><br/>

<pre><code class="lang-fsharp">query {
    for student in db.Student do
    where (student.StudentID = 1)
    select student
    exactlyOneOrDefault
}
</code></pre>

</td></tr><tr>
<td><code>headOrDefault</code></td><td>选择迄今为止选定的第一个元素，如果序列不包含任何元素，则为默认值。<br/><br/>

<pre><code class="lang-fsharp">query {
    for student in db.Student do
    select student
    headOrDefault
}
</code></pre>

</td></tr><tr>
<td><code>select</code></td><td>投影到目前为止选择的每个元素。<br/><br/>

<pre><code class="lang-fsharp">query {
    for student in db.Student do
    select student
}
</code></pre>

</td></tr><tr>
<td><code>where</code></td><td>根据指定的谓词选择元素。<br/><br/>

<pre><code class="lang-fsharp">query {
    for student in db.Student do
    where (student.StudentID > 4)
    select student
}
</code></pre>

</td></tr><tr>
<td><code>minBy</code></td><td>为迄今为止选择的每个元素选择一个值，并返回最小结果值。<br/><br/>

<pre><code class="lang-fsharp">query {
    for student in db.Student do
    minBy student.StudentID
}
</code></pre>

</td></tr><tr>
<td><code>maxBy</code></td><td>为迄今为止选择的每个元素选择一个值，并返回最大结果值。<br/><br/>

<pre><code class="lang-fsharp">query {
    for student in db.Student do
    maxBy student.StudentID
}
</code></pre>

</td></tr><tr>
<td><code>groupBy</code></td><td>根据指定的键选择器对迄今为止选定的元素进行分组。<br/><br/>

<pre><code class="lang-fsharp">query {
    for student in db.Student do
    groupBy student.Age into g
    select (g.Key, g.Count())
}
</code></pre>

</td></tr><tr>
<td><code>sortBy</code></td><td>按给定的排序键按升序对所选元素进行排序。<br/><br/>

<pre><code class="lang-fsharp">query {
    for student in db.Student do
    sortBy student.Name
    select student
}
</code></pre>

</td></tr><tr>
<td><code>sortByDescending</code></td><td>按给定的排序键按降序对所选元素进行排序。<br/><br/>

<pre><code class="lang-fsharp">query {
    for student in db.Student do
    sortByDescending student.Name
    select student
}
</code></pre>

</td></tr><tr>
<td><code>thenBy</code></td><td>按给定的排序键对迄今为止选定的元素执行后续排序。 此运算符仅可在、、或之后使用 <code>sortBy</code> <code>sortByDescending</code> <code>thenBy</code> <code>thenByDescending</code> 。<br/><br/>

<pre><code class="lang-fsharp">query {
    for student in db.Student do
    where student.Age.HasValue
    sortBy student.Age.Value
    thenBy student.Name
    select student
}
</code></pre>

</td></tr><tr>
<td><code>thenByDescending</code></td><td>按给定的排序键对迄今为止选定的元素执行后续排序。 此运算符仅可在、、或之后使用 <code>sortBy</code> <code>sortByDescending</code> <code>thenBy</code> <code>thenByDescending</code> 。<br/><br/>

<pre><code class="lang-fsharp">query {
    for student in db.Student do
    where student.Age.HasValue
    sortBy student.Age.Value
    thenByDescending student.Name
    select student
}
</code></pre>

</td></tr><tr>
<td><code>groupValBy</code></td><td>为迄今为止选择的每个元素选择一个值，并按给定键对元素进行分组。<br/><br/>

<pre><code class="lang-fsharp">query {
    for student in db.Student do
    groupValBy student.Name student.Age into g
    select (g, g.Key, g.Count())
}
</code></pre>

</td></tr><tr>
<td><code>join</code></td><td>基于匹配键将两组选定值关联起来。 请注意，联接表达式中 = sign 的键顺序很重要。 在所有联接中，如果行在符号后面拆分 <code>-&gt;</code> ，则缩进的缩进量必须至少为关键字 <code>for</code> 。<br/><br/>

<pre><code class="lang-fsharp">query {
    for student in db.Student do
    join selection in db.CourseSelection
        on (student.StudentID = selection.StudentID)
    select (student, selection)
}
</code></pre>

</td></tr><tr>
<td><code>groupJoin</code></td><td>基于匹配键关联两组选定值并对结果进行分组。 请注意，联接表达式中 = sign 的键顺序很重要。<br/><br/>

<pre><code class="lang-fsharp">query {
    for student in db.Student do
    groupJoin courseSelection in db.CourseSelection
        on (student.StudentID = courseSelection.StudentID) into g
    for courseSelection in g do
    join course in db.Course
        on (courseSelection.CourseID = course.CourseID)
    select (student.Name, course.CourseName)
}
</code></pre>

</td></tr><tr>
<td><code>leftOuterJoin</code></td><td>基于匹配键关联两组选定值并对结果进行分组。 如果任何组为空，则改用具有单个默认值的组。 请注意，联接表达式中 = sign 的键顺序很重要。<br/><br/>

<pre><code class="lang-fsharp">query {
    for student in db.Student do
    leftOuterJoin selection in db.CourseSelection
        on (student.StudentID = selection.StudentID) into result
    for selection in result.DefaultIfEmpty() do
    select (student, selection)
}
</code></pre>

</td></tr><tr>
<td><code>sumByNullable</code></td><td>为迄今为止选择的每个元素选择一个可以为 null 的值，并返回这些值的总和。 如果有可以为 null 的值，则它将被忽略。<br/><br/>

<pre><code class="lang-fsharp">query {
    for student in db.Student do
    sumByNullable student.Age
}
</code></pre>

</td></tr><tr>
<td><code>minByNullable</code></td><td>为迄今为止选择的每个元素选择一个可以为 null 的值，并返回这些值的最小值。 如果有可以为 null 的值，则它将被忽略。<br/><br/>

<pre><code class="lang-fsharp">query {
    for student in db.Student do
    minByNullable student.Age
}
</code></pre>

</td></tr><tr>
<td><code>maxByNullable</code></td><td>为迄今为止选择的每个元素选择一个可以为 null 的值，并返回这些值的最大值。 如果有可以为 null 的值，则它将被忽略。<br/><br/>

<pre><code class="lang-fsharp">query {
    for student in db.Student do
    maxByNullable student.Age
}
</code></pre>

</td></tr><tr>
<td><code>averageByNullable</code></td><td>为迄今为止选择的每个元素选择一个可以为 null 的值，并返回这些值的平均值。 如果有可以为 null 的值，则它将被忽略。<br/><br/>

<pre><code class="lang-fsharp">query {
    for student in db.Student do
    averageByNullable (Nullable.float student.Age)
}
</code></pre>

</td></tr><tr>
<td><code>averageBy</code></td><td>为到目前为止选择的每个元素选择一个值，并返回这些值的平均值。<br/><br/>

<pre><code class="lang-fsharp">query {
    for student in db.Student do
    averageBy (float student.StudentID)
}
</code></pre>

</td></tr><tr>
<td><code>distinct</code></td><td>选择到目前为止选择的元素中的非重复元素。<br/><br/>

<pre><code class="lang-fsharp">query {
    for student in db.Student do
    join selection in db.CourseSelection
        on (student.StudentID = selection.StudentID)
    distinct
}
</code></pre>

</td></tr><tr>
<td><code>exists</code></td><td>确定目前选择的任何元素是否都满足条件。<br/><br/>

<pre><code class="lang-fsharp">query {
    for student in db.Student do
    where
        (query {
            for courseSelection in db.CourseSelection do
            exists (courseSelection.StudentID = student.StudentID) })
    select student
}
</code></pre>

</td></tr><tr>
<td><code>find</code></td><td>选择迄今为止满足指定条件的第一个元素。<br/><br/>

<pre><code class="lang-fsharp">query {
    for student in db.Student do
    find (student.Name = "Abercrombie, Kim")
}
</code></pre>

</td></tr><tr>
<td><code>all</code></td><td>确定目前选定的所有元素是否都满足条件。<br/><br/>

<pre><code class="lang-fsharp">query {
    for student in db.Student do
    all (SqlMethods.Like(student.Name, "%,%"))
}
</code></pre>

</td></tr><tr>
<td><code>head</code></td><td>选择迄今为止选定的第一个元素。<br/><br/>

<pre><code class="lang-fsharp">query {
    for student in db.Student do
    head
}
</code></pre>

</td></tr><tr>
<td><code>nth</code></td><td>选择指定索引处的元素，该元素位于目前为止所选的位置。<br/><br/>

<pre><code class="lang-fsharp">query {
    for numbers in data do
    nth 3
}
</code></pre>

</td></tr><tr>
<td><code>skip</code></td><td>在迄今为止，跳过指定数目的元素，然后选择剩余的元素。<br/><br/>

<pre><code class="lang-fsharp">query {
    for student in db.Student do
    skip 1
}
</code></pre>

</td></tr><tr>
<td><code>skipWhile</code></td><td>只要指定的条件为 true，就跳过序列中的元素，然后选择剩余的元素。<br/><br/>

<pre><code class="lang-fsharp">query {
    for number in data do
    skipWhile (number &lt; 3)
    select student
}
</code></pre>

</td></tr><tr>
<td><code>sumBy</code></td><td>为到目前为止选择的每个元素选择一个值，并返回这些值的总和。<br/><br/>

<pre><code class="lang-fsharp">query {
    for student in db.Student do
    sumBy student.StudentID
}
</code></pre>

</td></tr><tr>
<td><code>take</code></td><td>从所选的到目前为止选择指定数量的连续元素。<br/><br/>

<pre><code class="lang-fsharp">query {
    for student in db.Student do
    select student
    take 2
}
</code></pre>

</td></tr><tr>
<td><code>takeWhile</code></td><td>只要指定的条件为 true，则从序列中选择元素，然后跳过剩余的元素。<br/><br/>

<pre><code class="lang-fsharp">query {
    for number in data do
    takeWhile (number &lt; 10)
}
</code></pre>

</td></tr><tr>
<td><code>sortByNullable</code></td><td>按给定的可以为 null 的排序键按升序对所选元素进行排序。<br/><br/>

<pre><code class="lang-fsharp">query {
    for student in db.Student do
    sortByNullable student.Age
    select student
}
</code></pre>

</td></tr><tr>
<td><code>sortByNullableDescending</code></td><td>按给定的可以为 null 的排序键对迄今为止选定的元素进行排序。<br/><br/>

<pre><code class="lang-fsharp">query {
    for student in db.Student do
    sortByNullableDescending student.Age
    select student
}
</code></pre>

</td></tr><tr>
<td><code>thenByNullable</code></td><td>按照给定的可以为 null 的排序键，对迄今为止选定的元素执行后续排序。 此运算符仅可在 <code>sortBy</code> 、、 <code>sortByDescending</code> <code>thenBy</code> 、或后立即使用 <code>thenByDescending</code> 。<br/><br/>

<pre><code class="lang-fsharp">query {
    for student in db.Student do
    sortBy student.Name
    thenByNullable student.Age
    select student
}
</code></pre>

</td></tr><tr>
<td><code>thenByNullableDescending</code></td><td>按给定的可以为 null 的排序键对迄今为止选定的元素执行后续排序。 此运算符仅可在 <code>sortBy</code> 、、 <code>sortByDescending</code> <code>thenBy</code> 、或后立即使用 <code>thenByDescending</code> 。<br/><br/>

<pre><code class="lang-fsharp">query {
    for student in db.Student do
    sortBy student.Name
    thenByNullableDescending student.Age
    select student
}
</code></pre>

</td></tr>
</table>

## <a name="comparison-of-transact-sql-and-f-query-expressions"></a>Transact-SQL 和 F# 查询表达式的比较

下表列出了一些常见的 Transact-sql 查询及其在 F # 中的等效项。 此表中的代码还假定与上表相同的数据库和用于设置类型提供程序的相同初始代码。

### <a name="table-2-transact-sql-and-f-query-expressions"></a>表 2. Transact-SQL 和 F# 查询表达式

<table style="width:100%">
  <tr>
    <th>Transact-sql (不区分大小写) </th>
    <th>F # 查询表达式 (区分大小写) </th>
  </tr>
<tr><td>
从表中选择所有字段。<br>

<pre><code class="lang-sql">SELECT * FROM Student
</code></pre>

</td><td>
<pre><code class="lang-fsharp">// All students.
query {
    for student in db.Student do
    select student
}
</code></pre>

</td></tr>
<tr><td>
计算表中的记录数。<br/>

<pre><code class="lang-sql">SELECT COUNT( * ) FROM Student
</code></pre>

</td><td>

<pre><code class="lang-fsharp">// Count of students.
query {
    for student in db.Student do
    count
}
</code></pre>

</td></tr><tr>
<td><code>EXISTS</code>
<br />

<!-- markdownlint-capture -->
<!-- markdownlint-disable no-space-in-emphasis -->
<pre><code class="lang-sql">SELECT * FROM Student
WHERE EXISTS
  (SELECT * FROM CourseSelection
   WHERE CourseSelection.StudentID = Student.StudentID)
</code></pre>
<!-- markdownlint-restore -->
</td>

<td>

<pre><code class="lang-fsharp">// Find students who have signed up at least one course.
query {
    for student in db.Student do
    where
        (query {
            for courseSelection in db.CourseSelection do
            exists (courseSelection.StudentID = student.StudentID) })
    select student
}
</code></pre>

</td></tr><tr>
<td>分组<br/>

<pre><code class="lang-sql">SELECT Student.Age, COUNT( * ) FROM Student
GROUP BY Student.Age
</code></pre>

</td><td>

<pre><code class="lang-fsharp">// Group by age and count.
query {
    for n in db.Student do
    groupBy n.Age into g
    select (g.Key, g.Count())
}
// OR
query {
    for n in db.Student do
    groupValBy n.Age n.Age into g
    select (g.Key, g.Count())
}
</code></pre>
</td></tr><tr><td>
具有条件的分组。<br/>

<pre><code class="lang-sql">SELECT Student.Age, COUNT( * )
FROM Student
GROUP BY Student.Age
HAVING student.Age > 10
</code></pre>

</td><td>

<pre><code class="lang-fsharp">// Group students by age where age > 10.
query {
    for student in db.Student do
    groupBy student.Age into g
    where (g.Key.HasValue && g.Key.Value > 10)
    select (g.Key, g.Count())
}
</code></pre>

</td></tr><tr><td>
包含计数条件的分组。<br/>

<!-- markdownlint-capture -->
<!-- markdownlint-disable no-space-in-emphasis -->
<pre><code class="lang-sql">SELECT Student.Age, COUNT( * )
FROM Student
GROUP BY Student.Age
HAVING COUNT( * ) > 1
</code></pre>
<!-- markdownlint-restore -->

</td><td>

<pre><code class="lang-fsharp">// Group students by age and count number of students
// at each age with more than 1 student.
query {
    for student in db.Student do
    groupBy student.Age into group
    where (group.Count() > 1)
    select (group.Key, group.Count())
}
</code></pre>

</td></tr><tr><td>
分组、计数和求和。<br/>

<pre><code class="lang-sql">SELECT Student.Age, COUNT( * ), SUM(Student.Age) as total
FROM Student
GROUP BY Student.Age
</code></pre>

</td><td>

<pre><code class="lang-fsharp">// Group students by age and sum ages.
query {
    for student in db.Student do
    groupBy student.Age into g
    let total =
        query {
            for student in g do
            sumByNullable student.Age
        }
    select (g.Key, g.Count(), total)
}
</code></pre>

</td></tr><tr><td>
按计数进行分组、计数和排序。<br/>

<!-- markdownlint-capture -->
<!-- markdownlint-disable no-space-in-emphasis -->
<pre><code class="lang-sql">SELECT Student.Age, COUNT( * ) as myCount
FROM Student
GROUP BY Student.Age
HAVING COUNT( * ) > 1
ORDER BY COUNT( * ) DESC
</code></pre>
<!-- markdownlint-restore -->

</td><td>

<pre><code class="lang-fsharp">// Group students by age, count number of students
// at each age, and display all with count > 1
// in descending order of count.
query {
    for student in db.Student do
    groupBy student.Age into g
    where (g.Count() > 1)
    sortByDescending (g.Count())
    select (g.Key, g.Count())
}
</code></pre>

</td></tr><tr><td>
<code>IN</code> 一组指定值<br/>

<pre><code class="lang-sql">SELECT *
FROM Student
WHERE Student.StudentID IN (1, 2, 5, 10)
</code></pre>

</td><td>

<pre><code class="lang-fsharp">// Select students where studentID is one of a given list.
let idQuery =
    query {
        for id in [1; 2; 5; 10] do
        select id
    }
query {
    for student in db.Student do
    where (idQuery.Contains(student.StudentID))
    select student
}
</code></pre>

</td></tr><tr><td>
<code>LIKE</code> 和 <code>TOP</code>。<br/>

<pre><code class="lang-sql">-- '_e%' matches strings where the second character is 'e'
SELECT TOP 2 * FROM Student
WHERE Student.Name LIKE '_e%'
</code></pre>

</td><td>
<pre><code class="lang-fsharp">// Look for students with Name match _e% pattern and take first two.
query {
    for student in db.Student do
    where (SqlMethods.Like( student.Name, "_e%") )
    select student
    take 2
}
</code></pre>

</td></tr><tr><td>
<code>LIKE</code> 具有模式匹配集。<br/>

<pre><code class="lang-sql">-- '[abc]%' matches strings where the first character is
-- 'a', 'b', 'c', 'A', 'B', or 'C'
SELECT * FROM Student
WHERE Student.Name LIKE '[abc]%'
</code></pre>
</td><td>

<pre><code class="lang-fsharp">query {
    for student in db.Student do
    where (SqlMethods.Like( student.Name, "[abc]%") )
    select student
}
</code></pre>

</td></tr><tr><td>
<code>LIKE</code> 带有 set 排除模式的。<br/>

<pre><code class="lang-sql">-- '[^abc]%' matches strings where the first character is
-- not 'a', 'b', 'c', 'A', 'B', or 'C'
SELECT * FROM Student
WHERE Student.Name LIKE '[^abc]%'
</code></pre>

</td><td>

<pre><code class="lang-fsharp">// Look for students with name matching [^abc]%% pattern.
query {
    for student in db.Student do
    where (SqlMethods.Like( student.Name, "[^abc]%") )
    select student
}
</code></pre>

</td></tr><tr><td>
<code>LIKE</code> ，但要选择不同的字段。<br/>

<pre><code class="lang-sql">SELECT StudentID AS ID FROM Student
WHERE Student.Name LIKE '[^abc]%'
</code></pre>

</td><td>

<pre><code class="lang-fsharp">query {
    for n in db.Student do
    where (SqlMethods.Like( n.Name, "[^abc]%") )
    select n.StudentID
}
</code></pre>

</td></tr><tr><td><code>LIKE</code>，其中包含子字符串搜索。<br/>

<pre><code class="lang-sql">SELECT * FROM Student
WHERE Student.Name like '%A%'
</code></pre>

</td><td>

<pre><code class="lang-fsharp">// Using Contains as a query filter.
query {
    for student in db.Student do
    where (student.Name.Contains("a"))
    select student
}
</code></pre>

</td></tr><tr><td>
<code>JOIN</code>具有两个表的简单。<br/>

<pre><code class="lang-sql">SELECT * FROM Student
JOIN CourseSelection
ON Student.StudentID = CourseSelection.StudentID
</code></pre>

</td><td>

<pre><code class="lang-fsharp">// Join Student and CourseSelection tables.
query {
    for student in db.Student do
    join selection in db.CourseSelection
        on (student.StudentID = selection.StudentID)
    select (student, selection)
}
</code></pre>

</td></tr><tr><td><code>LEFT JOIN</code> 具有两个表。<br/>

<pre><code class="lang-sql">SELECT * FROM Student
LEFT JOIN CourseSelection
ON Student.StudentID = CourseSelection.StudentID
</code></pre>

</td><td>

<pre><code class="lang-fsharp">//Left Join Student and CourseSelection tables.
query {
    for student in db.Student do
    leftOuterJoin selection in db.CourseSelection
        on (student.StudentID = selection.StudentID) into result
    for selection in result.DefaultIfEmpty() do
    select (student, selection)
}
</code></pre>

</td></tr><tr><td>带 <code>COUNT</code> 的 <code>JOIN</code><br/>

<pre><code class="lang-sql">SELECT COUNT( * ) FROM Student
JOIN CourseSelection
ON Student.StudentID = CourseSelection.StudentID
</code></pre>

</td><td>

<pre><code class="lang-fsharp">// Join with count.
query {
    for n in db.Student do
    join e in db.CourseSelection
        on (n.StudentID = e.StudentID)
    count
}
</code></pre>

</td></tr><tr><td><code>DISTINCT</code><br/>

<pre><code class="lang-sql">SELECT DISTINCT StudentID FROM CourseSelection
</code></pre>

</td><td>

<pre><code class="lang-fsharp">// Join with distinct.
query {
    for student in db.Student do
    join selection in db.CourseSelection
        on (student.StudentID = selection.StudentID)
    distinct
}
</code></pre>

</td></tr><tr><td>非重复计数。<br/>

<pre><code class="lang-sql">SELECT DISTINCT COUNT(StudentID) FROM CourseSelection
</code></pre>

</td><td>

<pre><code class="lang-fsharp">// Join with distinct and count.
query {
    for n in db.Student do
    join e in db.CourseSelection
        on (n.StudentID = e.StudentID)
    distinct
    count
}
</code></pre>

</td></tr><tr><td><code>BETWEEN</code><br/>

<pre><code class="lang-sql">SELECT * FROM Student
WHERE Student.Age BETWEEN 10 AND 15
</code></pre>

</td><td>

<pre><code class="lang-fsharp">// Selecting students with ages between 10 and 15.
query {
    for student in db.Student do
    where (student.Age ?>= 10 && student.Age ?&lt; 15)
    select student
}
</code></pre>

</td></tr><tr><td><code>OR</code><br/>

<pre><code class="lang-sql">SELECT * FROM Student
WHERE Student.Age = 11 OR Student.Age = 12
</code></pre>

</td><td>

<pre><code class="lang-fsharp">// Selecting students with age that's either 11 or 12.
query {
    for student in db.Student do
    where (student.Age.Value = 11 &#124;&#124; student.Age.Value = 12)
    select student
}
</code></pre>

</td></tr><tr><td><code>OR</code> 排序<br/>

<pre><code class="lang-sql">SELECT * FROM Student
WHERE Student.Age = 12 OR Student.Age = 13
ORDER BY Student.Age DESC
</code></pre>

</td><td>

<pre><code class="lang-fsharp">// Selecting students in a certain age range and sorting.
query {
    for n in db.Student do
    where (n.Age.Value = 12 &#124;&#124; n.Age.Value = 13)
    sortByNullableDescending n.Age
    select n
}
</code></pre>

</td></tr><tr><td><code>TOP</code>、 <code>OR</code> 和排序。<br/>

<pre><code class="lang-sql">SELECT TOP 2 student.Name FROM Student
WHERE Student.Age = 11 OR Student.Age = 12
ORDER BY Student.Name DESC
</code></pre>

</td><td>

<pre><code class="lang-fsharp">// Selecting students with certain ages,
// taking account of the possibility of nulls.
query {
    for student in db.Student do
    where
        ((student.Age.HasValue && student.Age.Value = 11) &#124;&#124;
         (student.Age.HasValue && student.Age.Value = 12))
    sortByDescending student.Name
    select student.Name
    take 2
}
</code></pre>

</td></tr><tr><td><code>UNION</code> 的两个查询。<br/>

<!-- markdownlint-capture -->
<!-- markdownlint-disable no-space-in-emphasis -->
<pre><code class="lang-sql">SELECT * FROM Student
UNION
SELECT * FROM lastStudent
</code></pre>
<!-- markdownlint-restore -->

</td><td>

<pre><code class="lang-fsharp">
let query1 =
    query {
        for n in db.Student do
        select (n.Name, n.Age)
    }

let query2 =
    query {
        for n in db.LastStudent do
        select (n.Name, n.Age)
    }

query2.Union (query1)
</code></pre>

</td></tr><tr><td>两个查询的交集。<br/>

<!-- markdownlint-capture -->
<!-- markdownlint-disable no-space-in-emphasis -->
<pre><code class="lang-sql">SELECT * FROM Student
INTERSECT
SELECT * FROM LastStudent
</code></pre>
<!-- markdownlint-restore -->
</td><td>

<pre><code class="lang-fsharp">
let query1 =
    query {
        for n in db.Student do
        select (n.Name, n.Age)
    }

let query2 =
    query {
        for n in db.LastStudent do
        select (n.Name, n.Age)
    }

query1.Intersect(query2)
</code></pre>

</td></tr><tr><td><code>CASE</code> 状态.<br/>

<pre><code class="lang-sql">SELECT student.StudentID,
CASE Student.Age
  WHEN -1 THEN 100
  ELSE Student.Age
END,
Student.Age
FROM Student
</code></pre>

</td><td>
<pre><code class="lang-fsharp">// Using if statement to alter results for special value.
query {
    for student in db.Student do
    select
        (if student.Age.HasValue && student.Age.Value = -1 then
             (student.StudentID, System.Nullable&lt;int&gt;(100), student.Age)
         else (student.StudentID, student.Age, student.Age))
}
</code></pre>

</td></tr><tr><td>多种情况。<br/>

<pre><code class="lang-sql">SELECT Student.StudentID,
CASE Student.Age
  WHEN -1 THEN 100
  WHEN 0 THEN 1000
  ELSE Student.Age
END,
Student.Age
FROM Student
</code></pre>

</td><td>

<pre><code class="lang-fsharp">// Using if statement to alter results for special values.
query {
    for student in db.Student do
    select
        (if student.Age.HasValue && student.Age.Value = -1 then
             (student.StudentID, System.Nullable&lt;int&gt;(100), student.Age)
         elif student.Age.HasValue && student.Age.Value = 0 then
             (student.StudentID, System.Nullable&lt;int&gt;(1000), student.Age)
         else (student.StudentID, student.Age, student.Age))
}
</code></pre>

</td></tr><tr><td>多个表。<br/>

<pre><code class="lang-sql">SELECT * FROM Student, Course
</code></pre>

</td><td>

<pre><code class="lang-fsharp">// Multiple table select.
query {
    for student in db.Student do
    for course in db.Course do
    select (student, course)
}
</code></pre>

</td></tr><tr><td>多个联接。<br/>

<pre><code class="lang-sql">SELECT Student.Name, Course.CourseName
FROM Student
JOIN CourseSelection
ON CourseSelection.StudentID = Student.StudentID
JOIN Course
ON Course.CourseID = CourseSelection.CourseID
</code></pre>

</td><td>

<pre><code class="lang-fsharp">// Multiple joins.
query {
    for student in db.Student do
    join courseSelection in db.CourseSelection
        on (student.StudentID = courseSelection.StudentID)
    join course in db.Course
        on (courseSelection.CourseID = course.CourseID)
    select (student.Name, course.CourseName)
}
</code></pre>

</td></tr><tr><td>多个左外部联接。<br/>

<pre><code class="lang-sql">SELECT Student.Name, Course.CourseName
FROM Student
LEFT OUTER JOIN CourseSelection
ON CourseSelection.StudentID = Student.StudentID
LEFT OUTER JOIN Course
ON Course.CourseID = CourseSelection.CourseID
</code></pre>

</td><td>

<pre><code class="lang-fsharp">// Using leftOuterJoin with multiple joins.
query {
    for student in db.Student do
    leftOuterJoin courseSelection in db.CourseSelection
        on (student.StudentID = courseSelection.StudentID) into g1
    for courseSelection in g1.DefaultIfEmpty() do
    leftOuterJoin course in db.Course
        on (courseSelection.CourseID = course.CourseID) into g2
    for course in g2.DefaultIfEmpty() do
    select (student.Name, course.CourseName)
}
</code></pre>

</td></tr></table>

下面的代码可用于为这些示例创建示例数据库。

<pre><code class="lang-sql">SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO

USE [master];
GO

IF EXISTS (SELECT * FROM sys.databases WHERE name = 'MyDatabase')
DROP DATABASE MyDatabase;
GO

-- Create the MyDatabase database.
CREATE DATABASE MyDatabase COLLATE SQL_Latin1_General_CP1_CI_AS;
GO

-- Specify a simple recovery model
-- to keep the log growth to a minimum.
ALTER DATABASE MyDatabase
SET RECOVERY SIMPLE;
GO

USE MyDatabase;
GO

CREATE TABLE [dbo].[Course] (
[CourseID]   INT           NOT NULL,
[CourseName] NVARCHAR (50) NOT NULL,
PRIMARY KEY CLUSTERED ([CourseID] ASC)
);

CREATE TABLE [dbo].[Student] (
[StudentID] INT           NOT NULL,
[Name]      NVARCHAR (50) NOT NULL,
[Age]       INT           NULL,
PRIMARY KEY CLUSTERED ([StudentID] ASC)
);

CREATE TABLE [dbo].[CourseSelection] (
[ID]        INT NOT NULL,
[StudentID] INT NOT NULL,
[CourseID]  INT NOT NULL,
PRIMARY KEY CLUSTERED ([ID] ASC),
CONSTRAINT [FK_CourseSelection_ToTable] FOREIGN KEY ([StudentID]) REFERENCES [dbo].[Student] ([StudentID]) ON DELETE NO ACTION ON UPDATE NO ACTION,
CONSTRAINT [FK_CourseSelection_Course_1] FOREIGN KEY ([CourseID]) REFERENCES [dbo].[Course] ([CourseID]) ON DELETE NO ACTION ON UPDATE NO ACTION
);

CREATE TABLE [dbo].[LastStudent] (
[StudentID] INT           NOT NULL,
[Name]      NVARCHAR (50) NOT NULL,
[Age]       INT           NULL,
PRIMARY KEY CLUSTERED ([StudentID] ASC)
);

-- Insert data into the tables.
USE MyDatabase
INSERT INTO Course (CourseID, CourseName)
VALUES(1, 'Algebra I');
INSERT INTO Course (CourseID, CourseName)
VALUES(2, 'Trigonometry');
INSERT INTO Course (CourseID, CourseName)
VALUES(3, 'Algebra II');
INSERT INTO Course (CourseID, CourseName)
VALUES(4, 'History');
INSERT INTO Course (CourseID, CourseName)
VALUES(5, 'English');
INSERT INTO Course (CourseID, CourseName)
VALUES(6, 'French');
INSERT INTO Course (CourseID, CourseName)
VALUES(7, 'Chinese');

INSERT INTO Student (StudentID, Name, Age)
VALUES(1, 'Abercrombie, Kim', 10);
INSERT INTO Student (StudentID, Name, Age)
VALUES(2, 'Abolrous, Hazen', 14);
INSERT INTO Student (StudentID, Name, Age)
VALUES(3, 'Hance, Jim', 12);
INSERT INTO Student (StudentID, Name, Age)
VALUES(4, 'Adams, Terry', 12);
INSERT INTO Student (StudentID, Name, Age)
VALUES(5, 'Hansen, Claus', 11);
INSERT INTO Student (StudentID, Name, Age)
VALUES(6, 'Penor, Lori', 13);
INSERT INTO Student (StudentID, Name, Age)
VALUES(7, 'Perham, Tom', 12);
INSERT INTO Student (StudentID, Name, Age)
VALUES(8, 'Peng, Yun-Feng', NULL);

INSERT INTO CourseSelection (ID, StudentID, CourseID)
VALUES(1, 1, 2);
INSERT INTO CourseSelection (ID, StudentID, CourseID)
VALUES(2, 1, 3);
INSERT INTO CourseSelection (ID, StudentID, CourseID)
VALUES(3, 1, 5);
INSERT INTO CourseSelection (ID, StudentID, CourseID)
VALUES(4, 2, 2);
INSERT INTO CourseSelection (ID, StudentID, CourseID)
VALUES(5, 2, 5);
INSERT INTO CourseSelection (ID, StudentID, CourseID)
VALUES(6, 2, 6);
INSERT INTO CourseSelection (ID, StudentID, CourseID)
VALUES(7, 2, 3);
INSERT INTO CourseSelection (ID, StudentID, CourseID)
VALUES(8, 3, 2);
INSERT INTO CourseSelection (ID, StudentID, CourseID)
VALUES(9, 3, 1);
INSERT INTO CourseSelection (ID, StudentID, CourseID)
VALUES(10, 4, 2);
INSERT INTO CourseSelection (ID, StudentID, CourseID)
VALUES(11, 4, 5);
INSERT INTO CourseSelection (ID, StudentID, CourseID)
VALUES(12, 4, 2);
INSERT INTO CourseSelection (ID, StudentID, CourseID)
VALUES(13, 5, 3);
INSERT INTO CourseSelection (ID, StudentID, CourseID)
VALUES(14, 5, 2);
INSERT INTO CourseSelection (ID, StudentID, CourseID)
VALUES(15, 7, 3);
</code></pre>

下面的代码包含本主题中显示的示例代码。

```fsharp
#if INTERACTIVE
#r "FSharp.Data.TypeProviders.dll"
#r "System.Data.dll"
#r "System.Data.Linq.dll"
#endif
open System
open Microsoft.FSharp.Data.TypeProviders
open System.Data.Linq.SqlClient
open System.Linq

type schema = SqlDataConnection<"Data Source=SERVER\INSTANCE;Initial Catalog=MyDatabase;Integrated Security=SSPI;">

let db = schema.GetDataContext()

let data = [1; 5; 7; 11; 18; 21]

type Nullable<'T when 'T : ( new : unit -> 'T) and 'T : struct and 'T :> ValueType > with
    member this.Print() =
        if this.HasValue then this.Value.ToString()
        else "NULL"

printfn "\ncontains query operator"
query {
    for student in db.Student do
    select student.Age.Value
    contains 11
}
|> printfn "Is at least one student age 11? %b"

printfn "\ncount query operator"
query {
    for student in db.Student do
    select student
    count
}
|> printfn "Number of students: %d"

printfn "\nlast query operator."
let num =
    query {
        for number in data do
        sortBy number
        last
    }
printfn "Last number: %d" num

open Microsoft.FSharp.Linq

printfn "\nlastOrDefault query operator."
query {
    for number in data do
    sortBy number
    lastOrDefault
}
|> printfn "lastOrDefault: %d"

printfn "\nexactlyOne query operator."
let student2 =
    query {
        for student in db.Student do
        where (student.StudentID = 1)
        select student
        exactlyOne
    }
printfn "Student with StudentID = 1 is %s" student2.Name

printfn "\nexactlyOneOrDefault query operator."
let student3 =
    query {
        for student in db.Student do
        where (student.StudentID = 1)
        select student
        exactlyOneOrDefault
    }
printfn "Student with StudentID = 1 is %s" student3.Name

printfn "\nheadOrDefault query operator."
let student4 =
    query {
        for student in db.Student do
        select student
        headOrDefault
    }
printfn "head student is %s" student4.Name

printfn "\nselect query operator."
query {
    for student in db.Student do
    select student
}
|> Seq.iter (fun student -> printfn "StudentID, Name: %d %s" student.StudentID student.Name)

printfn "\nwhere query operator."
query {
    for student in db.Student do
    where (student.StudentID > 4)
    select student
}
|> Seq.iter (fun student -> printfn "StudentID, Name: %d %s" student.StudentID student.Name)

printfn "\nminBy query operator."
let student5 =
    query {
        for student in db.Student do
        minBy student.StudentID
    }

printfn "\nmaxBy query operator."
let student6 =
    query {
        for student in db.Student do
        maxBy student.StudentID
    }

printfn "\ngroupBy query operator."
query {
    for student in db.Student do
    groupBy student.Age into g
    select (g.Key, g.Count())
}
|> Seq.iter (fun (age, count) -> printfn "Age: %s Count at that age: %d" (age.Print()) count)

printfn "\nsortBy query operator."
query {
    for student in db.Student do
    sortBy student.Name
    select student
}
|> Seq.iter (fun student -> printfn "StudentID, Name: %d %s" student.StudentID student.Name)

printfn "\nsortByDescending query operator."
query {
    for student in db.Student do
    sortByDescending student.Name
    select student
}
|> Seq.iter (fun student -> printfn "StudentID, Name: %d %s" student.StudentID student.Name)

printfn "\nthenBy query operator."
query {
    for student in db.Student do
    where student.Age.HasValue
    sortBy student.Age.Value
    thenBy student.Name
    select student
}
|> Seq.iter (fun student -> printfn "StudentID, Name: %d %s" student.Age.Value student.Name)

printfn "\nthenByDescending query operator."
query {
    for student in db.Student do
    where student.Age.HasValue
    sortBy student.Age.Value
    thenByDescending student.Name
    select student
}
|> Seq.iter (fun student -> printfn "StudentID, Name: %d %s" student.Age.Value student.Name)

printfn "\ngroupValBy query operator."
query {
    for student in db.Student do
    groupValBy student.Name student.Age into g
    select (g, g.Key, g.Count())
}
|> Seq.iter (fun (group, age, count) ->
    printfn "Age: %s Count at that age: %d" (age.Print()) count
    group |> Seq.iter (fun name -> printfn "Name: %s" name))

printfn "\n sumByNullable query operator"
query {
    for student in db.Student do
    sumByNullable student.Age
}
|> (fun sum -> printfn "Sum of ages: %s" (sum.Print()))

printfn "\n minByNullable"
query {
    for student in db.Student do
    minByNullable student.Age
}
|> (fun age -> printfn "Minimum age: %s" (age.Print()))

printfn "\n maxByNullable"
query {
    for student in db.Student do
    maxByNullable student.Age
}
|> (fun age -> printfn "Maximum age: %s" (age.Print()))

printfn "\n averageBy"
query {
    for student in db.Student do
    averageBy (float student.StudentID)
}
|> printfn "Average student ID: %f"

printfn "\n averageByNullable"
query {
    for student in db.Student do
    averageByNullable (Nullable.float student.Age)
}
|> (fun avg -> printfn "Average age: %s" (avg.Print()))

printfn "\n find query operator"
query {
    for student in db.Student do
    find (student.Name = "Abercrombie, Kim")
}
|> (fun student -> printfn "Found a match with StudentID = %d" student.StudentID)

printfn "\n all query operator"
query {
    for student in db.Student do
    all (SqlMethods.Like(student.Name, "%,%"))
}
|> printfn "Do all students have a comma in the name? %b"

printfn "\n head query operator"
query {
    for student in db.Student do
    head
}
|> (fun student -> printfn "Found the head student with StudentID = %d" student.StudentID)

printfn "\n nth query operator"
query {
    for numbers in data do
    nth 3
}
|> printfn "Third number is %d"

printfn "\n skip query operator"
query {
    for student in db.Student do
    skip 1
}
|> Seq.iter (fun student -> printfn "StudentID = %d" student.StudentID)

printfn "\n skipWhile query operator"
query {
    for number in data do
    skipWhile (number < 3)
    select number
}
|> Seq.iter (fun number -> printfn "Number = %d" number)

printfn "\n sumBy query operator"
query {
    for student in db.Student do
    sumBy student.StudentID
}
|> printfn "Sum of student IDs: %d"

printfn "\n take query operator"
query {
    for student in db.Student do
    select student
    take 2
}
|> Seq.iter (fun student -> printfn "StudentID = %d" student.StudentID)

printfn "\n takeWhile query operator"
query {
    for number in data do
    takeWhile (number < 10)
}
|> Seq.iter (fun number -> printfn "Number = %d" number)

printfn "\n sortByNullable query operator"
query {
    for student in db.Student do
    sortByNullable student.Age
    select student
}
|> Seq.iter (fun student ->
    printfn "StudentID, Name, Age: %d %s %s" student.StudentID student.Name (student.Age.Print()))

printfn "\n sortByNullableDescending query operator"
query {
    for student in db.Student do
    sortByNullableDescending student.Age
    select student
}
|> Seq.iter (fun student ->
    printfn "StudentID, Name, Age: %d %s %s" student.StudentID student.Name (student.Age.Print()))

printfn "\n thenByNullable query operator"
query {
    for student in db.Student do
    sortBy student.Name
    thenByNullable student.Age
    select student
}
|> Seq.iter (fun student ->
    printfn "StudentID, Name, Age: %d %s %s" student.StudentID student.Name (student.Age.Print()))

printfn "\n thenByNullableDescending query operator"
query {
    for student in db.Student do
    sortBy student.Name
    thenByNullableDescending student.Age
    select student
}
|> Seq.iter (fun student ->
    printfn "StudentID, Name, Age: %d %s %s" student.StudentID student.Name (student.Age.Print()))

printfn "All students: "
query {
    for student in db.Student do
    select student
}
|> Seq.iter (fun student -> printfn "%s %d %s" student.Name student.StudentID (student.Age.Print()))

printfn "\nCount of students: "
query {
    for student in db.Student do
    count
}
|> (fun count -> printfn "Student count: %d" count)

printfn "\nExists."
query {
    for student in db.Student do
    where
        (query {
            for courseSelection in db.CourseSelection do
            exists (courseSelection.StudentID = student.StudentID) })
    select student
}
|> Seq.iter (fun student -> printfn "%A" student.Name)

printfn "\n Group by age and count"
query {
    for n in db.Student do
    groupBy n.Age into g
    select (g.Key, g.Count())
}
|> Seq.iter (fun (age, count) -> printfn "%s %d" (age.Print()) count)

printfn "\n Group value by age."
query {
    for n in db.Student do
    groupValBy n.Age n.Age into g
    select (g.Key, g.Count())
}
|> Seq.iter (fun (age, count) -> printfn "%s %d" (age.Print()) count)

printfn "\nGroup students by age where age > 10."
query {
    for student in db.Student do
    groupBy student.Age into g
    where (g.Key.HasValue && g.Key.Value > 10)
    select (g, g.Key)
}
|> Seq.iter (fun (students, age) ->
    printfn "Age: %s" (age.Value.ToString())
    students
    |> Seq.iter (fun student -> printfn "%s" student.Name))

printfn "\nGroup students by age and print counts of number of students at each age with more than 1 student."
query {
    for student in db.Student do
    groupBy student.Age into group
    where (group.Count() > 1)
    select (group.Key, group.Count())
}
|> Seq.iter (fun (age, ageCount) ->
    printfn "Age: %s Count: %d" (age.Print()) ageCount)

printfn "\nGroup students by age and sum ages."
query {
    for student in db.Student do
    groupBy student.Age into g
    let total = query { for student in g do sumByNullable student.Age }
    select (g.Key, g.Count(), total)
}
|> Seq.iter (fun (age, count, total) ->
    printfn "Age: %d" (age.GetValueOrDefault())
    printfn "Count: %d" count
    printfn "Total years: %s" (total.ToString()))

printfn "\nGroup students by age and count number of students at each age, and display all with count > 1 in descending order of count."
query {
    for student in db.Student do
    groupBy student.Age into g
    where (g.Count() > 1)
    sortByDescending (g.Count())
    select (g.Key, g.Count())
}
|> Seq.iter (fun (age, myCount) ->
    printfn "Age: %s" (age.Print())
    printfn "Count: %d" myCount)

printfn "\n Select students from a set of IDs"
let idList = [1; 2; 5; 10]
let idQuery =
    query { for id in idList do select id }
query {
    for student in db.Student do
    where (idQuery.Contains(student.StudentID))
    select student
}
|> Seq.iter (fun student ->
    printfn "Name: %s" student.Name)

printfn "\nLook for students with Name match _e%% pattern and take first two."
query {
    for student in db.Student do
    where (SqlMethods.Like( student.Name, "_e%") )
    select student
    take 2
}
|> Seq.iter (fun student -> printfn "%s" student.Name)

printfn "\nLook for students with Name matching [abc]%% pattern."
query {
    for student in db.Student do
    where (SqlMethods.Like( student.Name, "[abc]%") )
    select student
}
|> Seq.iter (fun student -> printfn "%s" student.Name)

printfn "\nLook for students with name matching [^abc]%% pattern."
query {
    for student in db.Student do
    where (SqlMethods.Like( student.Name, "[^abc]%") )
    select student
}
|> Seq.iter (fun student -> printfn "%s" student.Name)

printfn "\nLook for students with name matching [^abc]%% pattern and select ID."
query {
    for n in db.Student do
    where (SqlMethods.Like( n.Name, "[^abc]%") )
    select n.StudentID
}
|> Seq.iter (fun id -> printfn "%d" id)

printfn "\n Using Contains as a query filter."
query {
    for student in db.Student do
    where (student.Name.Contains("a"))
    select student
}
|> Seq.iter (fun student -> printfn "%s" student.Name)

printfn "\nSearching for names from a list."
let names = [|"a";"b";"c"|]
query {
    for student in db.Student do
    if names.Contains (student.Name) then select student
}
|> Seq.iter (fun student -> printfn "%s" student.Name)

printfn "\nJoin Student and CourseSelection tables."
query {
    for student in db.Student do
    join selection in db.CourseSelection
        on (student.StudentID = selection.StudentID)
    select (student, selection)
}
|> Seq.iter (fun (student, selection) -> printfn "%d %s %d" student.StudentID student.Name selection.CourseID)

printfn "\nLeft Join Student and CourseSelection tables."
query {
    for student in db.Student do
    leftOuterJoin selection in db.CourseSelection
        on (student.StudentID = selection.StudentID) into result
    for selection in result.DefaultIfEmpty() do
    select (student, selection)
}
|> Seq.iter (fun (student, selection) ->
    let selectionID, studentID, courseID =
        match selection with
        | null -> "NULL", "NULL", "NULL"
        | sel -> (sel.ID.ToString(), sel.StudentID.ToString(), sel.CourseID.ToString())
    printfn "%d %s %d %s %s %s" student.StudentID student.Name (student.Age.GetValueOrDefault()) selectionID studentID courseID)

printfn "\nJoin with count"
query {
    for n in db.Student do
    join e in db.CourseSelection
        on (n.StudentID = e.StudentID)
    count
}
|> printfn "%d"

printfn "\n Join with distinct."
query {
    for student in db.Student do
    join selection in db.CourseSelection
        on (student.StudentID = selection.StudentID)
    distinct
}
|> Seq.iter (fun (student, selection) -> printfn "%s %d" student.Name selection.CourseID)

printfn "\n Join with distinct and count."
query {
    for n in db.Student do
    join e in db.CourseSelection
        on (n.StudentID = e.StudentID)
    distinct
    count
}
|> printfn "%d"

printfn "\n Selecting students with age between 10 and 15."
query {
    for student in db.Student do
    where (student.Age.Value >= 10 && student.Age.Value < 15)
    select student
}
|> Seq.iter (fun student -> printfn "%s" student.Name)

printfn "\n Selecting students with age either 11 or 12."
query {
    for student in db.Student do
    where (student.Age.Value = 11 || student.Age.Value = 12)
    select student
}
|> Seq.iter (fun student -> printfn "%s" student.Name)

printfn "\n Selecting students in a certain age range and sorting."
query {
    for n in db.Student do
    where (n.Age.Value = 12 || n.Age.Value = 13)
    sortByNullableDescending n.Age
    select n
}
|> Seq.iter (fun student -> printfn "%s %s" student.Name (student.Age.Print()))

printfn "\n Selecting students with certain ages, taking account of possibility of nulls."
query {
    for student in db.Student do
    where
        ((student.Age.HasValue && student.Age.Value = 11) ||
         (student.Age.HasValue && student.Age.Value = 12))
    sortByDescending student.Name
    select student.Name
    take 2
}
|> Seq.iter (fun name -> printfn "%s" name)

printfn "\n Union of two queries."
module Queries =
    let query1 = query {
        for n in db.Student do
        select (n.Name, n.Age)
    }

    let query2 = query {
        for n in db.LastStudent do
        select (n.Name, n.Age)
    }

    query2.Union (query1)
    |> Seq.iter (fun (name, age) -> printfn "%s %s" name (age.Print()))

printfn "\n Intersect of two queries."
module Queries2 =
    let query1 = query {
        for n in db.Student do
        select (n.Name, n.Age)
    }

    let query2 = query {
        for n in db.LastStudent do
        select (n.Name, n.Age)
    }

    query1.Intersect(query2)
    |> Seq.iter (fun (name, age) -> printfn "%s %s" name (age.Print()))

printfn "\n Using if statement to alter results for special value."
query {
    for student in db.Student do
    select
        (if student.Age.HasValue && student.Age.Value = -1 then
            (student.StudentID, System.Nullable<int>(100), student.Age)
         else (student.StudentID, student.Age, student.Age))
}
|> Seq.iter (fun (id, value, age) -> printfn "%d %s %s" id (value.Print()) (age.Print()))

printfn "\n Using if statement to alter results special values."
query {
    for student in db.Student do
    select
        (if student.Age.HasValue && student.Age.Value = -1 then
            (student.StudentID, System.Nullable<int>(100), student.Age)
         elif student.Age.HasValue && student.Age.Value = 0 then
            (student.StudentID, System.Nullable<int>(100), student.Age)
         else (student.StudentID, student.Age, student.Age))
}
|> Seq.iter (fun (id, value, age) -> printfn "%d %s %s" id (value.Print()) (age.Print()))

printfn "\n Multiple table select."
query {
    for student in db.Student do
    for course in db.Course do
    select (student, course)
}
|> Seq.iteri (fun index (student, course) ->
    if index = 0 then
        printfn "StudentID Name Age CourseID CourseName"
    printfn "%d %s %s %d %s" student.StudentID student.Name (student.Age.Print()) course.CourseID course.CourseName)

printfn "\nMultiple Joins"
query {
    for student in db.Student do
    join courseSelection in db.CourseSelection
        on (student.StudentID = courseSelection.StudentID)
    join course in db.Course
        on (courseSelection.CourseID = course.CourseID)
    select (student.Name, course.CourseName)
}
|> Seq.iter (fun (studentName, courseName) -> printfn "%s %s" studentName courseName)

printfn "\nMultiple Left Outer Joins"
query {
    for student in db.Student do
    leftOuterJoin courseSelection in db.CourseSelection
        on (student.StudentID = courseSelection.StudentID) into g1
    for courseSelection in g1.DefaultIfEmpty() do
    leftOuterJoin course in db.Course
        on (courseSelection.CourseID = course.CourseID) into g2
    for course in g2.DefaultIfEmpty() do
    select (student.Name, course.CourseName)
}
|> Seq.iter (fun (studentName, courseName) -> printfn "%s %s" studentName courseName)
```

下面是在 F# 交互窗口中运行此代码时的完整输出。

```console
--> Referenced 'C:\Program Files (x86)\Reference Assemblies\Microsoft\FSharp\3.0\Runtime\v4.0\Type Providers\FSharp.Data.TypeProviders.dll'

--> Referenced 'C:\Windows\Microsoft.NET\Framework\v4.0.30319\System.Data.dll'

--> Referenced 'C:\Windows\Microsoft.NET\Framework\v4.0.30319\System.Data.Linq.dll'

contains query operator
Binding session to 'C:\Users\ghogen\AppData\Local\Temp\tmp5E3C.dll'...
Binding session to 'C:\Users\ghogen\AppData\Local\Temp\tmp611A.dll'...
Is at least one student age 11? true

count query operator
Number of students: 8

last query operator.
Last number: 21

lastOrDefault query operator.
lastOrDefault: 21

exactlyOne query operator.
Student with StudentID = 1 is Abercrombie, Kim

exactlyOneOrDefault query operator.
Student with StudentID = 1 is Abercrombie, Kim

headOrDefault query operator.
head student is Abercrombie, Kim

select query operator.
StudentID, Name: 1 Abercrombie, Kim
StudentID, Name: 2 Abolrous, Hazen
StudentID, Name: 3 Hance, Jim
StudentID, Name: 4 Adams, Terry
StudentID, Name: 5 Hansen, Claus
StudentID, Name: 6 Penor, Lori
StudentID, Name: 7 Perham, Tom
StudentID, Name: 8 Peng, Yun-Feng

where query operator.
StudentID, Name: 5 Hansen, Claus
StudentID, Name: 6 Penor, Lori
StudentID, Name: 7 Perham, Tom
StudentID, Name: 8 Peng, Yun-Feng

minBy query operator.

maxBy query operator.

groupBy query operator.
Age: NULL Count at that age: 1
Age: 10 Count at that age: 1
Age: 11 Count at that age: 1
Age: 12 Count at that age: 3
Age: 13 Count at that age: 1
Age: 14 Count at that age: 1

sortBy query operator.
StudentID, Name: 1 Abercrombie, Kim
StudentID, Name: 2 Abolrous, Hazen
StudentID, Name: 4 Adams, Terry
StudentID, Name: 3 Hance, Jim
StudentID, Name: 5 Hansen, Claus
StudentID, Name: 8 Peng, Yun-Feng
StudentID, Name: 6 Penor, Lori
StudentID, Name: 7 Perham, Tom

sortByDescending query operator.
StudentID, Name: 7 Perham, Tom
StudentID, Name: 6 Penor, Lori
StudentID, Name: 8 Peng, Yun-Feng
StudentID, Name: 5 Hansen, Claus
StudentID, Name: 3 Hance, Jim
StudentID, Name: 4 Adams, Terry
StudentID, Name: 2 Abolrous, Hazen
StudentID, Name: 1 Abercrombie, Kim

thenBy query operator.
StudentID, Name: 10 Abercrombie, Kim
StudentID, Name: 11 Hansen, Claus
StudentID, Name: 12 Adams, Terry
StudentID, Name: 12 Hance, Jim
StudentID, Name: 12 Perham, Tom
StudentID, Name: 13 Penor, Lori
StudentID, Name: 14 Abolrous, Hazen

thenByDescending query operator.
StudentID, Name: 10 Abercrombie, Kim
StudentID, Name: 11 Hansen, Claus
StudentID, Name: 12 Perham, Tom
StudentID, Name: 12 Hance, Jim
StudentID, Name: 12 Adams, Terry
StudentID, Name: 13 Penor, Lori
StudentID, Name: 14 Abolrous, Hazen

groupValBy query operator.
Age: NULL Count at that age: 1
Name: Peng, Yun-Feng
Age: 10 Count at that age: 1
Name: Abercrombie, Kim
Age: 11 Count at that age: 1
Name: Hansen, Claus
Age: 12 Count at that age: 3
Name: Hance, Jim
Name: Adams, Terry
Name: Perham, Tom
Age: 13 Count at that age: 1
Name: Penor, Lori
Age: 14 Count at that age: 1
Name: Abolrous, Hazen

sumByNullable query operator
Sum of ages: 84

minByNullable
Minimum age: 10

maxByNullable
Maximum age: 14

averageBy
Average student ID: 4.500000

averageByNullable
Average age: 12

find query operator
Found a match with StudentID = 1

all query operator
Do all students have a comma in the name? true

head query operator
Found the head student with StudentID = 1

nth query operator
Third number is 11

skip query operator
StudentID = 2
StudentID = 3
StudentID = 4
StudentID = 5
StudentID = 6
StudentID = 7
StudentID = 8

skipWhile query operator
Number = 5
Number = 7
Number = 11
Number = 18
Number = 21

sumBy query operator
Sum of student IDs: 36

take query operator
StudentID = 1
StudentID = 2

takeWhile query operator
Number = 1
Number = 5
Number = 7

sortByNullable query operator
StudentID, Name, Age: 8 Peng, Yun-Feng NULL
StudentID, Name, Age: 1 Abercrombie, Kim 10
StudentID, Name, Age: 5 Hansen, Claus 11
StudentID, Name, Age: 7 Perham, Tom 12
StudentID, Name, Age: 3 Hance, Jim 12
StudentID, Name, Age: 4 Adams, Terry 12
StudentID, Name, Age: 6 Penor, Lori 13
StudentID, Name, Age: 2 Abolrous, Hazen 14

sortByNullableDescending query operator
StudentID, Name, Age: 2 Abolrous, Hazen 14
StudentID, Name, Age: 6 Penor, Lori 13
StudentID, Name, Age: 7 Perham, Tom 12
StudentID, Name, Age: 3 Hance, Jim 12
StudentID, Name, Age: 4 Adams, Terry 12
StudentID, Name, Age: 5 Hansen, Claus 11
StudentID, Name, Age: 1 Abercrombie, Kim 10
StudentID, Name, Age: 8 Peng, Yun-Feng NULL

thenByNullable query operator
StudentID, Name, Age: 1 Abercrombie, Kim 10
StudentID, Name, Age: 2 Abolrous, Hazen 14
StudentID, Name, Age: 4 Adams, Terry 12
StudentID, Name, Age: 3 Hance, Jim 12
StudentID, Name, Age: 5 Hansen, Claus 11
StudentID, Name, Age: 8 Peng, Yun-Feng NULL
StudentID, Name, Age: 6 Penor, Lori 13
StudentID, Name, Age: 7 Perham, Tom 12

thenByNullableDescending query operator
StudentID, Name, Age: 1 Abercrombie, Kim 10
StudentID, Name, Age: 2 Abolrous, Hazen 14
StudentID, Name, Age: 4 Adams, Terry 12
StudentID, Name, Age: 3 Hance, Jim 12
StudentID, Name, Age: 5 Hansen, Claus 11
StudentID, Name, Age: 8 Peng, Yun-Feng NULL
StudentID, Name, Age: 6 Penor, Lori 13
StudentID, Name, Age: 7 Perham, Tom 12
All students:
Abercrombie, Kim 1 10
Abolrous, Hazen 2 14
Hance, Jim 3 12
Adams, Terry 4 12
Hansen, Claus 5 11
Penor, Lori 6 13
Perham, Tom 7 12
Peng, Yun-Feng 8 NULL

Count of students:
Student count: 8

Exists.
"Abercrombie, Kim"
"Abolrous, Hazen"
"Hance, Jim"
"Adams, Terry"
"Hansen, Claus"
"Perham, Tom"

Group by age and count
NULL 1
10 1
11 1
12 3
13 1
14 1

Group value by age.
NULL 1
10 1
11 1
12 3
13 1
14 1

Group students by age where age > 10.
Age: 11
Hansen, Claus
Age: 12
Hance, Jim
Adams, Terry
Perham, Tom
Age: 13
Penor, Lori
Age: 14
Abolrous, Hazen

Group students by age and print counts of number of students at each age with more than 1 student.
Age: 12 Count: 3

Group students by age and sum ages.
Age: 0
Count: 1
Total years:
Age: 10
Count: 1
Total years: 10
Age: 11
Count: 1
Total years: 11
Age: 12
Count: 3
Total years: 36
Age: 13
Count: 1
Total years: 13
Age: 14
Count: 1
Total years: 14

Group students by age and count number of students at each age, and display all with count > 1 in descending order of count.
Age: 12
Count: 3

Select students from a set of IDs
Name: Abercrombie, Kim
Name: Abolrous, Hazen
Name: Hansen, Claus

Look for students with Name match _e% pattern and take first two.
Penor, Lori
Perham, Tom

Look for students with Name matching [abc]% pattern.
Abercrombie, Kim
Abolrous, Hazen
Adams, Terry

Look for students with name matching [^abc]% pattern.
Hance, Jim
Hansen, Claus
Penor, Lori
Perham, Tom
Peng, Yun-Feng

Look for students with name matching [^abc]% pattern and select ID.
3
5
6
7
8

Using Contains as a query filter.
Abercrombie, Kim
Abolrous, Hazen
Hance, Jim
Adams, Terry
Hansen, Claus
Perham, Tom

Searching for names from a list.

Join Student and CourseSelection tables.
2 Abolrous, Hazen 2
3 Hance, Jim 3
5 Hansen, Claus 5
2 Abolrous, Hazen 2
5 Hansen, Claus 5
6 Penor, Lori 6
3 Hance, Jim 3
2 Abolrous, Hazen 2
1 Abercrombie, Kim 1
2 Abolrous, Hazen 2
5 Hansen, Claus 5
2 Abolrous, Hazen 2
3 Hance, Jim 3
2 Abolrous, Hazen 2
3 Hance, Jim 3

Left Join Student and CourseSelection tables.
1 Abercrombie, Kim 10 9 3 1
2 Abolrous, Hazen 14 1 1 2
2 Abolrous, Hazen 14 4 2 2
2 Abolrous, Hazen 14 8 3 2
2 Abolrous, Hazen 14 10 4 2
2 Abolrous, Hazen 14 12 4 2
2 Abolrous, Hazen 14 14 5 2
3 Hance, Jim 12 2 1 3
3 Hance, Jim 12 7 2 3
3 Hance, Jim 12 13 5 3
3 Hance, Jim 12 15 7 3
4 Adams, Terry 12 NULL NULL NULL
5 Hansen, Claus 11 3 1 5
5 Hansen, Claus 11 5 2 5
5 Hansen, Claus 11 11 4 5
6 Penor, Lori 13 6 2 6
7 Perham, Tom 12 NULL NULL NULL
8 Peng, Yun-Feng 0 NULL NULL NULL

Join with count
15

Join with distinct.
Abercrombie, Kim 2
Abercrombie, Kim 3
Abercrombie, Kim 5
Abolrous, Hazen 2
Abolrous, Hazen 5
Abolrous, Hazen 6
Abolrous, Hazen 3
Hance, Jim 2
Hance, Jim 1
Adams, Terry 2
Adams, Terry 5
Adams, Terry 2
Hansen, Claus 3
Hansen, Claus 2
Perham, Tom 3

Join with distinct and count.
15

Selecting students with age between 10 and 15.
Abercrombie, Kim
Abolrous, Hazen
Hance, Jim
Adams, Terry
Hansen, Claus
Penor, Lori
Perham, Tom

Selecting students with age either 11 or 12.
Hance, Jim
Adams, Terry
Hansen, Claus
Perham, Tom

Selecting students in a certain age range and sorting.
Penor, Lori 13
Perham, Tom 12
Hance, Jim 12
Adams, Terry 12

Selecting students with certain ages, taking account of possibility of nulls.
Hance, Jim
Adams, Terry

Union of two queries.
Abercrombie, Kim 10
Abolrous, Hazen 14
Hance, Jim 12
Adams, Terry 12
Hansen, Claus 11
Penor, Lori 13
Perham, Tom 12
Peng, Yun-Feng NULL

Intersect of two queries.

Using if statement to alter results for special value.
1 10 10
2 14 14
3 12 12
4 12 12
5 11 11
6 13 13
7 12 12
8 NULL NULL

Using if statement to alter results special values.
1 10 10
2 14 14
3 12 12
4 12 12
5 11 11
6 13 13
7 12 12
8 NULL NULL

Multiple table select.
StudentID Name Age CourseID CourseName
1 Abercrombie, Kim 10 1 Algebra I
2 Abolrous, Hazen 14 1 Algebra I
3 Hance, Jim 12 1 Algebra I
4 Adams, Terry 12 1 Algebra I
5 Hansen, Claus 11 1 Algebra I
6 Penor, Lori 13 1 Algebra I
7 Perham, Tom 12 1 Algebra I
8 Peng, Yun-Feng NULL 1 Algebra I
1 Abercrombie, Kim 10 2 Trigonometry
2 Abolrous, Hazen 14 2 Trigonometry
3 Hance, Jim 12 2 Trigonometry
4 Adams, Terry 12 2 Trigonometry
5 Hansen, Claus 11 2 Trigonometry
6 Penor, Lori 13 2 Trigonometry
7 Perham, Tom 12 2 Trigonometry
8 Peng, Yun-Feng NULL 2 Trigonometry
1 Abercrombie, Kim 10 3 Algebra II
2 Abolrous, Hazen 14 3 Algebra II
3 Hance, Jim 12 3 Algebra II
4 Adams, Terry 12 3 Algebra II
5 Hansen, Claus 11 3 Algebra II
6 Penor, Lori 13 3 Algebra II
7 Perham, Tom 12 3 Algebra II
8 Peng, Yun-Feng NULL 3 Algebra II
1 Abercrombie, Kim 10 4 History
2 Abolrous, Hazen 14 4 History
3 Hance, Jim 12 4 History
4 Adams, Terry 12 4 History
5 Hansen, Claus 11 4 History
6 Penor, Lori 13 4 History
7 Perham, Tom 12 4 History
8 Peng, Yun-Feng NULL 4 History
1 Abercrombie, Kim 10 5 English
2 Abolrous, Hazen 14 5 English
3 Hance, Jim 12 5 English
4 Adams, Terry 12 5 English
5 Hansen, Claus 11 5 English
6 Penor, Lori 13 5 English
7 Perham, Tom 12 5 English
8 Peng, Yun-Feng NULL 5 English
1 Abercrombie, Kim 10 6 French
2 Abolrous, Hazen 14 6 French
3 Hance, Jim 12 6 French
4 Adams, Terry 12 6 French
5 Hansen, Claus 11 6 French
6 Penor, Lori 13 6 French
7 Perham, Tom 12 6 French
8 Peng, Yun-Feng NULL 6 French
1 Abercrombie, Kim 10 7 Chinese
2 Abolrous, Hazen 14 7 Chinese
3 Hance, Jim 12 7 Chinese
4 Adams, Terry 12 7 Chinese
5 Hansen, Claus 11 7 Chinese
6 Penor, Lori 13 7 Chinese
7 Perham, Tom 12 7 Chinese
8 Peng, Yun-Feng NULL 7 Chinese

Multiple Joins
Abercrombie, Kim Trigonometry
Abercrombie, Kim Algebra II
Abercrombie, Kim English
Abolrous, Hazen Trigonometry
Abolrous, Hazen English
Abolrous, Hazen French
Abolrous, Hazen Algebra II
Hance, Jim Trigonometry
Hance, Jim Algebra I
Adams, Terry Trigonometry
Adams, Terry English
Adams, Terry Trigonometry
Hansen, Claus Algebra II
Hansen, Claus Trigonometry
Perham, Tom Algebra II

Multiple Left Outer Joins
Abercrombie, Kim Trigonometry
Abercrombie, Kim Algebra II
Abercrombie, Kim English
Abolrous, Hazen Trigonometry
Abolrous, Hazen English
Abolrous, Hazen French
Abolrous, Hazen Algebra II
Hance, Jim Trigonometry
Hance, Jim Algebra I
Adams, Terry Trigonometry
Adams, Terry English
Adams, Terry Trigonometry
Hansen, Claus Algebra II
Hansen, Claus Trigonometry
Penor, Lori
Perham, Tom Algebra II
Peng, Yun-Feng

type schema
val db : schema.ServiceTypes.SimpleDataContextTypes.MyDatabase1
val student : System.Data.Linq.Table<schema.ServiceTypes.Student>
val data : int list = [1; 5; 7; 11; 18; 21]
type Nullable<'T
                when 'T : (new : unit ->  'T) and 'T : struct and
                     'T :> System.ValueType> with
  member Print : unit -> string
val num : int = 21
val student2 : schema.ServiceTypes.Student
val student3 : schema.ServiceTypes.Student
val student4 : schema.ServiceTypes.Student
val student5 : int = 1
val student6 : int = 8
val idList : int list = [1; 2; 5; 10]
val idQuery : seq<int>
val names : string [] = [|"a"; "b"; "c"|]
module Queries = begin
  val query1 : System.Linq.IQueryable<string * System.Nullable<int>>
  val query2 : System.Linq.IQueryable<string * System.Nullable<int>>
end
module Queries2 = begin
  val query1 : System.Linq.IQueryable<string * System.Nullable<int>>
  val query2 : System.Linq.IQueryable<string * System.Nullable<int>>
end
```

## <a name="see-also"></a>另请参阅

- [F# 语言参考](index.md)
- [QueryBuilder 类](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-linq-querybuilder.html)
- [计算表达式](Computation-Expressions.md)
