---
title: 操作指南（C# 指南）
description: 快速提示及重点短代码示例集合
ms.date: 12/20/2017
ms.openlocfilehash: 26d3931ff3b4ecfcc052c3ace25a09801f84c505
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90537400"
---
# <a name="how-to-c"></a>操作指南 (C#)

在《C# 指南》中的“操作指南”部分，你可快速了解常见问题的答案。 在某些情况下，可能会在多个部分列出相关文章。 我们希望用户可从多个搜索路径找到操作指南。

## <a name="general-c-concepts"></a>C# 一般概念

此处介绍了 C# 开发者在实践中经常会用到的几个提示和技巧：

- [使用对象初始值设定项初始化对象](../programming-guide/classes-and-structs/how-to-initialize-objects-by-using-an-object-initializer.md)。
- [了解向方法传递结构和传递类的区别](../programming-guide/classes-and-structs/how-to-know-the-difference-passing-a-struct-and-passing-a-class-to-a-method.md)。
- [使用运算符重载](../language-reference/operators/operator-overloading.md)。
- [实现和调用自定义扩展方法](../programming-guide/classes-and-structs/how-to-implement-and-call-a-custom-extension-method.md)。
- 即使是 C# 程序员，也可能需要[使用 Visual Basic `My` 命名空间](../programming-guide/namespaces/how-to-use-the-my-namespace.md)。
- [使用扩展方法创建新 `enum` 类型方法](../programming-guide/classes-and-structs/how-to-create-a-new-method-for-an-enumeration.md)。

### <a name="class-and-struct-members"></a>类和结构成员

创建类和结构来实现程序。 编写类或结构时常会使用这些方法。

- [声明自动实现的属性](../programming-guide/classes-and-structs/how-to-implement-a-lightweight-class-with-auto-implemented-properties.md)。
- [声明和使用读/写属性](../programming-guide/classes-and-structs/how-to-declare-and-use-read-write-properties.md)。
- [定义常量](../programming-guide/classes-and-structs/how-to-define-constants.md)。
- [替代 `ToString` 方法以提供字符串输出](../programming-guide/classes-and-structs/how-to-override-the-tostring-method.md)。
- [定义抽象属性](../programming-guide/classes-and-structs/how-to-define-abstract-properties.md)。
- [使用 XML 文档功能记录代码](../programming-guide/xmldoc/how-to-use-the-xml-documentation-features.md)。
- [显式实现接口成员](../programming-guide/interfaces/how-to-explicitly-implement-interface-members.md)，使公共接口保持简洁。
- [显式实现两个接口的成员](../programming-guide/interfaces/how-to-explicitly-implement-members-of-two-interfaces.md)。

### <a name="working-with-collections"></a>使用集合

这些文章有助于了解如何使用数据集合。

- [使用集合初始值设定项初始化字典](../programming-guide/classes-and-structs/how-to-initialize-a-dictionary-with-a-collection-initializer.md)。

## <a name="working-with-strings"></a>处理字符串

字符串是用于显示或操作文本的基本数据类型。 这些文章介绍了字符串的常见处理方法。

- [比较字符串](compare-strings.md)。
- [修改字符串内容](modify-string-contents.md)。
- [确定字符串是否表示数字](../programming-guide/strings/how-to-determine-whether-a-string-represents-a-numeric-value.md)。
- [使用 `String.Split` 分隔字符串](parse-strings-using-split.md)。
- [将多个字符串合并为一个字符串](concatenate-multiple-strings.md)。
- [在字符串中搜索文本](search-strings.md)。

## <a name="convert-between-types"></a>在类型间转换

你可能需要将对象转换为其他类型。

