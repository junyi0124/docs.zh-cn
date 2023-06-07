---
title: 布尔运算符
description: 了解有关中可用的布尔运算符F#编程语言。
ms.date: 05/16/2016
ms.openlocfilehash: ad4bdd1121389f7e280647dbe0c4d0098ffb17df
ms.sourcegitcommit: 8699383914c24a0df033393f55db3369db728a7b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/15/2019
ms.locfileid: "65641898"
---
# <a name="boolean-operators"></a>布尔运算符

本主题介绍中的布尔运算符的支持F#语言。

## <a name="summary-of-boolean-operators"></a>布尔运算符的摘要

下表总结了中可用的布尔运算符F#语言。 这些运算符支持的唯一类型是`bool`类型。

|运算符|描述|
|--------|-----------|
|`not`|布尔值求反|
|<code>&#124;&#124;</code>|布尔 OR|
|`&&`|布尔 AND|

执行布尔 AND 和 OR 运算符*短路计算*，即，它们仅在必要时以确定总体结果表达式的计算运算符右侧的表达式。 第二个表达式`&&`仅当第一个表达式的计算结果为计算运算符`true`; 的第二个表达式`||`仅当第一个表达式的计算结果为计算运算符`false`。

## <a name="see-also"></a>请参阅

- [位运算符](bitwise-operators.md)
- [算术运算符](arithmetic-operators.md)
- [符号和运算符参考](index.md)
