---
title: Oracle 架构集合
ms.date: 03/30/2017
ms.assetid: 89a75de8-dee8-45e2-a97f-254d7e62e7e1
ms.openlocfilehash: cb91a90ae7323283556954caa401646a2063a37e
ms.sourcegitcommit: d2e1dfa7ef2d4e9ffae3d431cf6a4ffd9c8d378f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/07/2019
ms.locfileid: "70783292"
---
# <a name="oracle-schema-collections"></a>Oracle 架构集合

除了通用架构集合之外，Microsoft Oracle .NET Framework 数据提供程序还支持下列特定的架构集合：

- 列

- 索引

- IndexColumns

- 过程

- 序列

- Synonyms

- 表

- 用户

- Views

- 函数

- package

- PackageBodies

- 自变量

- UniqueKeys

- PrimaryKeys

- ForeignKeys

- ForeignKeyColumns

- ProcedureParameters

## <a name="columns"></a>列

|列名|数据类型|描述|
|----------------|--------------|-----------------|
|OWNER|String|表、视图或群集的所有者。|
|TABLE_NAME|String|表、视图或群集的名称。|
|COLUMN_NAME|String|列名称。|
|Id|Decimal|列在创建时的序列号。|
|DATATYPE|String|列的数据类型。|
|LENGTH|Decimal|列的长度（字节数）。|
|PRECISION|Decimal|对于 NUMBER 数据类型为十进制精度；对于 FLOAT 数据类型为二进制精度；对于所有其他数据类型为 null。|
|SCALE|Decimal|数字中小数点右侧的位数。|
|NULLABLE|String|指定列是否允许 NULL。 如果列上存在 NOT NULL 约束，或列属于 PRIMARY KEY，值为 N。|

## <a name="indexes"></a>索引

