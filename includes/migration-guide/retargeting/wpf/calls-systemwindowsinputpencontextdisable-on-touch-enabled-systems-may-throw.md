---
ms.openlocfilehash: a44458484ea09c566defeabc9b604bd55d994e93
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/01/2020
ms.locfileid: "85617521"
---
### <a name="calls-to-systemwindowsinputpencontextdisable-on-touch-enabled-systems-may-throw-an-argumentexception"></a>在启用触摸的系统上调用 System.Windows.Input.PenContext.Disable 可能会引发 ArgumentException

#### <a name="details"></a>详细信息

在某些情况下，在启用触摸的系统上调用内部 **System.Windows.Intput.PenContext.Disable** 方法可能会因为重新进入而引发未处理的 `T:System.ArgumentException`。

#### <a name="suggestion"></a>建议

此问题已在 .NET Framework 4.7 中得到解决。 若要避免此异常，升级到 .NET Framework 4.7 及以上版本。

| “属性”    | “值”       |
|:--------|:------------|
| 范围   | 边缘        |
| Version | 4.6.1       |
| 类型    | 重定目标 |
