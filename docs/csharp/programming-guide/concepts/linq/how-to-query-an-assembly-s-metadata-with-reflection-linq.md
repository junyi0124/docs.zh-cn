---
title: 如何使用反射查询程序集的元数据 (LINQ) (C#)
description: 了解如何在 C# 中将 LINQ 与 .NET 反射 API 配合使用来检索与搜索条件匹配的方法的特定元数据。
ms.date: 07/20/2015
ms.assetid: c4cdce49-b1c8-4420-b12a-9ff7e6671368
ms.openlocfilehash: dc5352e9cb90e9ad2808fb027823174d07d69da6
ms.sourcegitcommit: 04022ca5d00b2074e1b1ffdbd76bec4950697c4c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/23/2020
ms.locfileid: "87104588"
---
# <a name="how-to-query-an-assemblys-metadata-with-reflection-linq-c"></a>如何使用反射查询程序集的元数据 (LINQ) (C#)

.NET 反射 API 可用于检查 .NET 程序集中的元数据，以及创建位于该程序集中的类型、类型成员、参数等等的集合。 因为这些集合支持泛型 <xref:System.Collections.Generic.IEnumerable%601> 接口，所以可以使用 LINQ 查询它们。  
  
下面的示例演示了如何将 LINQ 与反射配合使用以检索有关与指定搜索条件匹配的方法的特定元数据。 在这种情况下，该查询将在返回数组等可枚举类型的程序集中查找所有方法的名称。  
  
## <a name="example"></a>示例  
  
```csharp  
using System;
using System.Linq;
using System.Reflection;  

class ReflectionHowTO  
{  
    static void Main()  
    {  
        Assembly assembly = Assembly.Load("System.Core, Version=3.5.0.0, Culture=neutral, PublicKeyToken= b77a5c561934e089");  
        var pubTypesQuery = from type in assembly.GetTypes()  
                    where type.IsPublic  
                        from method in type.GetMethods()  
                        where method.ReturnType.IsArray == true
                            || ( method.ReturnType.GetInterface(  
                                typeof(System.Collections.Generic.IEnumerable<>).FullName ) != null  
                            && method.ReturnType.FullName != "System.String" )  
                        group method.ToString() by type.ToString();  

        foreach (var groupOfMethods in pubTypesQuery)  
        {  
            Console.WriteLine("Type: {0}", groupOfMethods.Key);  
            foreach (var method in groupOfMethods)  
            {  
                Console.WriteLine("  {0}", method);  
            }  
        }  

        Console.WriteLine("Press any key to exit... ");  
        Console.ReadKey();  
    }  
}
```  

该示例使用 <xref:System.Reflection.Assembly.GetTypes%2A?displayProperty=nameWithType> 方法返回指定程序集中的类型的数组。 将应用 [where](../../../language-reference/keywords/where-clause.md) 筛选器，以便仅返回公共类型。 对于每个公共类型，子查询使用从 <xref:System.Type.GetMethods%2A?displayProperty=nameWithType> 调用返回的 <xref:System.Reflection.MethodInfo> 数组生成。 筛选这些结果，以仅返回其返回类型为数组或实现 <xref:System.Collections.Generic.IEnumerable%601> 的其他类型的方法。 最后，通过使用类型名称作为键来对这些结果进行分组。  
  
## <a name="see-also"></a>请参阅

- [LINQ to Objects (C#)](./linq-to-objects.md)