|列名|数据类型|描述|
|----------------|--------------|-----------------|
|OWNER|String|索引的所有者。|
|INDEX_NAME|String|索引的名称。|
|INDEX_TYPE|String|索引的类型（NORMAL、BITMAP、FUNCTION-BASED NORMAL、FUNCTION-BASED BITMAP 或 DOMAIN）。|
|TABLE_OWNER|String|索引对象的所有者。|
|TABLE_NAME|String|索引对象的名称。|
|TABLE_TYPE|String|索引对象的类型（例如 TABLE、CLUSTER）。|
|UNIQUENESS|String|索引是 UNIQUE 还是 NONUNIQUE。|
|COMPRESSION|String|索引是 ENABLED 还是 DISABLED。|
|PREFIX_LENGTH|Decimal|压缩键的前缀中的列数。|
|TABLESPACE_NAME|String|包含索引的表空间的名称。|
|INI_TRANS|Decimal|初始事务数。|
|MAX_TRANS|Decimal|最大事务数。|
|INITIAL_EXTENT|Decimal|初始范围的大小。|
|NEXT_EXTENT|Decimal|辅助范围的大小。|
|MIN_EXTENTS|Decimal|段中允许的最小范围数。|
|MAX_EXTENTS|Decimal|段中允许的最大范围数。|
|PCT_INCREASE|Decimal|范围大小增加的百分比。|
|PCT_THRESHOLD|Decimal|每个索引条目允许的块空间的阈值百分比。|
|INCLUDE_COLUMN|Decimal|要加入通过索引组织的表的主键（非溢出）索引中的最后一列的列 ID。 此列映射到 *_TAB_COLUMNS 数据字典视图的 COLUMN_ID 列。|
|FREELISTS|Decimal|为此段分配的进程空闲列表数。|
|FREELIST_GROUPS|Decimal|为此段分配的空闲列表组数。|
|PCT_FREE|Decimal|块中可用空间的最小百分比。|
|LOGGING|String|日志信息。|
|BLEVEL|Decimal|B* 树级别：从根块到叶块的索引深度。 如果深度为 0，指示根块到叶块相同。|
|LEAF_BLOCKS|Decimal|索引中的叶块数。|
|DISTINCT_KEYS|Decimal|不同的索引值数。 对于强制使用 UNIQUE 和 PRIMARY KEY 约束的索引，此值与表中的行数相同 (USER_TABLES.NUM_ROWS)。|
|AVG_LEAF_BLOCKS_PER_KEY|Decimal|平均叶块数，索引中的每个不同值舍入到最接近的整数。 对于强制使用 UNIQUE 和 PRIMARY KEY 约束的索引，此值始终为 1。|
|AVG_DATA_BLOCKS_PER_KEY|Decimal|表中通过索引中舍入到最接近整数的不同值指向的平均数据块数。 此统计信息是包含的行中包含索引列的给定值的平均数据块数。|
|CLUSTERING_FACTOR|Decimal|根据索引值指示表中行的排序数量。|
|状态|String|非分区索引是 VALID 还是 UNUSABLE。|
|NUM_ROWS|Decimal|索引中的行数。|
|SAMPLE_SIZE|Decimal|用于分析索引的示例的大小。|
|LAST_ANALYZED|DateTime|最近分析此索引的日期。|
|DEGREE|String|每个实例用于扫描索引的线程数。|
|INSTANCES|String|在其上扫描索引的实例数。|
|PARTITIONED|String|此索引是否已分区（YES &#124; NO）。|
|TEMPORARY|String|索引是否在临时表上。|
|GENERATED|String|索引名称是否为系统生成（Y&#124;N）。|
|SECONDARY|String|索引是否是由 Oracle9i 数据磁带的 ODCIIndexCreate 方法创建的辅助对象（Y&#124;N）。|
|BUFFER_POOL|String|用于索引块的默认缓冲区池的名称。|
|USER_STATS|String|统计信息是否已由用户直接输入。|
|DURATION|String|指示临时表的持续时间：1） SYS $ SESSION：在会话期间保留行，2） SYS $ TRANSACTION：在 COMMIT 之后删除行，3） Null 表示永久表。|
|PCT_DIRECT_ACCESS|Decimal|对于通过索引组织的表上的辅助索引，为具有 VALID 猜测的行的百分比。|
|ITYP_OWNER|String|对于域索引，为索引类型的所有者。|
|ITYP_NAME|String|对于域索引，为索引类型的名称。|
|PARAMETERS|String|对于域索引，为参数字符串。|
|GLOBAL_STATS|String|对于分区索引，指示统计信息通过整体分析索引进行收集 (YES) 还是通过基础索引分区和子分区上的统计信息进行估计 (NO)。|
|DOMIDX_STATUS|String|反映域索引的状态。 NULL：指定的索引不是域索引。 VALID：索引是有效的域索引。 IDXTYP_INVLD：此域索引的索引类型无效。|
|DOMIDX_OPSTATUS|String|反映在域索引上执行的操作的状态：NULL：指定的索引不是域索引。 VALID：执行操作而未发生错误。 FAILED：操作发生错误并失败。|
|FUNCIDX_STATUS|String|指示基于函数的索引的状态：NULL：这不是基于函数的索引，已启用：启用、禁用基于函数的索引：禁用基于函数的索引。|
|JOIN_INDEX|String|指示此索引是否是联接索引。|

## <a name="indexcolumns"></a>IndexColumns

|列名|数据类型|描述|
|----------------|--------------|-----------------|
|INDEX_OWNER|String|索引的所有者。|
|INDEX_NAME|String|索引的名称。|
|TABLE_OWNER|String|表或群集的所有者。|
|TABLE_NAME|String|表或群集的名称。|
|COLUMN_NAME|String|对象类型列的列名或属性。|
|COLUMN_POSITION|Decimal|列或属性在索引中的位置。|
|COLUMN_LENGTH|Decimal|列的索引长度。|
|CHAR_LENGTH|Decimal|列的最大代码点长度。|
|DESCEND|String|列是否按照降序进行排序。|

