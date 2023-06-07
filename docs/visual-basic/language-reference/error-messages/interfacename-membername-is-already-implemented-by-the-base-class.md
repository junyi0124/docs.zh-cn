---
title: “<interfacename>.<membername>”已经由基类“<baseclassname>”实现。 假定重新实现 <type>
ms.date: 07/20/2015
f1_keywords:
- vbc42015
- bc42015
helpviewer_keywords:
- BC42015
ms.assetid: 658c070a-113e-4bd8-b294-12c243191160
ms.openlocfilehash: 8137f6b1712b6a0752a991f5a3d598b5f958252c
ms.sourcegitcommit: ff5a4eb5cffbcac9521bc44a907a118cd7e8638d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2020
ms.locfileid: "92162812"
---
# <a name="bc42015-interfacenamemembername-is-already-implemented-by-the-base-class-baseclassname-re-implementation-of-type-assumed"></a>BC42015： " \<interfacename> . \<membername> " 已经由基类 " \<baseclassname> " 实现。 假定重新实现 \<type>

派生类中的属性、过程或事件使用子句，用于 `Implements` 指定已在基类中实现的接口成员。

 派生类可以重新实现由其基类实现的接口成员。 这与重写基类实现不同。 有关详细信息，请参阅 [Implements](../statements/implements-clause.md)。

 默认情况下，此消息是一个警告。 有关隐藏警告或将警告视为错误的信息，请参见 [Configuring Warnings in Visual Basic](/visualstudio/ide/configuring-warnings-in-visual-basic)。

 **错误 ID：** BC42015

## <a name="to-correct-this-error"></a>更正此错误

- 如果要重新实现接口成员，则无需执行任何操作。 派生类中的代码访问重新实现成员，除非使用 `MyBase` 关键字访问基类实现。

- 如果不打算重新实现接口成员，请从属性、过程或事件声明中删除 `Implements` 子句。

## <a name="see-also"></a>另请参阅

- [接口](../../programming-guide/language-features/interfaces/index.md)
