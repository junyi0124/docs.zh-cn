---
title: 结构分析工具 (Mefx)
description: 了解组合分析工具 (Mefx)，该工具用于分析包含 .NET 中的 Managed Extensibility Framework (MEF) 部件的 DLL 和 EXE 文件。
ms.date: 03/30/2017
helpviewer_keywords:
- Composition Analysis Tool [MEF]
- MEF, Composition Analysis Tool
- Mefx [MEF], Composition Analysis Tool
ms.assetid: c48a7f93-83bb-4a06-aea0-d8e7bd1502ad
ms.openlocfilehash: d3f3a282cfa9274a1939d312987dd58b24eab2af
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96255788"
---
# <a name="composition-analysis-tool-mefx"></a>结构分析工具 (Mefx)

组合分析工具 (Mefx) 是一款命令行应用程序，用于分析包含 Managed Extensibility Framework (MEF) 部件的库 (.dll) 和应用程序 (.exe) 文件。 Mefx 的主要用途是为开发人员提供一种无需向应用程序本身添加繁琐的跟踪代码即可诊断其 MEF 应用程序中组合失败情况的方法。 它还可用于帮助你了解第三方所提供库中的部件。 本主题介绍如何使用 Mefx，并提供其语法参考。  
  
<a name="getting_mefx"></a>

## <a name="getting-mefx"></a>获取 Mefx  

 可在 GitHub 的 [Managed Extensibility Framework](https://github.com/MicrosoftArchive/mef/releases/tag/4.0) 上获取 Mefx。 只需下载并解压缩该工具。  
  
<a name="basic_syntax"></a>

## <a name="basic-syntax"></a>基本语法  

 采用以下格式从命令行调用 Mefx：  
  
```console
mefx [files and directories] [action] [options]  
```  
  
 第一组参数指定要从其加载部件进行分析的文件和目录。 使用 `/file:` 开关指定文件，使用 `/directory:` 开关指定目录。 你可以指定多个文件或目录，如下面的示例所示：  
  
```console  
mefx /file:MyAddIn.dll /directory:Program\AddIns [action...]  
```  
  
> [!NOTE]
> 每个 .dll 或 .exe 应仅加载一次。 如果多次加载文件，则该工具可能返回错误信息。  
  
 列出文件和目录后，必须指定一个命令并为该命令指定一个选项。  
  
<a name="listing_available_parts"></a>

## <a name="listing-available-parts"></a>列出可用部件  

 使用 `/parts` 操作列出所加载文件中声明的所有部件。 此时将显示一个简单的部件名称列表。  
  
```console
mefx /file:MyAddIn.dll /parts  
MyAddIn.AddIn  
MyAddIn.MemberPart  
```  
  
 有关部件的详细信息，请使用 `/verbose` 选项。 这将输出所有可用部件的详细信息。 若要获取有关单个部件的详细信息，请使用 `/type` 操作，而不是 `/parts`。  
  
```console  
mefx /file:MyAddIn.dll /type:MyAddIn.AddIn /verbose  
[Part] MyAddIn.MemberPart from: AssemblyCatalog (Assembly=" MyAddIn, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null")  
  [Export] MyAddIn.MemberPart (ContractName=" MyAddIn.MemberPart")  
```  
  
<a name="listing_imports_and_exports"></a>

## <a name="listing-imports-and-exports"></a>列出导入和导出的部件  

 `/imports` 和 `/exports` 操作分别将列出所有导入的部件和所有导出的部件。 你还可以通过使用 `/importers` 或 `/exporters` 操作列出导入或导出特殊类型的部件。  
  
```console  
mefx /file:MyAddIn.dll /importers:MyAddin.MemberPart  
MyAddin.AddIn  
```  
  
 你还可以向这些操作应用 `/verbose` 选项。  
  
<a name="finding_rejected_parts"></a>

## <a name="finding-rejected-parts"></a>查找拒绝的部件  

 一旦加载可用部件，Mefx 将使用 MEF 组合引擎将它们组合起来。 不能成功组合的部件称为 *“拒绝的部件”* 。 若要列出所有拒绝的部件，请使用 `/rejected` 操作。  
  
 你可以结合使用 `/verbose` 选项和 `/rejected` 选项打印关于拒绝的部件的详细信息。 在下面的示例中， `ClassLibrary1` DLL 包含 `AddIn` 部件，该部件导入 `MemberPart` 和 `ChainOne` 部件。 `ChainOne` 导入 `ChainTwo`，但 `ChainTwo` 不存在。 这意味着， `ChainOne` 被拒绝，这将导致 `AddIn` 被拒绝。  
  
```console  
mefx /file:ClassLibrary1.dll /rejected /verbose  
```  
  
 下面的示例显示上一条命令的完整输出：  
  
```output
[Part] ClassLibrary1.AddIn from: AssemblyCatalog (Assembly="ClassLibrary1, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null")  
  [Export] ClassLibrary1.AddIn (ContractName="ClassLibrary1.AddIn")  
  [Import] ClassLibrary1.AddIn.memberPart (ContractName="ClassLibrary1.MemberPart")  
    [SatisfiedBy] ClassLibrary1.MemberPart (ContractName="ClassLibrary1.MemberPart") from: ClassLibrary1.MemberPart from: AssemblyCatalog (Assembly="ClassLibrar  
y1, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null")  
  [Import] ClassLibrary1.AddIn.chain (ContractName="ClassLibrary1.ChainOne")  
    [Exception] System.ComponentModel.Composition.ImportCardinalityMismatchException: No valid exports were found that match the constraint '((exportDefinition.ContractName == "ClassLibrary1.ChainOne") AndAlso (exportDefinition.Metadata.ContainsKey("ExportTypeIdentity") AndAlso "ClassLibrary1.ChainOne".Equals(exportDefinition.Metadata.get_Item("ExportTypeIdentity"))))', invalid exports may have been rejected.  
   at System.ComponentModel.Composition.Hosting.ExportProvider.GetExports(ImportDefinition definition, AtomicComposition atomicComposition)  
   at Microsoft.ComponentModel.Composition.Diagnostics.CompositionInfo.AnalyzeImportDefinition(ExportProvider host, IEnumerable`1 availableParts, ImportDefinition id)  
    [Unsuitable] ClassLibrary1.ChainOne (ContractName="ClassLibrary1.ChainOne")  
