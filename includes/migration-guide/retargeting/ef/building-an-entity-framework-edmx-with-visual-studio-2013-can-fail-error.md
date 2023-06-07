---
ms.openlocfilehash: c5099f610569f7788bb829a2153006d20d8a4ea2
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/01/2020
ms.locfileid: "85615609"
---
### <a name="building-an-entity-framework-edmx-with-visual-studio-2013-can-fail-with-error-msb4062-if-using-the-entitydeploysplit-or-entityclean-tasks"></a>如果使用 EntityDeploySplit 或 EntityClean 任务，使用 Visual Studio 2013 生成 Entity Framework edmx 会失败，错误编码为 MSB4062

#### <a name="details"></a>详细信息

MSBuild 12.0 工具（包括在 Visual Studio 2013 中）更改了 MSBuild 文件位置，使得旧 Entity Framework 目标文件变为无效文件。 结果是 `EntityDeploySplit` 和 `EntityClean` 任务失败，因为它们无法找到 `Microsoft.Data.Entity.Build.Tasks.dll`。 请注意，此中断是由于工具集 (MSBuild/VS) 更改而引起的，并不是因为 .NET Framework 更改。 它仅在升级开发人员工具时才会发生，仅升级 .NET Framework 不会发生。

#### <a name="suggestion"></a>建议

从 .NET Framework 4.6 开始，Entity Framework 目标文件已修复，可与新 MSBuild 布局一起使用。 升级到该版本的 Framework 将解决此问题。 或者，可使用[此解决方法](https://stackoverflow.com/a/24249247/131944)直接修补目标文件。

| “属性”    | “值”       |
|:--------|:------------|
| 范围   | 主要       |
| Version | 4.5.1       |
| 类型    | 重定目标 |
