---
title: 程序集和并行执行
description: 了解并行执行，它是在同一台计算机上存储和运行应用程序或组件的多个版本的能力。
ms.date: 08/20/2019
helpviewer_keywords:
- side-by-side execution [.NET]
- assemblies [.NET], side-by-side execution
ms.assetid: e42036ee-7590-47d1-b884-cc856e39bd5d
ms.openlocfilehash: 7bedd5a384ceace014412eb894adad5c92c00b05
ms.sourcegitcommit: 279fb6e8d515df51676528a7424a1df2f0917116
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2020
ms.locfileid: "92687977"
---
# <a name="assemblies-and-side-by-side-execution"></a>程序集和并行执行

并行执行是在同一台计算机上存储和执行应用程序或组件的多个版本的能力。 这意味着在同一台计算机上可以同时有运行时的多个版本，并且可以有使用其中某个运行时版本的应用程序和组件的多个版本。 并行执行使您能够更多地控制应用程序绑定到的组件版本和应用程序使用的运行时版本。  
  
支持并行存储和执行同一程序集的不同版本是强命名中不可缺少的部分，这种支持内置于运行时基础结构中。 因为强名称程序集的版本号是其标识的一部分，所以运行时能够在全局程序集缓存中存储同一程序集的多个版本，并且在运行时加载这些程序集。  
  
尽管运行时使您能够创建并行应用程序，但并行执行并不是自动进行的。 有关创建并行执行的应用程序的详细信息，请参阅[并行执行的组件的创建指南](../../framework/deployment/guidelines-for-creating-components-for-side-by-side-execution.md)。  
  
## <a name="see-also"></a>请参阅

- [运行时如何定位程序集](../../framework/deployment/how-the-runtime-locates-assemblies.md)
- [.NET 中的程序集](index.md)
