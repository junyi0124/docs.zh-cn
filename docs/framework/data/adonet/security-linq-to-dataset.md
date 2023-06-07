---
title: 安全性 (LINQ to DataSet)
ms.date: 03/30/2017
ms.assetid: 6116b2b8-75f4-4d8b-aea6-c13e55cda50b
ms.openlocfilehash: 03a5f562cb6dda8579d7cbdca56a60c6da34a576
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91186921"
---
# <a name="security-linq-to-dataset"></a>安全性 (LINQ to DataSet)

本主题讨论 LINQ to DataSet 中的安全问题。  
  
## <a name="passing-a-query-to-an-untrusted-component"></a>将查询传递到不受信任的组件  

 可以在程序的一个点中表述 LINQ to DataSet 查询，并在另一个点中执行。 在表述查询的点，查询可引用在该点可见的任何元素，例如调用方法所属的类的私有成员，或者表示局部变量/参数的符号。 在执行时，查询能有效访问那些查询在表述中引用的成员，即使其中调用代码没有可见性。 执行查询的代码没有任意添加的可见性，因为它无法选择访问内容。 它能严格访问查询所访问的内容，并且仅通过查询本身访问。  
  
 这意味着将查询的引用传递到其他代码段即表示信任接收该查询的组件，组件可访问查询引用的所有公共成员和私有成员。 通常，不应将 LINQ to DataSet 查询传递到不受信任的组件，除非查询已经过仔细构造，使它不会公开应保留为私有的信息。  
  
## <a name="external-input"></a>外部输入  

 应用程序常常采用外部输入（来自用户或其他外部代理），并根据该输入执行操作。  在 LINQ to DataSet 的情况下，应用程序可能会根据外部输入或使用查询中的外部输入以特定方式构造查询。 LINQ to DataSet 查询在接受文本的任何位置都接受参数。 应用程序开发人员应使用参数化查询，而不是将来自外部代理的文本直接注入查询。  
  
 任何直接或间接从用户或外部代理派生的输入都可能包含利用目标语言的语法来执行未授权操作的内容。 这称为 SQL 注入式攻击，是以目标语言为 Transact-SQL 的攻击模式命名的。 恶意用户利用这种直接注入到查询的用户输入删除数据库表、产生拒绝服务或者更改所执行操作的性质。 尽管可以在 LINQ to DataSet 中进行查询撰写，但它是通过对象模型 API 执行的。 LINQ to DataSet 查询不是通过使用字符串操作或串联来撰写的，因为它们在 Transact-sql 中，并不易遭受传统意义上的 SQL 注入攻击。  
  
## <a name="see-also"></a>请参阅

- [编程指南](programming-guide-linq-to-dataset.md)
