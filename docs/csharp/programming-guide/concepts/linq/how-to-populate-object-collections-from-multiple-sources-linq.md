---
title: 如何从多个源填充对象集合 (LINQ) (C#)
description: 了解如何使用 C# 中的 LINQ 将来自不同源的数据合并到一系列新的类型。 这些示例同时使用匿名和命名类型。
ms.date: 06/12/2018
ms.assetid: 8ad7d480-b46c-4ccc-8c57-76f2d04ccc6d
ms.openlocfilehash: 9dc9f98ae09e0fe3437b5d2ccab32b3dbcd93714
ms.sourcegitcommit: 04022ca5d00b2074e1b1ffdbd76bec4950697c4c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/23/2020
ms.locfileid: "87104717"
---
# <a name="how-to-populate-object-collections-from-multiple-sources-linq-c"></a>如何从多个源填充对象集合 (LINQ) (C#)

本示例演示如何将来自不同源的数据合并到一系列新的类型。

> [!NOTE]
> 请勿尝试将内存中数据或文件系统中的数据与仍在数据库中的数据进行联接。 这种跨域联接可能产生未定义的结果，因为可能为数据库查询和其他类型的源定义了联接操作的不同方式。 此外，如果数据库中的数据量足够大，这样的操作还存在可能导致内存不足的异常的风险。 若要将数据库中的数据联接到内存数据，首先对数据库查询调用 `ToList` 或 `ToArray`，然后对返回的集合执行联接。

## <a name="to-create-the-data-file"></a>创建数据文件

按照[如何联接不同文件中的内容 (LINQ) (C#)](./how-to-join-content-from-dissimilar-files-linq.md) 中的说明，将 names.csv 和 scores.csv 文件复制到项目文件夹。

## <a name="example"></a>示例

下面的示例演示如何使用命名类型 `Student` 存储来自两个内存字符串集合（模拟 .csv 格式的电子表格数据）的合并数据。 第一个字符串集合代表学生姓名和 ID，第二个集合代表学生 ID（在第一列）和四次考试分数。 此 ID 用作外键。

```csharp
using System;
using System.Collections.Generic;
using System.Linq;

class Student
{
    public string FirstName { get; set; }
    public string LastName { get; set; }
    public int ID { get; set; }
    public List<int> ExamScores { get; set; }
}

class PopulateCollection
{
    static void Main()
    {
        // These data files are defined in How to join content from
        // dissimilar files (LINQ).

        // Each line of names.csv consists of a last name, a first name, and an
        // ID number, separated by commas. For example, Omelchenko,Svetlana,111
        string[] names = System.IO.File.ReadAllLines(@"../../../names.csv");

        // Each line of scores.csv consists of an ID number and four test
        // scores, separated by commas. For example, 111, 97, 92, 81, 60
        string[] scores = System.IO.File.ReadAllLines(@"../../../scores.csv");

        // Merge the data sources using a named type.
        // var could be used instead of an explicit type. Note the dynamic
        // creation of a list of ints for the ExamScores member. The first item
        // is skipped in the split string because it is the student ID,
        // not an exam score.
        IEnumerable<Student> queryNamesScores =
            from nameLine in names
            let splitName = nameLine.Split(',')
            from scoreLine in scores
            let splitScoreLine = scoreLine.Split(',')
            where Convert.ToInt32(splitName[2]) == Convert.ToInt32(splitScoreLine[0])
            select new Student()
            {
                FirstName = splitName[0],
                LastName = splitName[1],
                ID = Convert.ToInt32(splitName[2]),
                ExamScores = (from scoreAsText in splitScoreLine.Skip(1)
                              select Convert.ToInt32(scoreAsText)).
                              ToList()
            };

        // Optional. Store the newly created student objects in memory
        // for faster access in future queries. This could be useful with
        // very large data files.
        List<Student> students = queryNamesScores.ToList();

        // Display each student's name and exam score average.
        foreach (var student in students)
        {
            Console.WriteLine("The average score of {0} {1} is {2}.",
                student.FirstName, student.LastName,
                student.ExamScores.Average());
        }

        //Keep console window open in debug mode
        Console.WriteLine("Press any key to exit.");
        Console.ReadKey();
    }
}
/* Output:
    The average score of Omelchenko Svetlana is 82.5.
    The average score of O'Donnell Claire is 72.25.
    The average score of Mortensen Sven is 84.5.
    The average score of Garcia Cesar is 88.25.
    The average score of Garcia Debra is 67.
    The average score of Fakhouri Fadi is 92.25.
    The average score of Feng Hanying is 88.
    The average score of Garcia Hugo is 85.75.
    The average score of Tucker Lance is 81.75.
    The average score of Adams Terry is 85.25.
    The average score of Zabokritski Eugene is 83.
    The average score of Tucker Michael is 92.
 */
```

在 [select](../../../language-reference/keywords/select-clause.md) 子句中，对象初始值设定项使用来自两个源的数据实例化每个新的 `Student` 对象。

如果不需要存储查询的结果，那么和命名类型相比，匿名类型使用起来更方便。 如果在执行查询的方法外部传递查询结果，则需要使用命名类型。 下面的示例执行与前面示例相同的任务，但使用的是匿名类型，而不是命名类型：

```csharp
// Merge the data sources by using an anonymous type.
// Note the dynamic creation of a list of ints for the
// ExamScores member. We skip 1 because the first string
// in the array is the student ID, not an exam score.
var queryNamesScores2 =
    from nameLine in names
    let splitName = nameLine.Split(',')
    from scoreLine in scores
    let splitScoreLine = scoreLine.Split(',')
    where Convert.ToInt32(splitName[2]) == Convert.ToInt32(splitScoreLine[0])
    select new
    {
        First = splitName[0],
        Last = splitName[1],
        ExamScores = (from scoreAsText in splitScoreLine.Skip(1)
                      select Convert.ToInt32(scoreAsText))
                      .ToList()
    };

// Display each student's name and exam score average.
foreach (var student in queryNamesScores2)
{
    Console.WriteLine("The average score of {0} {1} is {2}.",
        student.First, student.Last, student.ExamScores.Average());
}
```

## <a name="see-also"></a>请参阅

- [LINQ 和字符串 (C#)](./linq-and-strings.md)
- [对象和集合初始值设定项](../../classes-and-structs/object-and-collection-initializers.md)
- [匿名类型](../../classes-and-structs/anonymous-types.md)