## <a name="procedures"></a>过程

|列名|数据类型|描述|
|----------------|--------------|-----------------|
|OWNER|String|对象的所有者。|
|OBJECT_NAME|String|对象的名称。|
|SUBOBJECT_NAME|String|子对象（例如分区）的名称。|
|OBJECT_ID|Decimal|对象的字典对象编号。|
|DATA_OBJECT_ID|Decimal|包含对象的段的字典对象编号。|
|LAST_DDL_TIME|DateTime|上次通过 DDL 命令修改对象（包括授予和吊销）的时间戳。|
|TIMESTAMP|String|指定对象（字符数据）的时间戳。|
|状态|String|对象的状态（VALID、INVALID 或 N/A）。|
|TEMPORARY|String|对象是否是临时对象（当前会话只能看到其放入此对象本身的数据）。|
|GENERATED|String|此对象的名称是否由系统生成？ （Y &#124; N）。|
|SECONDARY|String|这是否是由 Oracle9i 数据磁带的 ODCIIndexCreate 方法创建的辅助对象（Y &#124; N）。|
|CREATED|DateTime|创建对象的日期。|

## <a name="sequences"></a>序列

|列名|数据类型|描述|
|----------------|--------------|-----------------|
|SEQUENCE_OWNER|String|序列所有者的名称。|
|SEQUENCE_NAME|String|序列名称。|
|MIN_VALUE|Decimal|序列的最小值。|
|MAX_VALUE|Decimal|序列的最大值。|
|INCREMENT_BY|Decimal|序列递增的值。|
|CYCLE_FLAG|String|在达到限制时序列是否环绕。|
|ORDER_FLAG|String|序列号是否按顺序生成。|
|CACHE_SIZE|Decimal|要缓存的序列号数。|
|LAST_NUMBER|Decimal|写入磁盘的上一个序列号。 如果序列使用缓存，写入磁盘的序列号是上一个放入序列缓存的序列号。 此序列号很可能大于上一个使用的序列号。|

## <a name="synonyms"></a>Synonyms

|列名|数据类型|描述|
|----------------|--------------|-----------------|
|OWNER|String|同义词的所有者。|
|SYNONYM_NAME|String|同义词的名称。|
|TABLE_OWNER|String|通过同义词引用的对象的所有者。|
|TABLE_NAME|String|通过同义词引用的对象的名称。|
|DB_LINK|String|所引用的数据库链接的名称（如果有）。|

## <a name="tables"></a>表

|列名|数据类型|描述|
|----------------|--------------|-----------------|
|OWNER|String|表的所有者。|
|TABLE_NAME|String|表的名称。|
|TYPE|String|表的类型。|

## <a name="users"></a>用户

|列名|数据类型|描述|
|----------------|--------------|-----------------|
|NAME|String|用户的名称。|
|Id|Decimal|用户的 ID 号。|
|CREATEDATE|DateTime|用户的创建日期。|

## <a name="views"></a>Views

|列名|数据类型|描述|
|----------------|--------------|-----------------|
|OWNER|String|视图的所有者。|
|VIEW_NAME|String|视图的名称。|
|TEXT_LENGTH|Decimal|视图文本的长度。|
|TEXT|String|视图文本。|
|TYPE_TEXT_LENGTH|Decimal|类型化视图的类型子句的长度。|
|TYPE_TEXT|String|类型化视图的类型子句。|
|OID_TEXT_LENGTH|Decimal|类型化视图的 WITH OID 子句的长度。|
|OID_TEXT|String|类型化视图的 WITH OID 子句。|
|VIEW_TYPE_OWNER|String|视图类型的所有者（如果视图是类型化视图）。|
|VIEW_TYPE|String|视图类型（如果视图是类型化视图）。|
|SUPERVIEW_NAME|String|超级视图的名称。|

## <a name="functions"></a>函数