from: ClassLibrary1.ChainOne from: AssemblyCatalog (Assembly="ClassLibrary1, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null")  
      [Because] PartDefinitionIsRejected, The part providing the export is rejected because of other issues.  
  
[Part] ClassLibrary1.ChainOne from: AssemblyCatalog (Assembly="ClassLibrary1, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null")  
  [Primary Rejection]  
  [Export] ClassLibrary1.ChainOne (ContractName="ClassLibrary1.ChainOne")  
  [Import] ClassLibrary1.ChainOne.chain (ContractName="ClassLibrary1.ChainTwo")  
    [Exception] System.ComponentModel.Composition.ImportCardinalityMismatchException: No valid exports were found that match the constraint '((exportDefinition.ContractName == "ClassLibrary1.ChainTwo") AndAlso (exportDefinition.Metadata.ContainsKey("ExportTypeIdentity") AndAlso "ClassLibrary1.ChainTwo".Equals(exportDefinition.Metadata.get_Item("ExportTypeIdentity"))))', invalid exports may have been rejected.  
   at System.ComponentModel.Composition.Hosting.ExportProvider.GetExports(ImportDefinition definition, AtomicComposition atomicComposition)  
   at Microsoft.ComponentModel.Composition.Diagnostics.CompositionInfo.AnalyzeImportDefinition(ExportProvider host, IEnumerable`1 availableParts, ImportDefinition id)  
```  
  
 相关信息包含在 `[Exception]` 和 `[Unsuitable]` 结果中。 `[Exception]` 结果提供有关部件为何被拒绝的信息。 `[Unsuitable]` 结果指示以其他方式匹配的部件为何不能填充导入；在本例中，是因为该部件本身被拒绝而不能导入。  
  
<a name="analyzing_primary_causes"></a>

## <a name="analyzing-primary-causes"></a>分析主要原因  

 如果多个部件位于一个长的依赖关系链中，则关系链底部涉及部件的问题可能会导致整个关系链被拒绝。 由于失败的根源并不总是显而易见的，诊断这些问题可能很困难。 若要帮助解决问题，可以使用 `/causes` 操作，该操作将尝试查找任何级联拒绝的根源。  
  
 在前面的示例中使用 `/causes` 操作将仅列出 `ChainOne`的信息，其未填充导入是 `AddIn`被拒的根源。 `/causes` 操作可在正常选项和 `/verbose` 选项中使用。  
  
> [!NOTE]
> 在大多数情况下，Mefx 将能够诊断级联失败的根源。 但是，在以编程方式将部件添加到容器的情况下，在涉及层次结构容器的情况下，或者在涉及自定义 `ExportProvider` 实现的情况下，Mefx 将不能诊断原因。 一般情况下，因尽可能避免前面所述的各种情况，因为失败原因通常很难诊断。  
  
<a name="white_lists"></a>

## <a name="white-lists"></a>白名单  

 使用 `/whitelist` 选项可以指定一个文本文件，其中列出预期将被拒绝的部件。 意外的拒绝则将被标记出来。 在分析不完整的库或分析缺少某些依赖关系的子库时，这将很有帮助。 `/whitelist` 选项可应用于 `/rejected` 或 `/causes` 操作。  
  
 请考虑名为 test.txt 且包含文本“ClassLibrary1.ChainOne”的文件。 如果在前面的示例中将 `/rejected` 操作用于 `/whitelist` 选项，它将生成以下输出：  
  
```console
mefx /file:ClassLibrary1.dll /rejected /whitelist:test.txt  
[Unexpected] ClassLibrary1.AddIn  
ClassLibrary1.ChainOne  
```
