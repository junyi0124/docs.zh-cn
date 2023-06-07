---
title: Members
description: 了解编程语言中的F#对象成员。
ms.date: 05/16/2016
ms.openlocfilehash: 2e85d014cd1e9b7997638cb210fed5705c217719
ms.sourcegitcommit: 14ad34f7c4564ee0f009acb8bfc0ea7af3bc9541
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/01/2019
ms.locfileid: "73425070"
---
# <a name="members"></a>Members

本部分介绍 F# 对象类型的成员。

## <a name="remarks"></a>备注

*成员*是一种属于类型定义的功能，可使用 `member` 关键字进行声明。 记录、类、可区分联合、接口和结构等 F# 对象类型都支持成员。 有关详细信息，请参阅[记录](../records.md)、[类](../classes.md)、[可区分联合](../discriminated-Unions.md)、[接口](../interfaces.md)和[结构](../structures.md)。

成员通常组成类型的公共接口，这就是成员通常是公共成员（除非另外指定）的原因。 也可以声明私有成员或内部成员。 有关详细信息，请参阅[访问控制](../access-Control.md)。 类型的签名还可用于公开或不公开类型的某些成员。 有关详细信息，请参阅[签名](../signature-files.md)。

只能与类一起使用的私有字段和 `do` 绑定不是真正的成员，因为它们从不是类型的公共接口的一部分，并且也不是用 `member` 关键字声明的，但本部分对它们也进行了介绍。

## <a name="related-topics"></a>相关主题

|主题|描述|
|-----|-----------|
|[类中的 `let` 绑定](let-bindings-in-classes.md)|介绍类中私有字段和函数的定义。|
|[类中的 `do` 绑定](do-bindings-in-classes.md)|介绍对象初始化代码的规范。|
|[属性](properties.md)|介绍类和其他类型中的属性成员。|
|[索引属性](indexed-properties.md)|介绍类和其他类型中的类似数组的属性。|
|[方法](methods.md)|介绍属于类型的成员的函数。|
|[构造函数](constructors.md)|介绍用于初始化类型的对象的特殊函数。|
|[运算符重载](../operator-overloading.md)|介绍类型的自定义运算符的定义。|
|[事件](events.md)|介绍 F# 中的事件定义和事件处理支持。|
|[显式字段：`val` 关键字](explicit-fields-the-val-keyword.md)|介绍类型中未初始化字段的定义。|