|列名|数据类型|描述|
|----------------|--------------|-----------------|
|OWNER|String|对象的所有者。|
|OBJECT_NAME|String|对象的名称。|
|SUBOBJECT_NAME|String|子对象（例如分区）的名称。|
|OBJECT_ID|Decimal|对象的字典对象编号。|
|DATA_OBJECT_ID|Decimal|包含对象的段的字典对象编号。|
|OBJECT_TYPE|String|对象的类型。|
|CREATED|DateTime|创建对象的日期。|
|LAST_DDL_TIME|DateTime|上次通过 DDL 命令修改对象（包括授予和吊销）的时间戳。|
|TIMESTAMP|String|指定对象（字符数据）的时间戳。|
|状态|String|对象的状态（VALID、INVALID 或 N/A）。|
|TEMPORARY|String|对象是否是临时对象（当前会话只能看到其放入此对象本身的数据）。|
|GENERATED|String|此对象的名称是否由系统生成？ （Y &#124; N）。|
|SECONDARY|String|这是否是由 Oracle9i 数据磁带的 ODCIIndexCreate 方法创建的辅助对象（Y &#124; N）。|

## <a name="packages"></a>package

|列名|数据类型|描述|
|----------------|--------------|-----------------|
|OWNER|String|对象的所有者。|
|OBJECT_NAME|String|对象的名称。|
|SUBOBJECT_NAME|String|子对象（例如分区）的名称。|
|OBJECT_ID|Decimal|对象的字典对象编号。|
|DATA_OBJECT_ID|Decimal|包含对象的段的字典对象编号。|
|LAST_DDL_TIME|DateTime|上次通过 DDL 命令修改对象（包括授予和吊销）的时间戳。|
|TIMESTAMP|String|指定对象（字符数据）的时间戳。|
|状态|String|对象的状态（VALID、INVALID 或 N/A）。|
|TEMPORARY|String|对象是否是临时对象（当前会话只能看到其放入此对象本身的数据）。|
|GENERATED|String|此对象的名称是否由系统生成？ （Y &#124; N）。|
|SECONDARY|String|这是否是由 Oracle9i 数据磁带的 ODCIIndexCreate 方法创建的辅助对象（Y &#124; N）。|
|CREATED|DateTime|创建对象的日期。|

## <a name="packagebodies"></a>PackageBodies

|列名|数据类型|描述|
|----------------|--------------|-----------------|
|OWNER|String|对象的所有者。|
|OBJECT_NAME|String|对象的名称。|
|SUBOBJECT_NAME|String|子对象（例如分区）的名称。|
|OBJECT_ID|Decimal|对象的字典对象编号。|
|DATA_OBJECT_ID|Decimal|包含对象的段的字典对象编号。|
|LAST_DDL_TIME|DateTime|上次通过 DDL 命令修改对象（包括授予和吊销）的时间戳。|
|TIMESTAMP|String|指定对象（字符数据）的时间戳。|
|状态|String|对象的状态（VALID、INVALID 或 N/A）。|
|TEMPORARY|String|对象是否是临时对象（当前会话只能看到其放入此对象本身的数据）。|
|GENERATED|String|此对象的名称是否由系统生成？ （Y &#124; N）。|
|SECONDARY|String|这是否是由 Oracle9i 数据磁带的 ODCIIndexCreate 方法创建的辅助对象（Y &#124; N）。|
|CREATED|DateTime|创建对象的日期。|

## <a name="arguments"></a>自变量

