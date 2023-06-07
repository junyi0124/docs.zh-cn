---
title: 使用用户筛选的异常处理程序
ms.date: 03/30/2017
helpviewer_keywords:
- user-filtered exceptions
- exceptions, user-filtered
ms.assetid: aa80d155-060d-41b4-a636-1ceb424afee8
ms.openlocfilehash: 4b85c2be0ed61af38eac1b65fb70f0ef1ea4405e
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95669037"
---
# <a name="using-user-filtered-exception-handlers"></a>使用用户筛选的异常处理程序

目前，Visual Basic 支持用户筛选的异常。 用户筛选的异常处理程序基于定义的异常需求来捕获和处理异常。 这些处理程序使用含有关键字 **When** 的 **Catch** 语句。  
  
 特定异常对象对应于多个错误时，此方法非常有用。 在这种情况下，该对象通常包含与错误关联的特定错误代码的属性。 可以在表达式中使用此错误代码属性，以便只选择要在 **Catch** 子句中处理的特定错误。  
  
 下面的 Visual Basic 示例阐释 **Catch/When** 语句。  
  
```vb
Try  
    'Try statements.  
    Catch When Err = VBErr_ClassLoadException
    'Catch statements.
End Try  
```  
  
 用户筛选的子句的表达式不受任何限制。 如果在执行用户筛选的表达式期间发生异常，则将放弃该异常，并视筛选表达式的值为 false。 在这种情况下，公共语言运行时继续搜索当前异常的处理程序。  
  
## <a name="combining-the-specific-exception-and-the-user-filtered-clauses"></a>结合特定异常和用户筛选的子句  

 Catch 语句可以同时包含特定异常和用户筛选的子句。 运行时首先测试特定异常。 如果特定异常成功，运行时会执行用户筛选。 普通筛选可包含对类筛选器中声明的变量的引用。 请注意，两个筛选子句的顺序不能颠倒。  
  
 下面的 Visual Basic 示例介绍 **Catch** 语句中的特定异常 `ClassLoadException` 以及使用 **When** 关键字的用户筛选的子句。  
  
```vb
Try  
    'Try statements.
    Catch cle As ClassLoadException When cle.IsRecoverable()  
    'Catch statements.
End Try  
```  

## <a name="see-also"></a>另请参阅

- [异常](index.md)
