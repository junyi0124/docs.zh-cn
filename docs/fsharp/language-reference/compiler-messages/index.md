---
title: 编译器错误和警告
description: F# 编译器将发出的错误和警告的说明和解决方案
ms.date: 12/21/2019
ms.openlocfilehash: 58430297abe807027afdc52841d67d1233401ff1
ms.sourcegitcommit: e395fabeeea5c705d243d246fa64446839ac85b6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/03/2021
ms.locfileid: "97856115"
---
# <a name="f-compiler-messages"></a>F# 编译器消息

此部分详细介绍 F# 编译器将针对某些构造发出的编译器错误和警告。 可以通过以下方式更改默认错误集：

- 使用 `-warnaserror+` 编译器选项，将特定警告视为错误，

- 使用 `-nowarn` 编译器选项，忽略特定警告

如果此部分中尚未记录特定警告或错误：

- 请转到此页末尾，然后发送包含错误号或文本的反馈，或者
- 按照 [create-new-fsharp-compiler-message.fsx](https://github.com/dotnet/docs/blob/master/docs/fsharp/language-reference/compiler-messages/util/create-new-fsharp-compiler-message.fsx) 中的说明进行操作，并打开此存储库的拉取请求，自行添加。

## <a name="see-also"></a>另请参阅

- [F# 编译器选项](../compiler-options.md)