|列名|数据类型|描述|
|----------------|--------------|-----------------|
|OWNER|String|对象所有者的名称。|
|PACKAGE_NAME|String|包名称。|
|OBJECT_NAME|String|过程或函数的名称。|
|ARGUMENT_NAME|String|自变量的名称。|
|POSITION|Decimal|在自变量列表中的位置，对于函数返回值为 NULL。|
|SEQUENCE|Decimal|参数序列，包括所有嵌套级别。|
|DEFAULT_VALUE|String|自变量的默认值。|
|DEFAULT_LENGTH|Decimal|自变量的默认值的长度。|
|IN_OUT|String|自变量方向（IN、OUT 或 IN/OUT）。|
|DATA_LENGTH|Decimal|列的长度（字节数）。|
|DATA_PRECISION|Decimal|十进制位 (NUMBER) 或二进制位 (FLOAT) 的长度。|
|DATA_SCALE|Decimal|数字中小数点右侧的位数。|
|DATA_TYPE|String|参数的数据类型。|

## <a name="uniquekeys"></a>UniqueKeys

|列名|数据类型|描述|
|----------------|--------------|-----------------|
|OWNER|String|约束定义的所有者。|
|CONSTRAINT_NAME|String|约束定义的名称。|
|TABLE_NAME|String|与具有约束定义的表（或视图）关联的名称。|
|SEARCH_CONDITION|String|检查约束的搜索条件的文本。|
|R_OWNER|String|在引用约束中引用的表的所有者。|
|R_CONSTRAINT_NAME|String|所引用表的唯一约束定义的名称。|
|DELETE_RULE|String|删除引用约束的规则（CASCADE 或 NO ACTION）。|
|状态|String|约束的强制执行状态（ENABLED 或 DISABLED）。|
|DEFERRABLE|String|约束是否可以推迟。|
|VALIDATED|String|所有数据是否均遵循该约束（VALIDATED 或 NOT VALIDATED）。|
|GENERATED|String|约束名称是由用户还是由系统生成。|
|BAD|String|YES 值指示此约束以不明确的方式指定世纪。 为了避免因为这种不明确造成错误，使用 TO_DATE 函数重写该约束，包含一个四位的年份。|
|RELY|String|启用的约束强制执行还是非强制执行。|
|LAST_CHANGE|DateTime|上次启用或禁用约束的时间。|
|INDEX_OWNER|String|拥有索引的用户的名称。|
|INDEX_NAME|String|索引的名称。|

## <a name="primarykeys"></a>PrimaryKeys

|列名|数据类型|描述|
|----------------|--------------|-----------------|
|OWNER|String|约束定义的所有者。|
|CONSTRAINT_NAME|String|约束定义的名称。|
|TABLE_NAME|String|与具有约束定义的表（或视图）关联的名称。|
|SEARCH_CONDITION|String|检查约束的搜索条件的文本。|
|R_OWNER|String|在引用约束中引用的表的所有者。|
|R_CONSTRAINT_NAME|String|所引用表的唯一约束定义的名称。|
|DELETE_RULE|String|删除引用约束的规则（CASCADE 或 NO ACTION）。|
|状态|String|约束的强制执行状态（ENABLED 或 DISABLED）。|
|DEFERRABLE|String|约束是否可以推迟。|
|VALIDATED|String|所有数据是否均遵循该约束（VALIDATED 或 NOT VALIDATED）。|
|GENERATED|String|约束名称是由用户还是由系统生成。|
|BAD|String|YES 值指示此约束以不明确的方式指定世纪。 为了避免因为这种不明确造成错误，使用 TO_DATE 函数重写该约束，包含一个四位的年份。|
|RELY|String|启用的约束强制执行还是非强制执行。|
|LAST_CHANGE|DateTime|上次启用或禁用约束的时间。|
|INDEX_OWNER|String|拥有索引的用户的名称。|
|INDEX_NAME|String|索引的名称。|

## <a name="foreignkeys"></a>ForeignKeys

