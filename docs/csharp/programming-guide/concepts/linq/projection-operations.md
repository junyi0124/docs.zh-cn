---
title: 投影运算 (C#)
description: 了解投影运算。 这些运算将对象转换为新的窗体，该窗体通常只包含稍后将用到的属性。
ms.date: 07/20/2015
ms.assetid: 98df573a-aad9-4b8c-9a71-844be2c4fb41
ms.openlocfilehash: 6128b1bb2e7ba3dbb1b428d475acc307ba931013
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91185998"
---
# <a name="projection-operations-c"></a>投影运算 (C#)

投影是指将对象转换为一种新形式的操作，该形式通常只包含那些将随后使用的属性。 通过使用投影，您可以构造从每个对象生成的新类型。 可以投影属性，并对该属性执行数学函数。 还可以在不更改原始对象的情况下投影该对象。  
  
 下面一节列出了执行投影的标准查询运算符方法。  
  
## <a name="methods"></a>方法  
  
|方法名|描述|C# 查询表达式语法|更多信息|  
|-----------------|-----------------|---------------------------------|----------------------|  
|选择|投影基于转换函数的值。|`select`|<xref:System.Linq.Enumerable.Select%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.Select%2A?displayProperty=nameWithType>|  
|SelectMany|投影基于转换函数的值序列，然后将它们展平为一个序列。|使用多个 `from` 子句|<xref:System.Linq.Enumerable.SelectMany%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.SelectMany%2A?displayProperty=nameWithType>|  
  
## <a name="query-expression-syntax-examples"></a>查询表达式语法示例  
  
### <a name="select"></a>选择  

 下面的示例使用 `select` 子句来投影字符串列表中每个字符串的第一个字母。  
  
```csharp  
List<string> words = new List<string>() { "an", "apple", "a", "day" };  
  
var query = from word in words  
            select word.Substring(0, 1);  
  
foreach (string s in query)  
    Console.WriteLine(s);  
  
/* This code produces the following output:  
  
    a  
    a  
    a  
    d  
*/  
```  
  
### <a name="selectmany"></a>SelectMany  

 下面的示例使用多个 `from` 子句来投影字符串列表中每个字符串中的每个单词。  
  
```csharp  
List<string> phrases = new List<string>() { "an apple a day", "the quick brown fox" };  
  
var query = from phrase in phrases  
            from word in phrase.Split(' ')  
            select word;  
  
foreach (string s in query)  
    Console.WriteLine(s);  
  
/* This code produces the following output:  
  
    an  
    apple  
    a  
    day  
    the  
    quick  
    brown  
    fox  
*/  
```  
  
## <a name="select-versus-selectmany"></a>Select 与 SelectMany  

 `Select()` 和 `SelectMany()` 的工作都是依据源值生成一个或多个结果值。 `Select()` 为每个源值生成一个结果值。 因此，总体结果是一个与源集合具有相同元素数目的集合。 与之相反，`SelectMany()` 生成单个总体结果，其中包含来自每个源值的串联子集合。 作为参数传递到 `SelectMany()` 的转换函数必须为每个源值返回一个可枚举值序列。 然后，`SelectMany()` 串联这些可枚举序列，以创建一个大的序列。  
  
 下面两个插图演示了这两个方法的操作之间的概念性区别。 在每种情况下，假定选择器（转换）函数从每个源值中选择一个由花卉数据组成的数组。  
  
 下图描述 `Select()` 如何返回一个与源集合具有相同元素数目的集合。  
  
 ![显示 Select() 的操作的图](./media/projection-operations/select-action-graphic.png)  
  
 下图描述 `SelectMany()` 如何将中间数组序列串联为一个最终结果值，其中包含每个中间数组中的每个值。  
  
 ![显示 SelectMany() 的操作的图。](./media/projection-operations/select-many-action-graphic.png )  
  
### <a name="code-example"></a>代码示例  

 下面的示例比较 `Select()` 和 `SelectMany()` 的行为。 代码通过从源集合的每个花卉名称列表中提取前两项来创建一个“花束”。 此示例中，transform 函数 <xref:System.Linq.Enumerable.Select%60%602%28System.Collections.Generic.IEnumerable%7B%60%600%7D%2CSystem.Func%7B%60%600%2C%60%601%7D%29> 使用的“单值”本身即是值的集合。 这需要额外的 `foreach` 循环，以便枚举每个子序列中的每个字符串。  
  
```csharp  
class Bouquet  
{  
    public List<string> Flowers { get; set; }  
}  
  
static void SelectVsSelectMany()  
{  
    List<Bouquet> bouquets = new List<Bouquet>() {  
        new Bouquet { Flowers = new List<string> { "sunflower", "daisy", "daffodil", "larkspur" }},  
        new Bouquet{ Flowers = new List<string> { "tulip", "rose", "orchid" }},  
        new Bouquet{ Flowers = new List<string> { "gladiolis", "lily", "snapdragon", "aster", "protea" }},  
        new Bouquet{ Flowers = new List<string> { "larkspur", "lilac", "iris", "dahlia" }}  
    };  
  
    // *********** Select ***********
    IEnumerable<List<string>> query1 = bouquets.Select(bq => bq.Flowers);  
  
    // ********* SelectMany *********  
    IEnumerable<string> query2 = bouquets.SelectMany(bq => bq.Flowers);  
  
    Console.WriteLine("Results by using Select():");  
    // Note the extra foreach loop here.  
    foreach (IEnumerable<String> collection in query1)  
        foreach (string item in collection)  
            Console.WriteLine(item);  
  
    Console.WriteLine("\nResults by using SelectMany():");  
    foreach (string item in query2)  
        Console.WriteLine(item);  
  
    /* This code produces the following output:  
  
       Results by using Select():  
        sunflower  
        daisy  
        daffodil  
        larkspur  
        tulip  
        rose  
        orchid  
        gladiolis  
        lily  
        snapdragon  
        aster  
        protea  
        larkspur  
        lilac  
        iris  
        dahlia  
  
       Results by using SelectMany():  
        sunflower  
        daisy  
        daffodil  
        larkspur  
        tulip  
        rose  
        orchid  
        gladiolis  
        lily  
        snapdragon  
        aster  
        protea  
        larkspur  
        lilac  
        iris  
        dahlia  
    */  
  
}  
```  
  
## <a name="see-also"></a>请参阅

- <xref:System.Linq>
- [标准查询运算符概述 (C#)](./standard-query-operators-overview.md)
- [select 子句](../../../language-reference/keywords/select-clause.md)
- [如何从多个源填充对象集合 (LINQ) (C#)](./how-to-populate-object-collections-from-multiple-sources-linq.md)
- [如何使用组将一个文件拆分成多个文件 (LINQ) (C#)](./how-to-split-a-file-into-many-files-by-using-groups-linq.md)
