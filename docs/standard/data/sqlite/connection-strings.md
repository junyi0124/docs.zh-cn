---
title: 连接字符串
ms.date: 12/13/2019
description: 连接字符串的支持关键字和值。
ms.openlocfilehash: 3c50b31689abf6d47aa8f83a6f6f755bcfec0ea3
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90555388"
---
# <a name="connection-strings"></a>连接字符串

用于指定如何连接到数据库的连接字符串。 Microsoft.Data.Sqlite 中的连接字符串遵循标准 [ADO.NET 语法](../../../framework/data/adonet/connection-strings.md)，形成以分号分隔的关键字和值的列表。

## <a name="keywords"></a>关键字

以下连接字符串关键字可以与 Microsoft.Data.Sqlite 一起使用：

### <a name="data-source"></a>数据源

数据库文件的路径。 DataSource  （不带空格）和 Filename  是此关键字的别名。

SQLite 处理相对于当前工作目录的路径。 还可以指定绝对路径。

如果为空  ，则 SQLite 将创建一个临时磁盘数据库，该数据库会在连接关闭时删除。

如果为 `:memory:`，则使用内存数据库。 有关详细信息，请参阅[内存数据库](in-memory-databases.md)。

以 `|DataDirectory|` 替换字符串开头的路径被视作与相对路径相同。 如果设置，路径是相对于 DataDirectory 应用程序域属性值进行设置的。

此关键字还支持 [URI 文件名](https://www.sqlite.org/uri.html)。

### <a name="mode"></a>模式

连接模式。

| “值”           | 描述                                                                                        |
| --------------- | -------------------------------------------------------------------------------------------------- |
| ReadWriteCreate | 打开数据库以进行读取和写入，如果数据库不存在，则创建数据库。 这是默认设置。 |
| ReadWrite       | 打开数据库以进行读取和写入。                                                        |
| ReadOnly        | 以只读模式打开数据库。                                                              |
| 内存          | 打开内存数据库。                                                                       |

### <a name="cache"></a>缓存

连接使用的缓存模式。

| “值”   | 描述                                                                                    |
| ------- | ---------------------------------------------------------------------------------------------- |
| 默认 | 使用基础 SQLite 库的默认模式。 这是默认设置。                   |
| Private | 每个连接使用一个专用缓存。                                                          |
| Shared  | 连接共享一个缓存。 此模式可更改事务和表锁定的行为。 |

### <a name="password"></a>Password

加密密钥。 指定后，打开连接后会立即发送 `PRAGMA key`。

> [!WARNING]
> 当本机 SQLite 库不支持加密时，密码不起作用。

### <a name="foreign-keys"></a>Foreign Keys

一个指示是否启用外键约束的值。

| “值”   | 描述
| ------- | --- |
| True    | 打开连接后会立即发送 `PRAGMA foreign_keys = 1`。
| False   | 打开连接后会立即发送 `PRAGMA foreign_keys = 0`。
| （空） | 不发送 `PRAGMA foreign_keys`。 这是默认设置。 |

如果在 e_sqlite3 中，SQLITE_DEFAULT_FOREIGN_KEYS 用于编译本机 SQLite 库，那么就不需要启用外键。

### <a name="recursive-triggers"></a>递归触发器

一个指示是否启用递归触发器的值。

| “值” | 描述                                                                 |
| ----- | --------------------------------------------------------------------------- |
| True  | 打开连接后会立即发送 `PRAGMA recursive_triggers`。 |
| False | 不发送 `PRAGMA recursive_triggers`。 这是默认设置。              |

## <a name="connection-string-builder"></a>连接字符串生成器

可以使用 <xref:Microsoft.Data.Sqlite.SqliteConnectionStringBuilder> 作为创建连接字符串的强类型方式。 它还可以用于防御连接字符串注入攻击。

[!code-csharp[](../../../../samples/snippets/standard/data/sqlite/EncryptionSample/Program.cs?name=snippet_ConnectionStringBuilder)]

## <a name="examples"></a>示例

### <a name="basic"></a>Basic

一个包含共享缓存的基本连接字符串，用于提高并发。

```connectionstring
Data Source=Application.db;Cache=Shared
```

### <a name="encrypted"></a>加密

一个加密的数据库。

```connectionstring
Data Source=Encrypted.db;Password=MyEncryptionKey
```

### <a name="read-only"></a>只读

一个应用无法修改的只读数据库。

```connectionstring
Data Source=Reference.db;Mode=ReadOnly
```

### <a name="in-memory"></a>内存中

一个专用的内存数据库。

```connectionstring
Data Source=:memory:
```

### <a name="sharable-in-memory"></a>可共享的内存

一个标识有“Sharable”  名称的可共享内存数据库。

```connectionstring
Data Source=Sharable;Mode=Memory;Cache=Shared
```

## <a name="see-also"></a>请参阅

* [ADO.NET 中的连接字符串](../../../framework/data/adonet/connection-strings.md)
* [内存数据库](in-memory-databases.md)
* [事务](transactions.md)
