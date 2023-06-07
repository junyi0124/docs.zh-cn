---
title: 程序集安全注意事项
description: 在生成 .NET 程序集时，可指定该程序集运行所需的权限。 本文讨论强名称程序集和签名工具。
ms.date: 08/20/2019
helpviewer_keywords:
- assemblies [.NET], security
- signcodes
- names [.NET], assemblies
- strong-named assemblies, security considerations
- signing assemblies
- assemblies [.NET], signing
- granting permissions, assemblies
- assemblies [.NET], strong-named
- names [.NET], strong names
- permissions [.NET], assemblies
- security [.NET], assemblies
- integrity with assemblies
ms.assetid: 1b5439c1-f3d5-4529-bd69-01814703d067
ms.openlocfilehash: 91ea206abf80da275651854b9f13aa0116b7a1c5
ms.sourcegitcommit: 279fb6e8d515df51676528a7424a1df2f0917116
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2020
ms.locfileid: "92687261"
---
# <a name="assembly-security-considerations"></a>程序集安全注意事项

在生成程序集时，可指定该程序集运行所需的一组权限。 是否将特定的权限授予程序集是基于证据的。  
  
 使用证据有两种截然不同的方式：  
  
- 将输入证据与加载程序所收集的证据合并，以创建用于策略决策的最终证据集。 使用这种语义的方法包括 **Assembly.Load** 、 **Assembly.LoadFrom** 和 **Activator.CreateInstance** 。  
  
- 原封不动地使用输入证据作为用于策略决策的最终证据集。 使用这种语义的方法包括 **Assembly.Load(byte[])** 和 **AppDomain.DefineDynamicAssembly()** 。  
  
  通过在将运行程序集的计算机上设置[安全策略](../../framework/misc/code-access-security-basics.md)，可以授予一些可选的权限。 如果你希望代码可以处理所有潜在的安全异常，可以执行以下操作之一：  
  
- 为您的代码必须具有的所有权限插入权限请求，并预先处理在未授予权限时发生的加载时错误。  
  
- 不要使用权限请求来获取您的代码可能需要的权限，但一定要准备处理在未授予权限时发生的安全异常。  
  
  > [!NOTE]
  > 安全性是一个较为复杂的领域，您将要作出很多选择。 有关详细信息，请参阅[安全性的基础概念](../security/key-security-concepts.md)。  
  
 在加载时，程序集的证据用作安全策略的输入。 安全策略是由企业和计算机的管理员以及用户策略设置建立的，它在执行时确定向所有托管代码授予的权限组。 可以为该程序集（如果该程序集具有签名工具生成的签名）的发行者建立安全策略，或者为该程序集的下载网站和区域（就 Internet Explorer 而言）建立安全策略，也可以为该程序集的强名称建立该策略。 例如，一台计算机的管理员可以建立这样一种安全策略：它允许从某一网站下载由指定软件公司签发用以访问计算机上的数据库的所有代码，但不授予对该计算机磁盘的写访问权。  
  
## <a name="strong-named-assemblies-and-signing-tools"></a>强名称程序集和签名工具  

 > [!WARNING]
 > 不要依赖于通过强名称实现安全性。 它们仅提供唯一的标识。

 可采用两种不同但相互补充的方式对程序集进行签名：使用强名称或使用 [SignTool.exe（签名工具）](../../framework/tools/signtool-exe.md)。 使用强名称对程序集进行签名将向包含程序集清单的文件添加公钥加密。 强名称签名帮助验证名称的唯一性，避免名称欺骗，并在解析引用时向调用方提供某标识。  
  
 任何信任级别都不会与一个强名称关联，这就使 [SignTool.exe（签名工具）](../../framework/tools/signtool-exe.md)变得十分重要。 这两个签名工具要求发行者向第三方证书颁发机构证实其标识并获取证书。 然后此证书将嵌入到你的文件中，并且管理员能够使用该证书来决定是否相信这些代码的真实性。  
  
 可将强名称和使用 [SignTool.exe（签名工具）](../../framework/tools/signtool-exe.md)创建的数字签名一起提供给程序集，或者可以单独使用其中之一。 这两个签名工具一次只能对一个文件进行签名，对于多文件程序集，您可以对包含程序集清单的文件进行签名。 强名称存储在包含程序集清单的文件中，但使用 [SignTool.exe（签名工具）](../../framework/tools/signtool-exe.md)创建的签名存储在该程序集清单所在的可迁移可执行 (PE) 文件中保留的槽中。 在已具有依赖于 [SignTool.exe（签名工具）](../../framework/tools/signtool-exe.md)生成的签名的信任层次结构时，或者当策略只使用密钥部分并且不检查信任链时，就可以使用 [SignTool.exe（签名工具）](../../framework/tools/signtool-exe.md)对程序集进行的签名（带或不带强名称）。  
  
> [!NOTE]
> 在一个程序集上同时使用强名称和签名工具签名时，必须首先分配强名称。  
  
 公共语言运行时还将执行哈希验证；程序集清单包含构成该程序集的所有文件的列表，包括当生成清单时存在的每一文件的散列。 在加载每个文件时，将对文件的内容进行哈希处理，并将该内容与存储在清单中的哈希值进行比较。 如果两个哈希值不匹配，则将无法加载该程序集。  
  
 因为强命名和使用 [SignTool.exe（签名工具）](../../framework/tools/signtool-exe.md)进行签名确保了完整性，因此可将代码访问安全策略建立在这两种形式的程序集证据的基础上。 强命名和使用 [SignTool.exe（签名工具）](../../framework/tools/signtool-exe.md)进行签名通过数字签名和证书来确保完整性。 上面提到的所有技术（哈希验证、强命名和使用 [SignTool.exe（签名工具）](../../framework/tools/signtool-exe.md) 进行签名）可协同工作，确保程序集没有做过任何方式的改动。  
  
## <a name="see-also"></a>请参阅

- [具有强名称的程序集](strong-named.md)
- [.NET 中的程序集](index.md)
- [SignTool.exe（签名工具）](../../framework/tools/signtool-exe.md)