|列名|数据类型|描述|
|----------------|--------------|-----------------|
|PRIMARY_KEY_CONSTRAINT_NAME|String|约束定义的名称。|
|PRIMARY_KEY_OWNER|String|约束定义的所有者。|
|PRIMARY_KEY_TABLE_NAME|String|与具有约束定义的表（或视图）关联的名称。|
|FOREIGN_KEY_OWNER|String|约束定义的所有者。|
|FOREIGN_KEY_CONSTRAINT_NAME|String|约束定义的名称。|
|FOREIGN_KEY_TABLE_NAME|String|与具有约束定义的表（或视图）关联的名称。|
|SEARCH_CONDITION|String|检查约束的搜索条件的文本。|
|R_OWNER|String|在引用约束中引用的表的所有者。|
|R_CONSTRAINT_NAME|String|所引用表的唯一约束定义的名称。|
|DELETE_RULE|String|删除引用约束的规则（CASCADE 或 NO ACTION）。|
|状态|String|约束的强制执行状态（ENABLED 或 DISABLED）。|
|VALIDATED|String|所有数据是否均遵循该约束（VALIDATED 或 NOT VALIDATED）。|
|GENERATED|String|约束名称是由用户还是由系统生成。|
|RELY|String|启用的约束强制执行还是非强制执行。|
|LAST_CHANGE|DateTime|上次启用或禁用约束的时间。|
|INDEX_OWNER|String|拥有索引的用户的名称。|
|INDEX_NAME|String|索引的名称。|

## <a name="foreignkeycolumns"></a>ForeignKeyColumns

|列名|数据类型|描述|
|----------------|--------------|-----------------|
|OWNER|String|约束定义的所有者。|
|CONSTRAINT_NAME|String|约束定义的名称。|
|TABLE_NAME|String|具有约束定义的表的名称。|
|COLUMN_NAME|String|约束定义中指定的对象类型列的列或属性的名称。|
|POSITION|Decimal|对象定义中列或属性的原始位置。|

## <a name="procedureparameters"></a>ProcedureParameters

|列名|数据类型|描述|
|----------------|--------------|-----------------|
|OWNER|String|对象的所有者。|
|OBJECT_NAME|String|过程或函数的名称。|
|PACKAGE_NAME|String|过程或函数的名称。|
|OBJECT_ID|Decimal|对象的对象编号。|
|OVERLOAD|String|重载唯一标识符。|
|ARGUMENT_NAME|String|自变量的名称。|
|POSITION|Decimal|在自变量列表中的位置，对于函数返回值为 null。|
|SEQUENCE|Decimal|参数序列，包括所有嵌套级别。|
|DATA_LEVEL|Decimal|复合类型的参数的嵌套深度。|
|DATA_TYPE|String|参数的数据类型。|
|DEFAULT_VALUE|String|自变量的默认值。|
|DEFAULT_LENGTH|Decimal|参数的默认值的长度。|
|IN_OUT|String|自变量方向（IN、OUT 或 IN/OUT）。|
|DATA_LENGTH|Decimal|列的长度（字节数）。|
|DATA_PRECISION|Decimal|十进制位 (NUMBER) 或二进制位 (FLOAT) 的长度。|
|DATA_SCALE|Decimal|数字中小数点右侧的位数。|
|RADIX|Decimal|数字的参数基数。|
|CHARACTER_SET_NAME|String|自变量的字符集名称。|
|TYPE_OWNER|String|参数类型的所有者。|
|TYPE_NAME|String|参数类型的名称。 如果类型是包局部类型（即在包指定中声明），此列将显示包的名称。|
|TYPE_SUBNAME|String|只与包局部类型有关。 显示在 TYPE_NAME 列中标识的包中声明的类型名称。|
|TYPE_LINK|String|只有 TYPE_NAME 列中标识的包是远程包时，才与包局部类型有关。 此列显示用于引用远程包的数据库链接。|
|PLS_TYPE|String|对于数值参数，为参数的 PL/SQL 类型的名称。 否则为 Null。|
|CHAR_LENGTH|Decimal|字符串数据类型的字符限制。|
|CHAR_USED|String|指示字节限制 (B) 或字符限制 (C) 是否正式用于字符串。|

## <a name="see-also"></a>请参阅

- [ADO.NET 概述](ado-net-overview.md)
