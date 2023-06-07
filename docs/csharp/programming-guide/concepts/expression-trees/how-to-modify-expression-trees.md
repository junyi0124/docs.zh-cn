---
title: 如何修改表达式树 (C#)
description: 了解如何通过创建现有表达式树的副本并进行所需的更改来修改表达式树。
ms.date: 07/20/2015
ms.assetid: 9b0cd8c2-457e-4833-9e36-31e79545f442
ms.openlocfilehash: 01176f489794a0f4ca29d229d29507fdba0fd5a8
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91167686"
---
# <a name="how-to-modify-expression-trees-c"></a>如何修改表达式树 (C#)

本主题演示如何修改表达式树。 表达式树是不可变的，这意味着不能直接对它们进行修改。 若要更改表达式树，必须创建现有表达式树的副本，创建此副本后，进行必要的更改。 可以使用 <xref:System.Linq.Expressions.ExpressionVisitor> 类遍历现有表达式树，以及复制它访问的每个节点。  
  
### <a name="to-modify-an-expression-tree"></a>修改表达式树  
  
1. 创建新的**控制台应用程序**项目。  
  
2. 为 `System.Linq.Expressions` 命名空间的文件添加 `using` 指令。  
  
3. 向项目中添加 `AndAlsoModifier` 类。  
  
    ```csharp  
    public class AndAlsoModifier : ExpressionVisitor  
    {  
        public Expression Modify(Expression expression)  
        {  
            return Visit(expression);  
        }  
  
        protected override Expression VisitBinary(BinaryExpression b)  
        {  
            if (b.NodeType == ExpressionType.AndAlso)  
            {  
                Expression left = this.Visit(b.Left);  
                Expression right = this.Visit(b.Right);  
  
                // Make this binary expression an OrElse operation instead of an AndAlso operation.  
                return Expression.MakeBinary(ExpressionType.OrElse, left, right, b.IsLiftedToNull, b.Method);  
            }  
  
            return base.VisitBinary(b);  
        }  
    }  
    ```  
  
     此类继承 <xref:System.Linq.Expressions.ExpressionVisitor> 类，并且专用于修改表示条件 `AND` 运算的表达式。 它将运算从条件 `AND` 更改为条件 `OR`。 若要执行此操作，此类替代基类型的 <xref:System.Linq.Expressions.ExpressionVisitor.VisitBinary%2A> 方法，因为条件 `AND` 表达式表示为二进制表达式。 在 `VisitBinary` 方法中，如果传递给它的表达式表示条件 `AND` 操作，那么代码将构造一个新的表达式，此表达式包含条件 `OR` 运算符，而不是条件 `AND` 运算符。 如果传递给 `VisitBinary` 的表达式不表示条件 `AND` 运算，那么此方法遵从基类实现。 基类方法构造类似于所传递的表达式树的节点，但是这些节点将子树替换为访问者以递归方式生成的表达式树。  
  
4. 为 `System.Linq.Expressions` 命名空间的文件添加 `using` 指令。  
  
5. 向 Program.cs 文件中的 `Main` 方法添加代码以创建表达式树，并将其传递给将对其进行修改的方法。  
  
    ```csharp  
    Expression<Func<string, bool>> expr = name => name.Length > 10 && name.StartsWith("G");  
    Console.WriteLine(expr);  
  
    AndAlsoModifier treeModifier = new AndAlsoModifier();  
    Expression modifiedExpr = treeModifier.Modify((Expression) expr);  
  
    Console.WriteLine(modifiedExpr);  
  
    /*  This code produces the following output:  
  
        name => ((name.Length > 10) && name.StartsWith("G"))  
        name => ((name.Length > 10) || name.StartsWith("G"))  
    */  
    ```  
  
     此代码创建的表达式中包含条件 `AND` 运算。 然后，此代码创建 `AndAlsoModifier` 类的实例，并将表达式传递给此类的 `Modify` 方法。 输出原始以及修改后的表达式树以显示更改。  
  
6. 编译并运行该应用程序。  
  
## <a name="see-also"></a>请参阅

- [如何执行表达式树 (C#)](./how-to-execute-expression-trees.md)
- [表达式树 (C#)](./index.md)
