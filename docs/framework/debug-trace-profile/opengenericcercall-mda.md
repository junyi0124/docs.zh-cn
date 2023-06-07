---
title: openGenericCERCall MDA
description: 如果在线程中止或应用程序域卸载时 CER 代码未运行，请参阅 openGenericCERCall 托管调试助手。
ms.date: 03/30/2017
helpviewer_keywords:
- MDAs (managed debugging assistants), CER calls
- open generic CER calls
- constrained execution regions
- openGenericCERCall MDA
- CER calls
- managed debugging assistants (MDAs), CER calls
- generics [.NET Framework], open generic CER calls
ms.assetid: da3e4ff3-2e67-4668-9720-fa776c97407e
ms.openlocfilehash: 4df33b0cdf9759edec47f02b3feb671d03284ec8
ms.sourcegitcommit: c23d9666ec75b91741da43ee3d91c317d68c7327
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/01/2020
ms.locfileid: "85803932"
---
# <a name="opengenericcercall-mda"></a>openGenericCERCall MDA

激活 `openGenericCERCall` 托管调试助手，警告根方法中具有通用类型变量的约束执行区域 (CER) 图形正于 JIT 编译或生成本机图像时进行处理，并且至少一个通用类型变量是对象引用类型。

## <a name="symptoms"></a>症状

线程中止或应用程序域卸载时 CER 代码不运行。

## <a name="cause"></a>原因

在 JIT 编译时，包含对象引用类型的实例只是代表性的，因为生成的代码是共享的，每个对象引用类型变量可能是任何对象引用类型。 这会阻止提前准备一些运行时资源。

特别是，使用通用类型变量的方法会在后台怠缓地分配资源。 这些被称为通用字典条目。 例如，对于语句（ `List<T> list = new List<T>();` 其中 `T` 是泛型类型变量），运行时必须在运行时查找并可能创建准确的实例化，例如，等等 `List<Object>, List<String>` 。 这可能由于超出开发人员控制外的各种原因（例如内存不足）而失败。

仅应在 JIT 编译时激活此 MDA，而非有精确的实例化时激活。

此 MDA 激活时，可能的症状是 CER 对于不良实例化无法正常运行。 事实上，运行时还没尝试在导致此 MDA 被激活的情况下实现 CER。 因此，如果开发人员使用共享 CER 实例化，则不会捕获 JIT 编译错误、泛型类型加载错误或预期 CER 区域内的线程中止。

## <a name="resolution"></a>解决方法

不要对可能包含 CRE 的方法使用对象引用类型的通用类型变量。

## <a name="effect-on-the-runtime"></a>对运行时的影响

此 MDA 对 CLR 无任何影响。

## <a name="output"></a>Output

下面是此 MDA 的输出示例：
  
 ```output
 Method 'GenericMethodWithCer', which contains at least one constrained execution region, cannot be prepared automatically since it has one or more unbound generic type parameters.
 The caller must ensure this method is prepared explicitly at run time prior to execution.
 method name="GenericMethodWithCer"
 declaringType name="OpenGenericCERCall"
 ```

## <a name="configuration"></a>Configuration

```xml
<mdaConfig>
  <assistants>
    <openGenericCERCall/>
  </assistants>
</mdaConfig>
```  

## <a name="example"></a>示例

CER 代码未执行。

```csharp
using System;
using System.Collections.Generic;
using System.Runtime.CompilerServices;

class Program
{
    static void Main(string[] args)
    {
        CallGenericMethods();
    }
    static void CallGenericMethods()
    {
        // This call is correct. The instantiation of the method
        // contains only nonreference types.
        MyClass.GenericMethodWithCer<int>();

        // This call is incorrect. A shared version of the method that
        // cannot be completely analyzed will be JIT-compiled. The
        // MDA will be activated at JIT-compile time, not at run time.
        MyClass.GenericMethodWithCer<String>();
    }
}

class MyClass
{
    public static void GenericMethodWithCer<T>()
    {
        RuntimeHelpers.PrepareConstrainedRegions();
        try
        {

        }
        finally
        {
            // This is the start of the CER.
            Console.WriteLine("In finally block.");
        }
    }
}
```

## <a name="see-also"></a>请参阅

- <xref:System.Runtime.CompilerServices.RuntimeHelpers.PrepareMethod%2A>
- <xref:System.Runtime.ConstrainedExecution>
- [使用托管调试助手诊断错误](diagnosing-errors-with-managed-debugging-assistants.md)
