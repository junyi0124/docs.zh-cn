---
title: 将类型库当作程序集导入
description: 将包含 COM 类型定义的类型库当作程序集导入。 了解如何从类型库创建元数据，从而生成互操作程序集。
ms.date: 03/30/2017
helpviewer_keywords:
- importing type library
- type metadata
- custom wrappers
- metadata
- exposing COM components to .NET Framework
- Type Library Importer
- interoperation with unmanaged code, importing type library
- interoperation with unmanaged code, exposing COM components
- type libraries
- TypeLibConverter class, importing type library as assembly
- COM interop, importing type library
- COM interop, exposing COM components
ms.assetid: d1898229-cd40-426e-a275-f3eb65fbc79f
ms.openlocfilehash: bc1b921fea5aff086e21c046369f1d461f553bc7
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90554679"
---
# <a name="importing-a-type-library-as-an-assembly"></a>将类型库当作程序集导入

COM 类型定义通常位于类型库中。 而符合 CLS 的编译器则在程序集中生成类型元数据。 类型信息的这两种来源具有很大的区别。 本主题将说明从类型库中生成元数据的技术。 生成的程序集称为互操作程序集，其中包含的类型信息允许 .NET Framework 应用程序使用 COM 类型。

可通过两种方式将此类型信息提供给应用程序：

- 使用仅设计时互操作程序集：从 .NET Framework 4 开始，可以指示编译器将类型信息从互操作程序集嵌入到可执行文件中。 编译器只嵌入应用程序使用的类型信息。 无需将互操作程序集与应用程序一起部署。 这是推荐采用的方法。

- 部署互操作程序集：创建对互操作程序集的标准引用。 这种情况下，互操作程序集必须与应用程序一起部署。 如果使用此方法且不使用专有 COM 组件，请始终引用由打算并入托管代码中的 COM 组件的创建者发布的主互操作程序集 (PIA)。 有关生成和使用主互操作程序集的详细信息，请参阅[主互操作程序集](/previous-versions/dotnet/netframework-4.0/aax7sdch(v=vs.100))。

使用仅设计时互操作程序集时，可以嵌入 COM 组件创建者发布的主互操作程序集中的类型信息。 但是，无需将主互操作程序集与应用程序一起部署。

使用仅设计时互操作程序集可以缩减应用程序的大小，因为大部分应用程序不会用到 COM 组件的所有功能。 编译器在嵌入类型信息时效率非常高，如果应用程序只使用 COM 接口上的部分方法，则编译器不嵌入未使用的方法。 具有嵌入的类型信息的应用程序与另一个此类应用程序交互时，或者与一个使用主互操作程序集的应用程序交互时，公共语言运行时使用类型等效规则来确定同名的两个类型是否表示相同的 COM 类型。 使用 COM 对象不需要知道这些规则。 但是，若对这些规则感兴趣，请参阅[类型等效性和嵌入的互操作类型](type-equivalence-and-embedded-interop-types.md)。

## <a name="generating-metadata"></a>生成元数据

COM 类型库可以是扩展名名 .tlb 的独立文件，例如 Loanlib.tlb。 部分类型库嵌入在 .dll 或 .exe 文件的资源部分中。 类型库信息的其它来源为 .olb 和 .ocx 文件。

找到包含目标 COM 类型的实现的类型库后，可以通过下列选项来生成包含类型元数据的互操作程序集：

- Visual Studio

  Visual Studio 将类型库中的 COM 类型自动转换为程序集中的元数据。 有关说明，请参阅[如何：添加对类型库的引用](how-to-add-references-to-type-libraries.md)中所述。

- [类型库导入程序 (Tlbimp.exe)](../tools/tlbimp-exe-type-library-importer.md)

  类型库导入程序提供命令行选项，用于调整生成的互操作文件中的元数据、从现有类型库导入类型以及生成互操作程序和命名空间。 有关说明，请参阅[如何：从类型库生成互操作程序集](how-to-generate-interop-assemblies-from-type-libraries.md)。

- <xref:System.Runtime.InteropServices.TypeLibConverter?displayProperty=nameWithType> 类

  此类提供将类型库中的组件类和接口转换为程序集中的元数据的方法。 它生成和 Tlbimp.exe 相同的元数据输出。 但与 Tlbimp.exe 不同的是，<xref:System.Runtime.InteropServices.TypeLibConverter> 类可以将内存中类型库转换为元数据。

- 自定义包装器

  类型库不可用或不正确时，一种可选的做法是在托管源代码中创建类或接口的重复定义。 然后，使用面向运行时的编译器来编译源代码，生成程序集中的元数据。

  要手动定义 COM 类型，必须具有对以下项的访问权限：

  - 所定义的组件类和接口的精确描述。

  - 可生成正确 .NET Framework 类定义的编译器，例如 C# 编译器。

  - 有关类型库到程序集转换规则的知识。

  编写自定义包装器是一项高级技术。 有关如何生成自定义包装器的其他信息，请参阅[自定义标准包装器](/previous-versions/dotnet/netframework-4.0/h7hx9abd(v=vs.100))。

 有关 COM 互操作导入过程的详细信息，请参阅[有关从类型库转换到程序集的摘要](/previous-versions/dotnet/netframework-4.0/k83zzh38(v=vs.100))。

## <a name="see-also"></a>请参阅

- <xref:System.Runtime.InteropServices.TypeLibConverter>
- [向 .NET Framework 公开 COM 组件](exposing-com-components.md)
- [有关从类型库转换到程序集的摘要](/previous-versions/dotnet/netframework-4.0/k83zzh38(v=vs.100))
- [Tlbimp.exe（类型库导入程序）](../tools/tlbimp-exe-type-library-importer.md)
- [自定义标准包装器](/previous-versions/dotnet/netframework-4.0/h7hx9abd(v=vs.100))
- [在托管代码中使用 COM 类型](/previous-versions/dotnet/netframework-4.0/3y76b69k(v=vs.100))
- [编译互操作项目](compiling-an-interop-project.md)
- [部署互操作应用程序](deploying-an-interop-application.md)
- [如何：添加对类型库的引用](how-to-add-references-to-type-libraries.md)
- [如何：从类型库生成互操作程序集](how-to-generate-interop-assemblies-from-type-libraries.md)
