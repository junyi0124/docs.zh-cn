---
title: 兼容性
description: 了解代码更改如何影响 .NET 中的兼容性。
ms.date: 06/10/2019
ms.openlocfilehash: 1cf14b7ff4143367653bd1c305cc1dda6711f980
ms.sourcegitcommit: a2c8b19e813a52b91facbb5d7e3c062c7188b457
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/26/2020
ms.locfileid: "85415688"
---
# <a name="how-code-changes-can-affect-compatibility"></a>代码更改如何影响兼容性

兼容性是指在 .NET 实现版本（而不是最初开发代码的版本）上编译或执行代码的能力。 [特殊的变更](index.md)可能会以六种不同的方式影响兼容性：

- [行为变更](#behavioral-change)
- [二进制兼容性](#binary-compatibility)
- [源兼容性](#source-compatibility)
- [设计时兼容性](#design-time-compatibility)
- [后向兼容性](#backwards-compatibility)
- [前向兼容性](#forward-compatibility)（不是 .NET Core 的目标）

## <a name="behavioral-change"></a>行为变更

行为变更表示对成员行为的更改。 此更改可能对外部可见（例如一种方法可能引发不同的异常），或者它可能表示一种更改的实现（例如，计算返回值的方式更改、添加或删除内部方法调用，或甚至显著性能改进）。

当行为变更对外部可见并修改类型的公共协定时，由于它们影响二进制兼容性，因此易于评估。 实现更改更难评估；取决于更改的性质以及使用 API 的频率和模式，更改的影响可能从严重到无害。

## <a name="binary-compatibility"></a>二进制兼容性

二进制兼容性指的是 API 使用者无需重新编译即可在更新的版本上使用 API。 添加方法或向类型添加新接口实现等此类更改不影响二进制兼容性。 但是，删除或更改程序集的公共签名，以便使用者无法再访问该程序集公开的同一接口，这会影响二进制兼容性。 此类更改称为“二进制不兼容的更改”。

## <a name="source-compatibility"></a>源兼容性

源兼容性指的是 API 的现有使用者无需进行任何源更改即可根据更新的版本重新编译。 当使用者需要更改源代码以根据更新版本的 API 成功生成时，会发生源不兼容的更改。

## <a name="design-time-compatibility"></a>设计时兼容性

设计时兼容性指的是保留不同版本的 Visual Studio 和其他设计时环境中的体验。 虽然这可能涉及到设计器的行为或 UI，但设计时兼容性最重要的方面是项目兼容性。 项目或解决方案必须能够在设计时环境的更新版本中打开和使用。

## <a name="backwards-compatibility"></a>后向兼容性

向后兼容性指的是 API 的现有使用者能够在操作方式相同的情况下在新版本中运行。 行为变更和二进制兼容性更改都会影响向后兼容性。 如果使用者无法在运行更新版本的 API 时以不同方式运行或操作，则 API 是向后不兼容的。

建议不做出影响向后兼容性的更改，因为开发人员需要更新版本的 API 中的向后兼容性。

## <a name="forward-compatibility"></a>向前兼容性

向前兼容性指的是 API 的现有使用者能够在进行相同操作的情况下在旧版本中运行。 如果使用者无法在运行旧版本的 API 时以不同方式运行或操作，则 API 是向前不兼容的。

保持向前兼容性实际上排除了版本之间的任何更改或添加，因为这些更改会阻止面向较新版本的消费者在较低版本下运行。 开发人员认为依赖于较新 API 的使用者可能无法正常运行较旧的 API。

保持向前兼容性不是 .NET Core 的目标。

## <a name="see-also"></a>请参阅

- [评估 .NET Core 中的中断性变更](index.md)
