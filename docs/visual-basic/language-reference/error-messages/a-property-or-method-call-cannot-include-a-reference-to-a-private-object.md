---
title: 无论是作为参数还是作为返回值，属性或方法调用都不能包括对私有对象的引用
ms.date: 07/20/2015
f1_keywords:
- vbrID98
ms.assetid: 059b43e1-202d-4fa2-806b-7bad63c1e7ca
ms.openlocfilehash: 00237d795fb62fbae426e0015d8e64d5fabb4c32
ms.sourcegitcommit: ff5a4eb5cffbcac9521bc44a907a118cd7e8638d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2020
ms.locfileid: "92162669"
---
# <a name="a-property-or-method-call-cannot-include-a-reference-to-a-private-object-either-as-an-argument-or-as-a-return-value"></a>无论是作为参数还是作为返回值，属性或方法调用都不能包括对私有对象的引用

此错误的可能原因包括：

- 客户端调用了进程外组件的属性或方法，并且试图将对私有对象的引用作为参数之一进行传递。

- 进程外组件调用了其客户端上的回调方法，并试图传递对私有对象的引用。

- 进程外组件试图将对私有对象的引用作为它所引发的事件的参数进行传递。

- 客户端试图将私有对象引用赋给它正在处理的事件的 `ByRef` 参数。

## <a name="to-correct-this-error"></a>更正此错误

- 删除引用。

## <a name="see-also"></a>另请参阅

- [专用](../modifiers/private.md)
