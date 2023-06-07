---
ms.openlocfilehash: 4d410811095786b33580d25f6c6eab3ac2f27148
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/01/2020
ms.locfileid: "85616027"
---
### <a name="resolveassemblyreference-task-now-warns-of-dependencies-with-the-wrong-architecture"></a>ResolveAssemblyReference 任务现在将对体系结构错误的依赖项发出警告

#### <a name="details"></a>详细信息

此任务发出警告 MSB3270，它表示引用或任何依赖项不匹配应用程序的体系结构。 例如，如果使用 `AnyCPU` 选项编译的应用程序包括 x86 引用，则会发生此情况。 这样的情况可能导致运行时应用失败（在本例中为，如果应用部署为 x64 过程）。

#### <a name="suggestion"></a>建议

有两方面的影响：

- 重新编译将生成警告，当应用在以前版本的 MSBuild 下编译时，此类警告未显示。 但是，由于该警告可标识运行时失败可能的来源，所以应该调查并处理它。
- 如果警告被视为错误，则应用将无法进行编译。

| “属性”    | “值”       |
|:--------|:------------|
| 范围   | 次要       |
| Version | 4.5.1       |
| 类型    | 重定目标 |
