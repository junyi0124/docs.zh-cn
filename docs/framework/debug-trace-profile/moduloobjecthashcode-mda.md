---
title: moduloObjectHashcode MDA
description: 查看 moduloObjectHashcode 托管调试助手 (MDA) ，这将更改对象类以获取 GetHashCode 方法结果上的余数值。
ms.date: 03/30/2017
helpviewer_keywords:
- managed debugging assistants (MDAs), hashcode modulus
- Modulo object hash code
- moduloObjectHashcode MDA
- hashcode modulus
- MDAs (managed debugging assistants), hashcode modulus
- GetHashCode method
- modulus of hashcodes
ms.assetid: b45366ff-2a7a-4b8e-ab01-537b72e9de68
ms.openlocfilehash: 6258d5df7be1923a69de3172dd4c7f32e0b51f35
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96240519"
---
# <a name="moduloobjecthashcode-mda"></a>moduloObjectHashcode MDA

`moduloObjectHashcode` 托管调试助手 (MDA) 更改 <xref:System.Object> 类的行为，以便对 <xref:System.Object.GetHashCode%2A> 方法返回的哈希代码执行取模运算。 此 MDA 的模数默认为 1，这将导致 <xref:System.Object.GetHashCode%2A> 对所有对象都返回 0。  
  
## <a name="symptoms"></a>症状  

 在迁移到公共语言运行时 (CLR) 的新版本后，程序将不再正确执行：  
  
- 程序从 <xref:System.Collections.Hashtable> 获得错误的对象。  
  
- <xref:System.Collections.Hashtable> 中的枚举顺序发生了一项变化，导致程序中断。  
  
- 以前相等的两个对象不再相等。  
  
- 以前不相等的两个对象现在相等。  
  
## <a name="cause"></a>原因  

 由于在 <xref:System.Collections.Hashtable> 中的键的类上，<xref:System.Object.Equals%2A> 方法实现通过比较 <xref:System.Object.GetHashCode%2A> 方法调用的结果测试对象是否相等，因此程序可能从 <xref:System.Collections.Hashtable> 中得到错误的对象。 不应该使用哈希代码测试对象是否相等，因为即使两个对象各自的字段具有不同的值，它们也可能具有相同的哈希代码。 虽然在实践中很罕见，但的确可能会发生这些哈希代码冲突。 这对 <xref:System.Collections.Hashtable> 查找的影响是两个不相等的键似乎相等，并从 <xref:System.Collections.Hashtable> 返回错误的对象。 出于性能的考虑，<xref:System.Object.GetHashCode%2A> 的实现在运行时的两个版本之间可能改变，因此在一个版本中可能不会出现的冲突可能会在随后的版本中出现。 启用此 MDA 测试代码在哈希代码冲突时是否有 bug。 启用后，此 MDA 将导致 <xref:System.Object.GetHashCode%2A> 方法返回 0，造成所有哈希代码冲突。 启用此 MDA 对程序的唯一影响是程序将会运行得更慢。  
  
 如果用于计算键的哈希代码的算法改变，则 <xref:System.Collections.Hashtable> 中的枚举顺序可能随运行时版本而改变。 要测试程序是否依赖键的枚举顺序或哈希表的值，可启用此 MDA。  
  
## <a name="resolution"></a>解决方法  

 从不使用哈希代码替换对象标识。 实现 <xref:System.Object.Equals%2A?displayProperty=nameWithType> 方法的重写，以便不比较哈希代码。  
  
 不要创建依赖哈希表中的键或值的枚举顺序的依赖项。  
  
## <a name="effect-on-the-runtime"></a>对运行时的影响  

 启用了此 MDA 时，应用程序的运行速度更慢。 此 MDA 只取得原本应该返回的哈希代码，改为返回除以某个模数之后得到的余数。  
  
## <a name="output"></a>输出  

 此 MDA 没有输出。  
  
## <a name="configuration"></a>Configuration  

 `modulus` 属性指定用于哈希代码的模数。 默认值为 1。  
  
```xml  
<mdaConfig>  
  <assistants>  
    <moduloObjectHashcode modulus="1" />  
  </assistants>  
</mdaConfig>  
```  
  
## <a name="see-also"></a>另请参阅

- <xref:System.Object.GetHashCode%2A?displayProperty=nameWithType>
- <xref:System.Object.Equals%2A?displayProperty=nameWithType>
- [使用托管调试助手诊断错误](diagnosing-errors-with-managed-debugging-assistants.md)
