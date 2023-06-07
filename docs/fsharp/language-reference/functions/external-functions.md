---
title: 外部函数
description: 了解如何在F#本机代码中调用函数的语言支持。
ms.date: 05/16/2016
ms.openlocfilehash: 3c8edaba25e07b6ca2c44a58c4b55dc98a13b4fc
ms.sourcegitcommit: 68653db98c5ea7744fd438710248935f70020dfb
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/22/2019
ms.locfileid: "69968731"
---
# <a name="external-functions"></a>外部函数

本主题介绍F#在本机代码中调用函数的语言支持。

## <a name="syntax"></a>语法

```fsharp
[<DllImport( arguments )>]
extern declaration
```

## <a name="remarks"></a>备注

在前面的语法中,*参数*表示提供给`System.Runtime.InteropServices.DllImportAttribute`特性的参数。 第一个参数是一个字符串, 表示包含此函数的 DLL 的名称, 但不包含 .dll 扩展名。 可以为`System.Runtime.InteropServices.DllImportAttribute`类的任何公共属性 (如调用约定) 提供其他参数。

假设你有一个包含C++以下导出函数的本机 DLL。

```cpp
#include <stdio.h>
extern "C" void __declspec(dllexport) HelloWorld()
{
    printf("Hello world, invoked by F#!\n");
}
```

您可以通过使用以下代码F#从调用此函数。

```fsharp
open System.Runtime.InteropServices

module InteropWithNative =
    [<DllImport(@"C:\bin\nativedll", CallingConvention = CallingConvention.Cdecl)>]
    extern void HelloWorld()

InteropWithNative.HelloWorld()
```

与本机代码的互操作性称为*平台调用*, 是 CLR 的一项功能。 有关详细信息，请参阅[与非托管代码交互操作](../../../framework/interop/index.md)。 此部分中的信息适用于F#。

## <a name="see-also"></a>请参阅

- [函数](index.md)
