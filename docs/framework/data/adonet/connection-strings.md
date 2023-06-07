---
title: 连接字符串
description: 了解 ADO.NET 中的连接字符串，其中包含作为参数从数据提供程序传递到数据源的初始化信息。
ms.date: 10/10/2018
ms.assetid: 745c5f95-2f02-4674-b378-6d51a7ec2490
ms.openlocfilehash: baa6599a532746895cbb3920f2c623dd4c2be864
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/02/2020
ms.locfileid: "84287020"
---
# <a name="connection-strings-in-adonet"></a>在 ADO.NET 中的连接字符串

连接字符串包含作为参数从数据提供程序传递到数据源的初始化信息。 数据访问接口接收连接字符串作为属性的值 <xref:System.Data.Common.DbConnection.ConnectionString?displayProperty=nameWithType> 。 提供程序分析连接字符串，并确保语法正确，并且支持关键字。 然后， <xref:System.Data.Common.DbConnection.Open?displayProperty=nameWithType> 方法将已分析的连接参数传递到数据源。 数据源执行进一步的验证，并建立连接。

## <a name="connection-string-syntax"></a>连接字符串语法

连接字符串是以分号分隔的键/值参数对列表：

```csharp
keyword1=value; keyword2=value;
```

关键字不区分大小写。 但是，值可能区分大小写，具体取决于数据源。 关键字和值可以包含[空格字符](https://en.wikipedia.org/wiki/Whitespace_character#Unicode)。 关键字和无引号值中会忽略前导空格和尾随空格。

如果值包含分号、 [Unicode 控制字符](https://en.wikipedia.org/wiki/Unicode_control_characters)或前导或尾随空格，则必须用单引号或双引号将其引起来。 例如：

```csharp
Keyword=" whitespace  ";
Keyword='special;character';
```

封闭字符不能出现在它所包含的值中。 因此，包含单引号的值只能用双引号引起来，反之亦然：

```csharp
Keyword='double"quotation;mark';
Keyword="single'quotation;mark";
```

还可以通过将其中两个字符一起使用来转义封闭字符：

```csharp
Keyword="double""quotation";
Keyword='single''quotation';
```

引号本身以及等号不需要进行转义，因此以下连接字符串有效：

```csharp
Keyword=no "escaping" 'required';
Keyword=a=b=c
```

由于每个值都是在下一个分号或字符串末尾之前读取的，因此后一示例中的值为 `a=b=c` ，而最后的分号是可选的。

所有连接字符串共享上述相同的基本语法。 但所识别的关键字集取决于提供程序，但在以前的 Api （例如*ODBC*）中已经演变。 用于*SQL Server* （）的 *.NET Framework*数据提供程序 `SqlClient` 支持来自较低版本 api 的多个关键字，但通常更灵活，并接受多个常用连接字符串关键字的同义词。

键入错误会导致错误。 例如， `Integrated Security=true` 是有效的，但 `IntegratedSecurity=true` 会导致错误。

在运行时从未经验证用户输入手动构造的连接字符串容易受到字符串注入式攻击，并危害数据源的安全性。 为了解决这些问题， *ADO.NET* 2.0 引入了每个 *.NET Framework*数据提供程序的[连接字符串生成器](connection-string-builders.md)。 这些连接字符串生成器将参数作为强类型属性公开，并在将连接字符串发送到数据源之前验证连接字符串。

## <a name="in-this-section"></a>本节内容

[连接字符串生成器](connection-string-builders.md)\
演示如何使用 `ConnectionStringBuilder` 类在运行时构造有效的连接字符串。

[连接字符串和配置文件](connection-strings-and-configuration-files.md)\
演示如何在配置文件中存储和检索连接字符串。

[连接字符串语法](connection-string-syntax.md)\
描述如何为 `SqlClient`、`OracleClient`、`OleDb` 和 `Odbc` 配置提供程序专用的连接字符串。

[保护连接信息](protecting-connection-information.md)\
演示保护用于连接到数据源的信息的各项技术。

## <a name="see-also"></a>另请参阅

- [连接到数据源](/cpp/data/odbc/connecting-to-a-data-source)
- [ADO.NET 概述](ado-net-overview.md)
