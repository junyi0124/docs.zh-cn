---
title: 调试结构
ms.date: 03/30/2017
helpviewer_keywords:
- unmanaged structures [.NET Framework], debugging
- debugging structures [.NET Framework]
- structures [.NET Framework debugging]
ms.assetid: 173ba2c2-ab34-49ae-b6a8-e5c49882bf05
ms.openlocfilehash: bf84f8ddb1e86da3b9d0e4326584e61304640558
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95676265"
---
# <a name="debugging-structures"></a>调试结构

本节描述调试 API 使用的非托管结构。

## <a name="in-this-section"></a>本节内容

 [CLRDATA_ADDRESS_RANGE 结构](clrdata-address-range-structure.md) 定义地址范围。

 [CLRDATA_IL_ADDRESS_MAP 结构](clrdata-il-address-map-structure.md) 定义用于寻址映射的 IL

 [CLR_DEBUGGING_VERSION 结构](clr-debugging-version-structure.md) 出于调试目的，定义公共语言运行时 (CLR) 的产品版本。

 [CodeChunkInfo 结构](codechunkinfo-structure.md) 表示内存中的单个代码块。

 [COR_ACTIVE_FUNCTION](cor-active-function-structure.md) 包含有关线程帧中当前处于活动状态的函数的信息。

 [COR_ARRAY_LAYOUT 结构](cor-array-layout-structure.md) 提供有关内存中数组对象的布局的信息。

 [COR_DEBUG_IL_TO_NATIVE_MAP](cor-debug-il-to-native-map-structure.md) 包含用于将 Microsoft 中间语言 (MSIL) 代码映射到本机代码的偏移量。

 [COR_DEBUG_STEP_RANGE](cor-debug-step-range-structure.md) 包含一系列代码的偏移信息。

 [COR_FIELD 结构](cor-field-structure.md) 提供有关对象中的字段的信息。

 [COR_GC_REFERENCE 结构](cor-gc-reference-structure.md) 包含有关要进行垃圾回收的对象的信息。

 [COR_HEAPINFO 结构](cor-heapinfo-structure.md) 提供有关垃圾回收堆的常规信息，包括是否可枚举。

 [COR_HEAPOBJECT 结构](cor-heapobject-structure.md) 提供有关托管堆上的对象的信息。

 [COR_IL_MAP](cor-il-map-structure.md) 指定函数的相对偏移量中的更改。

 [COR_SEGMENT 结构](cor-segment-structure.md) 包含有关托管堆中的内存区域的信息。

 [COR_TYPEID 结构](cor-typeid-structure.md) 包含一个类型标识符。

 [COR_TYPE_LAYOUT 结构](cor-type-layout-structure.md) 提供有关内存中对象的布局的信息。

 [COR_VERSION](cor-version-structure.md) 存储公共语言运行时的由四个部分组成的标准版本号。

 [CorDebugBlockingObject 结构](cordebugblockingobject-structure.md) 定义一个对象，该对象阻止线程，以及线程被阻止的原因。

 [CorDebugEHClause 结构](cordebugehclause-structure.md) 表示一种异常处理 (EH) 子句，适用于一段给定的中间语言 (IL) 。

 [CorDebugExceptionObjectStackFrame 结构](cordebugexceptionobjectstackframe-structure.md) 表示异常对象中的堆栈帧信息。

 [CorDebugGuidToTypeMapping 结构](cordebugguidtotypemapping-structure.md) 将 Windows 运行时 GUID 映射到其对应的 [ICorDebugType](icordebugtype-interface.md) 对象。

 [DacpGetModuleAddress 结构](dacpgetmoduleaddress-structure.md) 定义模块地址请求的容器。

 [DacpMethodDescData 结构](dacpmethoddescdata-structure.md) 定义方法的运行时信息的传输缓冲区。

 [DacpModuleData 结构](dacpmoduledata-structure.md) 定义模块的运行时信息的传输缓冲区。

 [DacpReJitData 结构](dacprejitdata-structure.md) 定义有关给定探查器检测到的方法的基本信息。

 [StackTrace_SimpleContext 结构](stacktrace-simplecontext-structure.md) 提供了一个简单的上下文，可以使用它代替完整的 `CONTEXT` 结构。

## <a name="related-sections"></a>相关章节

 [调试 Coclass](debugging-coclasses.md)

 [调试接口](debugging-interfaces.md)

 [调试全局静态函数](debugging-global-static-functions.md)

 [调试枚举](debugging-enumerations.md)

 [调试](index.md)
