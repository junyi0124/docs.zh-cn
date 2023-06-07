---
title: 如何：加载和卸载程序集
description: CLR 会自动加载程序引用的 .NET 程序集。 你还可以将特定程序集动态加载到当前应用程序域。
ms.date: 08/19/2019
ms.assetid: 6a4f490f-3576-471f-9533-003737cad4a3
ms.openlocfilehash: fe1a5fe63361620f8ab99ec8469169ed2213c0ee
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95731483"
---
# <a name="how-to-load-and-unload-assemblies"></a>如何：加载和卸载程序集

公共语言运行时会自动加载程序所引用的程序集，但也可以将特定的程序集动态加载到当前的应用程序域。 有关详细信息，请参阅[如何：将程序集加载到应用程序域中](../../framework/app-domains/how-to-load-assemblies-into-an-application-domain.md)。

在 .NET Framework 中，只有在卸载完所有包含单个程序集的应用程序域之后，才能卸载此程序集。 即使程序集不在范围之内，在卸载包含它的所有应用程序域之前，实际的程序集文件都将保持加载状态。 在 .NET Core 中，<xref:System.Runtime.Loader.AssemblyLoadContext?displayProperty=nameWithType> 类处理程序集的卸载。 有关详细信息，请参阅[如何在 .NET Core 中使用和调试程序集可卸载性](unloadability.md)。

## <a name="load-and-unload-assemblies"></a>加载和卸载程序集

要将程序集加载到应用程序域中，请使用 <xref:System.AppDomain> 和 <xref:System.Reflection.Assembly> 类中包含的几种加载方法中的一种。 有关详细信息，请参阅[如何：将程序集加载到应用程序域中](../../framework/app-domains/how-to-load-assemblies-into-an-application-domain.md)。 请注意，.NET Core 仅支持单个应用程序域。

若要在 .NET Framework 中卸载程序集，必须卸载包含它的所有应用程序域。 要卸载应用程序域，请使用 <xref:System.AppDomain.Unload%2A?displayProperty=nameWithType> 方法。 有关详细信息，请参阅[如何：卸载应用程序域](../../framework/app-domains/how-to-unload-an-application-domain.md)。

如果想要卸载 .NET Framework 应用程序中的某些程序集而不是其他程序集，请考虑创建一个新的应用程序域，在此域中执行代码，然后卸载此应用程序域。 有关详细信息，请参阅[如何：卸载应用程序域](../../framework/app-domains/how-to-unload-an-application-domain.md)。  

## <a name="see-also"></a>请参阅

- [C# 编程指南](../../csharp/programming-guide/index.md)
- [编程概念 (Visual Basic)](../../visual-basic/programming-guide/concepts/index.md)
- [.NET 中的程序集](index.md)
- [如何：将程序集加载到应用程序域中](../../framework/app-domains/how-to-load-assemblies-into-an-application-domain.md)
