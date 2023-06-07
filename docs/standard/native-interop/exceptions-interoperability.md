---
title: 异常互操作性
ms.date: 01/16/2020
helpviewer_keywords:
- unmanaged code, exceptions
- exceptions, unmanaged code
- interop, exceptions
- exceptions, interop
ms.openlocfilehash: db7c9943c298607aa91a4bd08ddc12bbafc872be
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94830618"
---
# <a name="working-with-interop-exceptions-in-unmanaged-code"></a>在非托管代码中处理互操作异常

仅在 Windows 平台上支持非托管代码异常互操作。 非 Windows 平台上会出现可移植性问题。 由于 Unix ABI 没有异常处理的定义，因此托管代码无法知道异常机制的内部工作方式。 因此，异常最终可能导致不可预知的行为和故障。

## <a name="setjmplongjmp-behaviors"></a>Setjmp/Longjmp 行为

不支持与 `setjmp` 和 `longjmp` C 函数的互操作。 无法使用 `longjmp` 跳过托管帧。

有关详细信息，请参阅 [longjmp 文档](/cpp/c-runtime-library/reference/longjmp)。

## <a name="see-also"></a>请参阅

- [异常](index.md)
- [与本机库的互操作](https://www.mono-project.com/docs/advanced/pinvoke/#runtime-exception-propagation)
