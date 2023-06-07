---
title: 类型提供程序安全性
description: 了解如何在类型提供程序安全性F#，包括如何更改的类型提供程序的信任设置。
ms.date: 05/16/2016
ms.openlocfilehash: 26f95ad3950b37a668c497f293b9941ed13a18c7
ms.sourcegitcommit: 8699383914c24a0df033393f55db3369db728a7b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/15/2019
ms.locfileid: "65641839"
---
# <a name="type-provider-security"></a>类型提供程序安全性

类型提供程序将所引用的程序集 (Dll) 在F#项目或脚本包含代码以连接到外部数据源并呈现到此类型信息的F#类型的环境。 通常情况下，代码中引用的程序集只运行时编译，并执行代码 (或在脚本的情况下发送到的代码F#交互)。 但是，将在 Visual Studio 运行类型提供程序程序集，只是在编辑器中浏览代码时多。 这是因为类型提供程序需要运行到编辑器，例如快速信息工具提示，IntelliSense 完成添加额外信息等。 因此，有一些额外的安全注意事项类型提供程序程序集，因为它们在 Visual Studio 进程内自动运行。

## <a name="security-warning-dialog"></a>安全警告对话框

当首次使用特定类型提供程序程序集，Visual Studio 将显示一个安全对话框，警告类型提供程序即将运行。 Visual Studio 将加载的类型提供程序之前，它使你能够决定是否信任此特定的提供程序。 如果您信任的源类型提供程序，然后选择"我信任此类型提供程序"。 如果不信任的源类型提供程序，然后选择"我不信任此类型提供程序"。 信任提供程序使其能够在 Visual Studio 运行和提供智能感知和生成功能。 但是，如果恶意类型提供程序本身，运行其代码可能会泄露你的计算机。

如果你的项目包含引用类型提供程序选择对话框中不信任的代码，然后在编译时，编译器将报告一个错误，指示类型提供程序不受信任。 由红色波形曲线指示依赖于不受信任的类型提供程序的任何类型。 它可以安全地浏览在编辑器中的代码。

如果您决定更改的信任设置，直接在 Visual Studio 中，执行以下步骤。

### <a name="to-change-the-trust-settings-for-type-providers"></a>若要更改类型提供程序的信任设置

1. 上`Tools`菜单中，选择`Options`，然后展开`F# Tools`节点。

2. 选择`Type Providers`，和在类型提供程序列表中，类型提供程序信任，请选中复选框并清除该复选框，对于那些不信任。

## <a name="see-also"></a>请参阅

- [类型提供程序](index.md)