- [确定字符串是否表示数字](../programming-guide/strings/how-to-determine-whether-a-string-represents-a-numeric-value.md)。
- [在表示十六进制数的字符串和数字之间进行转换](../programming-guide/types/how-to-convert-between-hexadecimal-strings-and-numeric-types.md)。
- [将字符串转换为 `DateTime`](../../standard/base-types/parsing-datetime.md)。
- [将字节数组转换为 int](../programming-guide/types/how-to-convert-a-byte-array-to-an-int.md)。
- [将字符串转换为数字](../programming-guide/types/how-to-convert-a-string-to-a-number.md)。
- [使用模式匹配、`as` 和 `is` 运算符安全强制转换为其他类型](safely-cast-using-pattern-matching-is-and-as-operators.md)。
- [定义自定义类型转换](../language-reference/operators/user-defined-conversion-operators.md)。
- [确定类型是否为可为 null 的值类型](../language-reference/builtin-types/nullable-value-types.md#how-to-identify-a-nullable-value-type)。
- [在可为 null 和不可为 null 的值类型之间转换](../language-reference/builtin-types/nullable-value-types.md#conversion-from-a-nullable-value-type-to-an-underlying-type)。

## <a name="equality-and-ordering-comparisons"></a>相等比较和排序比较

可创建类型来定义自己的相等规则，或者定义该类型对象间的自然顺序。

- [基于引用的相等性测试](../programming-guide/statements-expressions-operators/how-to-test-for-reference-equality-identity.md)。
- [为类型定义基于值的相等](../programming-guide/statements-expressions-operators/how-to-define-value-equality-for-a-type.md)。

## <a name="exception-handling"></a>异常处理

.NET 程序通过引发异常报告方法未能成功完成其任务。 通过这些文章可了解如何处理异常。

- [使用 `try` 和 `catch` 处理异常](../programming-guide/exceptions/how-to-handle-an-exception-using-try-catch.md)。
- [使用 `finally` 子句清理资源](../programming-guide/exceptions/how-to-execute-cleanup-code-using-finally.md)。
- [从非 CLS （公共语言规范）异常中恢复](../programming-guide/exceptions/how-to-catch-a-non-cls-exception.md)。

## <a name="delegates-and-events"></a>委托和事件

委托和事件为涉及松散耦合代码块的策略提供了功能。

- [声明、实例化和使用委托](../programming-guide/delegates/how-to-declare-instantiate-and-use-a-delegate.md)。
- [合并多播委托](../programming-guide/delegates/how-to-combine-delegates-multicast-delegates.md)。

事件提供发布或订阅通知的机制。

- [订阅和取消订阅事件](../programming-guide/events/how-to-subscribe-to-and-unsubscribe-from-events.md)。
- [实现接口中声明的事件](../programming-guide/events/how-to-implement-interface-events.md)。
- [代码发布事件时，遵循 .NET 准则](../programming-guide/events/how-to-publish-events-that-conform-to-net-framework-guidelines.md)。
- [从派生类中引发在基类中定义的事件](../programming-guide/events/how-to-raise-base-class-events-in-derived-classes.md)。
- [实现自定义事件访问器](../programming-guide/events/how-to-implement-custom-event-accessors.md)。

## <a name="linq-practices"></a>LINQ 做法

通过 LINQ 可编写代码来查询任何支持 LINQ 查询表达式模式的数据源。 这些文章有助于你理解该模式并使用不同的数据源。

- [查询集合](../programming-guide/concepts/linq/how-to-query-an-arraylist-with-linq.md)。
- [在查询中使用 Lambda 表达式](../programming-guide/statements-expressions-operators/how-to-use-lambda-expressions-in-a-query.md)。
- [在查询表达式中使用 `var`](../programming-guide/classes-and-structs/how-to-use-implicitly-typed-local-variables-and-arrays-in-a-query-expression.md)。
- [从查询返回元素属性的子集](../programming-guide/classes-and-structs/how-to-return-subsets-of-element-properties-in-a-query.md)。
- [编写使用复杂筛选的查询](../../standard/linq/write-queries-complex-filtering.md)。
- [对数据源的元素排序](../../standard/linq/sort-elements.md)。
- [对多个键上的元素排序](../../standard/linq/sort-elements-multiple-keys.md)。
- [控制投影的类型](../../standard/linq/control-type-projection.md)。
- [对某个值在源序列中出现的次数进行计数](../programming-guide/concepts/linq/how-to-count-occurrences-of-a-word-in-a-string-linq.md)。
- [计算中间值](../../standard/linq/calculate-intermediate-values.md)。
- [合并来自多个源的数据](../programming-guide/concepts/linq/how-to-populate-object-collections-from-multiple-sources-linq.md)。
- [查找两个序列之间的差集](../programming-guide/concepts/linq/how-to-find-the-set-difference-between-two-lists-linq.md)。
- [调试空查询结果](../../standard/linq/debug-empty-query-results-sets.md)。
- [向 LINQ 查询添加自定义方法](../programming-guide/concepts/linq/how-to-add-custom-methods-for-linq-queries.md)。

## <a name="multiple-threads-and-async-processing"></a>多线程和异步处理

新式程序常使用异步操作。 这些文章可帮助你了解如何使用这些方法。

- [使用 `System.Threading.Tasks.Task.WhenAll` 提高异步性能](../programming-guide/concepts/async/index.md)。
- [使用 `async` 和 `await` 并行发出多个 Web 请求](../programming-guide/concepts/async/index.md)。
- [使用线程池](../../standard/threading/the-managed-thread-pool.md#using-the-thread-pool)。

## <a name="command-line-args-to-your-program"></a>程序的命令行参数

通常情况下，C# 程序具有命令行参数。 通过这些文章可了解如何访问和处理这些命令行参数。

- [使用 `for` 检索所有命令行参数](../programming-guide/main-and-command-args/how-to-display-command-line-arguments.md)。
