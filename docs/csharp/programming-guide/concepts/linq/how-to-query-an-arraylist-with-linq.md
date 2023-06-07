---
title: 如何使用 LINQ 查询 ArrayList (C#)
description: 此示例使用 LINQ 对 C# 中的 ArrayList 进行查询。 必须声明范围变量的类型，以反映集合中对象的类型。
ms.date: 07/20/2015
ms.assetid: 2bfb471c-6e9a-4e60-bd83-4a1778abde11
ms.openlocfilehash: 278c05cfc864ee4f53e1215a2acb739efd87f8b1
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91153997"
---
# <a name="how-to-query-an-arraylist-with-linq-c"></a>如何使用 LINQ 查询 ArrayList (C#)

如果使用 LINQ 来查询非泛型 <xref:System.Collections.IEnumerable> 集合（例如 <xref:System.Collections.ArrayList>），必须显式声明范围变量的类型，以反映集合中对象的特定类型。 例如，如果有 `Student` 对象的 <xref:System.Collections.ArrayList>，那么 [from 子句](../../../language-reference/keywords/from-clause.md)应如下所示：  
  
```csharp
var query = from Student s in arrList  
//...
```  
  
 通过指定范围变量的类型，可将 <xref:System.Collections.ArrayList> 中的每项强制转换为 `Student`。  
  
 在查询表达式中使用显式类型范围变量等效于调用 <xref:System.Linq.Enumerable.Cast%2A> 方法。 如果无法执行指定的强制转换，<xref:System.Linq.Enumerable.Cast%2A> 将引发异常。 <xref:System.Linq.Enumerable.Cast%2A> 和 <xref:System.Linq.Enumerable.OfType%2A> 是两个标准查询运算符方法，可对非泛型 <xref:System.Collections.IEnumerable> 类型执行操作。 有关详细信息，请参阅 [LINQ 查询操作中的类型关系](./type-relationships-in-linq-query-operations.md)。  
  
## <a name="example"></a>示例  

 下面的示例演示对 <xref:System.Collections.ArrayList> 的简单查询。 请注意，本示例在代码调用 <xref:System.Collections.ArrayList.Add%2A> 方法时使用对象初始值设定项，但这不是必需的。  
  
```csharp  
using System;  
using System.Collections;  
using System.Linq;  
  
namespace NonGenericLINQ  
{  
    public class Student  
    {  
        public string FirstName { get; set; }  
        public string LastName { get; set; }  
        public int[] Scores { get; set; }  
    }  
  
    class Program  
    {  
        static void Main(string[] args)  
        {  
            ArrayList arrList = new ArrayList();  
            arrList.Add(  
                new Student  
                    {  
                        FirstName = "Svetlana", LastName = "Omelchenko", Scores = new int[] { 98, 92, 81, 60 }  
                    });  
            arrList.Add(  
                new Student  
                    {  
                        FirstName = "Claire", LastName = "O’Donnell", Scores = new int[] { 75, 84, 91, 39 }  
                    });  
            arrList.Add(  
                new Student  
                    {  
                        FirstName = "Sven", LastName = "Mortensen", Scores = new int[] { 88, 94, 65, 91 }  
                    });  
            arrList.Add(  
                new Student  
                    {  
                        FirstName = "Cesar", LastName = "Garcia", Scores = new int[] { 97, 89, 85, 82 }  
                    });  
  
            var query = from Student student in arrList  
                        where student.Scores[0] > 95  
                        select student;  
  
            foreach (Student s in query)  
                Console.WriteLine(s.LastName + ": " + s.Scores[0]);  
  
            // Keep the console window open in debug mode.  
            Console.WriteLine("Press any key to exit.");  
            Console.ReadKey();  
        }  
    }  
}  
/* Output:
    Omelchenko: 98  
    Garcia: 97  
*/  
```  
  
## <a name="see-also"></a>请参阅

- [LINQ to Objects (C#)](./linq-to-objects.md)
