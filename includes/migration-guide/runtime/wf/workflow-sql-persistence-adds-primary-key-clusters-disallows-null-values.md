---
ms.openlocfilehash: cb9305f623044233082286863d2f2d2c7e9d665a
ms.sourcegitcommit: cbacb5d2cebbf044547f6af6e74a9de866800985
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/05/2020
ms.locfileid: "89496258"
---
### <a name="workflow-sql-persistence-adds-primary-key-clusters-and-disallows-null-values-in-some-columns"></a>工作流 SQL 持久性添加主键群集并在某些列中禁止 null 值

#### <a name="details"></a>详细信息

从 .NET Framework 4.7 起，SqlWorkflowInstanceStoreSchema.sql 脚本为 SQL 工作流实例存储 (SWIS) 创建的表格使用群集主键。 因此，标识不支持 <code>null</code> 值。 此更改不影响 SWIS 操作。 进行了更新以支持 SQL Server 事务复制。

#### <a name="suggestion"></a>建议

若要体验此更改，SQL 文件 SqlWorkflowInstanceStoreSchemaUpgrade.sql 必须应用到现有安装。 新数据库安装自动具有此更改。

| 名称    | 值       |
|:--------|:------------|
| 范围   |边缘|
|Version|4.7|
|类型|运行时|

#### <a name="affected-apis"></a>受影响的 API

无法通过 API 分析检测到。

<!--

#### Affected APIs

Not detectable via API analysis.

-->
