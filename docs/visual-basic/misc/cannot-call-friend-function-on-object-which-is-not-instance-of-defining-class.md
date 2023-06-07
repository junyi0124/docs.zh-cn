---
title: 在不是定义类的实例的对象上不能调用友元函数
ms.date: 07/20/2015
f1_keywords:
- vbrID97
ms.assetid: b9d821f0-8565-4f15-bb35-184789c69662
ms.openlocfilehash: fe95f80d42fc12ab2829db899fe295e32f358db6
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91059803"
---
# <a name="cannot-call-friend-function-on-object-which-is-not-an-instance-of-defining-class"></a>在不是定义类的实例的对象上不能调用友元函数

尝试调用某个类的 `Friend` 过程，或者尝试跨进程或跨线程访问 `Friend` 属性或方法。 `Friend` 过程可从类外部的模块中调用，但该模块是在其中定义该类的项目的一部分。  
  
## <a name="to-correct-this-error"></a>更正此错误  
  
- 确保从模块中调用或访问该过程，此模块是在其中定义该类的项目的一部分。  
  
## <a name="see-also"></a>请参阅

- [Friend](../language-reference/modifiers/friend.md)
