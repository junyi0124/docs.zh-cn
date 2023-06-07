---
title: 使用反射访问特性 (C#)
description: 通过使用 GetCustomAttributes 方法，使用反射获取 C# 中使用自定义特性定义的信息。
ms.date: 07/20/2015
ms.assetid: dce3a696-4ceb-489a-b5e4-322a83052f18
ms.openlocfilehash: 2ed4e844d1c98bcf265572f201bce6679fd642e9
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91202625"
---
# <a name="accessing-attributes-by-using-reflection-c"></a>使用反射访问特性 (C#)

你可以定义自定义特性并将其放入源代码中这一事实，在没有检索该信息并对其进行操作的方法的情况下将没有任何价值。 通过使用反射，可以检索通过自定义特性定义的信息。 主要方法是 `GetCustomAttributes`，它返回对象数组，这些对象在运行时等效于源代码特性。 此方法有多个重载版本。 有关详细信息，请参阅 <xref:System.Attribute>。  
  
 特性规范，例如：  
  
```csharp  
[Author("P. Ackerman", version = 1.1)]  
class SampleClass  
```  
  
 在概念上等效于此：  
  
```csharp  
Author anonymousAuthorObject = new Author("P. Ackerman");  
anonymousAuthorObject.version = 1.1;  
```  
  
 但是，在为特性查询 `SampleClass` 之前，代码将不会执行。 对 `SampleClass` 调用 `GetCustomAttributes` 会导致按上述方式构造并初始化一个 `Author` 对象。 如果该类具有其他特性，则将以类似方式构造其他特性对象。 然后 `GetCustomAttributes` 会以数组形式返回 `Author` 对象和任何其他特性对象。 之后你便可以循环访问此数组，根据每个数组元素的类型确定所应用的特性，并从特性对象中提取信息。  
  
## <a name="example"></a>示例  

 此处是一个完整的示例。 定义自定义特性、将其应用于多个实体，并通过反射对其进行检索。  
  
```csharp  
// Multiuse attribute.  
[System.AttributeUsage(System.AttributeTargets.Class |  
                       System.AttributeTargets.Struct,  
                       AllowMultiple = true)  // Multiuse attribute.  
]  
public class Author : System.Attribute  
{  
    string name;  
    public double version;  
  
    public Author(string name)  
    {  
        this.name = name;  
  
        // Default value.  
        version = 1.0;  
    }  
  
    public string GetName()  
    {  
        return name;  
    }  
}  
  
// Class with the Author attribute.  
[Author("P. Ackerman")]  
public class FirstClass  
{  
    // ...  
}  
  
// Class without the Author attribute.  
public class SecondClass  
{  
    // ...  
}  
  
// Class with multiple Author attributes.  
[Author("P. Ackerman"), Author("R. Koch", version = 2.0)]  
public class ThirdClass  
{  
    // ...  
}  
  
class TestAuthorAttribute  
{  
    static void Test()  
    {  
        PrintAuthorInfo(typeof(FirstClass));  
        PrintAuthorInfo(typeof(SecondClass));  
        PrintAuthorInfo(typeof(ThirdClass));  
    }  
  
    private static void PrintAuthorInfo(System.Type t)  
    {  
        System.Console.WriteLine("Author information for {0}", t);  
  
        // Using reflection.  
        System.Attribute[] attrs = System.Attribute.GetCustomAttributes(t);  // Reflection.  
  
        // Displaying output.  
        foreach (System.Attribute attr in attrs)  
        {  
            if (attr is Author)  
            {  
                Author a = (Author)attr;  
                System.Console.WriteLine("   {0}, version {1:f}", a.GetName(), a.version);  
            }  
        }  
    }  
}  
/* Output:  
    Author information for FirstClass  
       P. Ackerman, version 1.00  
    Author information for SecondClass  
    Author information for ThirdClass  
       R. Koch, version 2.00  
       P. Ackerman, version 1.00  
*/  
```  
  
## <a name="see-also"></a>请参阅

- <xref:System.Reflection>
- <xref:System.Attribute>
- [C# 编程指南](../../index.md)
- [检索存储在特性中的信息](../../../../standard/attributes/retrieving-information-stored-in-attributes.md)
- [反射 (C#)](../reflection.md)
- [特性 (C#)](./index.md)
- [创建自定义特性 (C#)](./creating-custom-attributes.md)
