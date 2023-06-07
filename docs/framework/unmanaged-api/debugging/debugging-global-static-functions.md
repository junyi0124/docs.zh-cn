---
title: 调试全局静态函数
ms.date: 03/30/2017
helpviewer_keywords:
- global static functions [.NET Framework debugging]
- debugging global static functions [.NET Framework]
- unmanaged global static functions [.NET Framework], debugging
ms.assetid: efc64414-77c3-48d0-881a-8594ed416aad
ms.openlocfilehash: 04906322e311b580abddeca7744cf3e75d471e05
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95722981"
---
# <a name="debugging-global-static-functions"></a>调试全局静态函数

本节介绍了调试 API 使用的非托管全局静态函数。  
  
## <a name="in-this-section"></a>本节内容  

 [_EFN_GetManagedExcepStack 函数](efn-getmanagedexcepstack-function.md)  
 给定托管的异常对象地址后，将返回其中包含的堆栈跟踪的字符串版本。  
  
 [_EFN_GetManagedObjectFieldInfo 函数](efn-getmanagedobjectfieldinfo-function.md)  
 使用提供的对象指针和字段名，获取从对象的开头到字段和字段值的偏移量。  
  
 [_EFN_GetManagedObjectName 函数](efn-getmanagedobjectname-function.md)  
 通过使用提供的托管对象指针获取类型名称。  
  
 [_EFN_StackTrace 函数](efn-stacktrace-function.md)  
 提供托管堆栈跟踪的文本表示形式以及 `CONTEXT` 记录的数组，其中每项对应非托管代码和托管代码之间的每个转换。  
  
 [CLRDataCreateInstance 函数](clrdatacreateinstance-function.md)  
 由公共语言运行时 (CLR) 数据访问服务调用，用于创建指定目标进程的指定接口对象。  
  
 [PFN_CLRDataCreateInstance 函数指针](pfn-clrdatacreateinstance-function-pointer.md)  
 指向 CLR 数据访问服务调用来创建指定目标进程的指定接口对象的函数。  
  
## <a name="related-sections"></a>相关章节  

 [调试 Coclass](debugging-coclasses.md)  
  
 [调试接口](debugging-interfaces.md)  
  
 [调试枚举](debugging-enumerations.md)  
  
 [调试结构](debugging-structures.md)
