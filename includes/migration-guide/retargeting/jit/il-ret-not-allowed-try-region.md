---
ms.openlocfilehash: 4a65e721e5639f12445a9a44f46baa0d26898a88
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/01/2020
ms.locfileid: "85615607"
---
### <a name="il-ret-not-allowed-in-a-try-region"></a>试用区域中不允许 IL ret

#### <a name="details"></a>详细信息

与 JIT64 实时编译器不同，RyuJIT（在 .NET Framework 4.6 中使用）不允许在试用区域中使用 IL ret 指令。 ECMA-335 规范不允许从试用区域返回，并且没有已知的托管编译器会生成此类 IL。 但是，如果由反射发出生成，则 JIT64 编译器执行此类 IL。

#### <a name="suggestion"></a>建议

如果应用正在生成在试用区域中包含 ret 操作码的 IL，则该应用会面向 .NET Framework 4.5，使用旧的 JIT 并避免此中断。 或者，生成的 IL 可更新为在试用区域之后返回。

| “属性”    | “值”       |
|:--------|:------------|
| 范围   | 边缘        |
| Version | 4.6         |
| 类型    | 重定目标 |
