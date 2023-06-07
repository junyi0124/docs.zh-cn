---
title: 可以为 null 的结构化类型 (Entity SQL)
ms.date: 03/30/2017
ms.assetid: ae006fa9-997e-45bb-8a04-a7f62026171e
ms.openlocfilehash: fc2230401ef98c005ab52a845de37482c0dcf698
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91202258"
---
# <a name="nullable-structured-types-entity-sql"></a>可以为 null 的结构化类型 (Entity SQL)

结构化类型的 `null` 实例是不存在的实例。 这不同于其中所有属性都具有 `null` 值的现有实例。  
  
 本主题介绍可以为 Null 的结构化类型，包括哪些类型可以为 null 以及哪些代码模式会产生可以为 Null 的结构化类型的 `null` 实例。  
  
## <a name="kinds-of-nullable-structured-types"></a>可以为 Null 的结构化类型的种类  

 有三种可以为 Null 的结构化类型：  
  
- 行类型。  
  
- 复杂类型。  
  
- 实体类型。  
  
## <a name="code-patterns-that-produce-null-instances-of-structured-types"></a>能够产生结构化类型的 null 实例的代码模式  

 以下方案会产生 `null` 实例：  
  
- 将 `null` 说明为结构化类型：  
  
    ```sql  
    TREAT (NULL AS StructuredType)  
    ```  
  
- 将基类型向上转换为派生类型：  
  
    ```sql  
    TREAT (BaseType AS DerivedType)  
    ```  
  
- 对 false 条件进行外部联接：  
  
    ```sql  
    Collection1 LEFT OUTER JOIN Collection2  
    ON FalseCondition  
    ```  
  
     --或  
  
    ```sql  
    Collection1 RIGHT OUTER JOIN Collection2  
    ON FalseCondition  
    ```  
  
     --或  
  
    ```sql  
    Collection1 FULL OUTER JOIN Collection2  
    ON FalseCondition  
    ```  
  
- 取消 `null` 引用的引用：  
  
    ```sql  
    DEREF(NullRef)  
    ```  
  
- 从空集合获取 ANYELEMENT：  
  
    ```sql  
    ANYELEMENT(EmptyCollection)  
    ```  
  
- 检查结构化类型实例是否为 `null`：  
  
    ```csharp  
    ...  
    for (int i = 0; i < reader.FieldCount; i++)  
    {  
        if (reader.IsDBNull(i))  
        {  
            Console.WriteLine("[NULL]");  
        }  
        else  
        {  
            Console.WriteLine(reader.GetValue(i).ToString());  
        }  
    }  
    ```  
  
## <a name="see-also"></a>请参阅

- [Entity SQL 概述](entity-sql-overview.md)
