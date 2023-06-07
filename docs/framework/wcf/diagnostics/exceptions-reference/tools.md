---
title: 工具
ms.date: 03/30/2017
ms.assetid: 89c907f9-313f-408c-992a-631f1eadf1da
ms.openlocfilehash: 2f7ef8b7f871dada676eb05713d6f624427d5395
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96285689"
---
# <a name="tools"></a>工具

本主题列出了 Windows Communication Foundation (WCF) 工具生成的所有异常。

## <a name="exception-list"></a>异常列表

|资源代码|资源字符串|
|-------------------|---------------------|
|ParametersTarget|\<enum>|
|ParametersToolConfig|\<configFile>|
|ErrInvalidPath|指定的路径无效。 请检查指定的自变量。|
|ParametersReference|\<file path>|
|WrnCannotLoadConfigFileForValidation|处理从指定位置加载的配置文件时出错。 无法验证此配置文件中定义的服务。|
|MoreHelp|有关更多帮助，请键入“svcutil”及指定自变量。|
|HelpMergeConfig|导致生成的配置合并至现有的文件中，而不是覆盖现有的文件。|
|ErrCannotWriteFile|无法写入输出文件。|
|ErrInvalidNamespaceArgument|向指定选项传递了指定的无效值。 请指定逗号分隔的目标命名空间和 CLR 命名空间对。|
|HelpImportXmlType|配置 DataContract 序列化程序，以便将非 DataContract 类型作为 IXmlSerializable 类型导入。|
|ErrExclusiveOptionsSpecified|当已指定了其他指定选项时，将不能使用此指定选项。|
|WrnHttpGetFailed|指定的 URI 发生了 HTTP GET 错误。|
|ErrInputFileNotAssemblyOrMetadata|通过指定输入参数读取的位于指定位置的文件似乎不是 XML 元数据文件或有效程序集。|
|WrnUnknownMetadataFound|不能保存指定类型的无法识别的元数据文档。|
|ErrDirectoryContainsInvalidCharacters|向指定选项传递了指定的无效值。 路径中不允许出现该指定字符。|
|WrnCannotResolveServiceForValidation|无法加载具有指定 configName 的服务。 若要验证服务，请同时提供包含该服务类型的程序集和具有此服务的配置的可执行文件。|
|ErrUnexpectedValue|指定的选项不支持任何值。|
|#InvalidArg|指定的内容包含无效自变量。|
|ParametersExcludeType|\<type>|
|HelpXmlSerializer|生成使用 XmlSerializer 进行序列化和反序列化的数据类型。|
|#|---------------------------------------------------------------------------------------------------------------------=|
|ErrUnexpectedError|工具中发生错误。|
|HelpNologo|取消了版权和标题消息。|
|ErrInputConflictsWithTarget|不支持从指定位置读取的输入类型使用设置为指定值的指定选项。|
|WrnCannotLoadServiceForExport|加载要导出的服务类型时出错。|
|HelpMetadataDownloadCategory|-= 元数据下载 =-|
|WrnNoServiceContractTypes|无法为指定程序集生成 XmlSerializer 类型。 未发现服务协定类型。|
|WrnCouldNotLoadTypesFromReferenceAssemblyAt|加载从指定位置加载的程序集中的类型时出错。 程序集中的某些类型无法加载，工具无法使用这些类型。|
|ErrDirectoryPointsToAFile|向指定选项传递了指定的无效值。 指定的值是文件的路径。|
|错误|错误：|
|ErrDuplicateReferenceValues|使用指定的选项对指定的程序集加载了两次。 程序集只可以引用一次。|
|WrnNoXmlSerializerOperationBehavior|无法为指定程序集生成 XmlSerializer。 程序集中没有任何服务协定使用具有 XmlSerializerOperationBehavior 的操作。|
|ErrCannotCreateDirectory|无法创建指定的目录。|
|ErrCouldNotLoadTypesFromAssemblyAt|无法加载指定的程序集中的任何类型。|
|ErrUnknownSwitch|指定的开关是无法识别的选项。|
|徽标|该工具的徽标是带有版本号的“Microsoft ® Service Model Metadata Tool”。|
|NoCodeWasGenerated|未生成任何代码。<br /><br /> 在尝试生成客户端时，导致此问题的原因可能是元数据文档不包含任何有效的协定或服务，<br /><br /> 或者因为发现所有协定/服务都存在于引用程序集中。 请验证向工具传递了所有元数据文档。|
|WrnUnableToLoadContractForSGen|加载协定类型时出错。 无法为此协定生成 XmlSerializer 类型。 已指定类型和详细信息。|
|WrnOptionConflictsWithInput|不能将指定的选项用于多个输入程序集。 忽略了指定的选项。|
|ErrUnableToImportMetadata|尝试导入元数据时发生严重错误。|
|ErrInvalidSerializer|向指定的选项传递了无效的序列化程序值。 指定了受支持的序列化程序。|
|SavingDownloadedMetadata|正在保存下载的元数据文件...|
|WrnNoConfigForServices|所有传递的程序集都无法使用配置文件来执行，或者所有配置文件都不包含具有指定配置名的服务。|
|ErrInputConflictsWithOption|从指定位置读取的输入不能与指定的选项一起使用，原因是它们指示了不同的工具操作模式。|
|ErrUnableToExportEndpoints|导出指定的服务类型时出错。|
|ErrInputSchemaParseError|读取指定的内容时发生 XML 架构分析错误。 请验证 XML 是否格式正确且有效。|
|ErrInputPolicyParseError|读取指定的内容时发生 WS-Policy 分析错误。 请验证 XML 是否格式正确且有效。|
|ErrUnableToLoadReferenceType|加载所引用的协定类型时出错。 已忽略该指定类型。|
|WrnCannotLoadServiceForValidation|加载要验证的服务时出错。 已指定类型和详细信息。|
|HelpCodeGenerationCategory|-= 代码生成 =-|
|RetreivingMetadataWithMexAndDisco|尝试使用 WS-Metadata Exchange 或 DISCO 从指定位置下载元数据。|
|ErrGeneralSchemaValidation|验证导出期间生成的 XML 架构时出错。|
|ParametersDirectory|\<directory>|
|ErrCannotLoadSpecifiedType|无法为传递给指定选项的指定值加载任何类型。 请确保使用指定选项指定了此类型所属的程序集。|
|ErrOptionModeConflict|该指定的选项不能与另一个指定的选项一起使用，原因是它们指示了不同的输出类型。|
|ErrIsNotAnAssembly|无法将指定的文件加载为程序集。 请验证此文件是否是 .NET 程序集。|
|ErrInputConflictsWithMode|从指定位置读取的输入和其他选项不一致。|
|ErrDuplicateValuePassedToTypeArg|多次向指定的选项传递了指定的值。 每个类型仅可以指定一次。|
|ErrInputEPRFileParseError|无法从指定位置读取终结点引用。 请验证 XML 是否格式正确且有效。|
|ErrCouldNotCreateCodeProvider|无法为传递给/参数的指定值创建代码提供程序 {1} 。 请验证是否正确安装并配置了代码提供程序。|
|ErrPathTooLongDirOnly|产生的指定路径过长。 请检查指定的自变量。|
|HelpDataContractSerializer|生成使用 DataContract 序列化程序进行序列化和反序列化的数据类型。|
|ErrUnableToExportEndpoint|导出在为程序集加载的配置文件中找到的指定服务类型的指定命名空间中的指定终结点名称时出错。|
|HelpUsage1|显示帮助用法。|
|HelpUsage2|显示帮助用法。|
|HelpUsage3|显示帮助用法。|
|HelpUsage4|显示帮助用法。|
|HelpUsage5|显示帮助用法。|
|ErrDirectoryNotFound|找不到指定的目录。 请验证目录是否存在以及您是否具有相应的读取权限。|
|ErrUnableToLoadFile|无法读取指定的文件。|
|ErrNoFilesFound|指定的输入路径似乎未引用任何现有的文件。|
|ParametersConfig|\<configFile>|
|ErrDirectoryInsteadOfFile|指定的输入路径似乎是目录。 输入必须是 URL 或文件路径。|
|HelpConfig|指示工具生成具有所提供名称的配置文件。 默认文件：output.config。|
|ErrSingleUseSwitch|不能多次指定所指定的选项。|
|警告|警告：|
|WrnAmbiguousServiceConfig|发现多个具有指定配置名称的服务配置，指定了以下程序集。|
|ErrInvalidInputPath|指定的输入路径似乎未引用任何现有文件，似乎也不是有效的 URI。|
|ErrUnableToLoadInputs|读取加载的元数据时出错。|
|GeneratingSerializer|正在生成 XML 序列化程序...|
|HelpToolConfig|代替应用程序配置文件使用的自定义配置文件。 可以使用该自定义配置文件来更改元数据配置或注册配置扩展，而无需更改工具的配置文件。|
|ErrValidateInvalidUse|该指定的选项不能与另一个指定的选项一起使用。|
|WrnWSMExFailed|指定的 URI 发生了 WS-Metadata Exchange 错误。|
|HelpNoconfig|未生成配置。|
|HelpCodeGenerationDescription|指定的内容可以从元数据文档生成服务协定、客户端和数据类型。|
|HelpTargetMetadata|输出元数据。 如果输入是 URL，则 Svcutil.exe 将元数据保存到磁盘，且不生成代码。 如果输入是一个或多个程序集，则 Svcutil.exe 从程序集中的类型生成元数据。|
|ErrAmbiguousOptionModeConflict|指定的选项与其他选项冲突。 请检查工具的使用。|
|ErrNotLanguageOrCodeDomType|传递给指定自变量的指定值不代表已定义的语言，无法作为完全限定的 CLR 类型加载。|
|ErrUnableToUniquifyFilename|无法创建输出文件名。 要用指定的前缀创建的文件过多。|
|ErrCannotCreateFile|无法创建指定的输出文件。|
|ErrExpectedValue|指定的选项需要指定一个值。|
|ErrCannotDisambiguateSpecifiedTypes|在被引用程序集的集合中存在多个具有相同名称的类型。 请使用程序集限定的名称来区分指定选项的指定类型。|
|RetreivingMetadataWithMexOnly|尝试使用 WS-Metadata Exchange 从指定位置下载元数据。 此 URL 不支持 DISCO。|
|ErrInvalidTarget|当使用指定的选项指定时，指定的目标无效。 已指定受支持的目标。|
|ErrPathTooLong|产生的路径过长。 请评审指定的自变量。|
|HelpCommonOptionsCategory|-= 常用选项 =-|
|ParametersServiceName|\<serviceConfigName>|
|ErrNoValidInputFilesSpecified|没有指定有效的输入文件。 请指定元数据文档或程序集文件。|
|ParametersLanguage|\<language>|
|ErrUnableToLoadMetadataDocument|从加载的文档之一读取元数据时出错。 已指定文档标识符。|
|ErrConflictingInputs|指定的输入参数与指定内容冲突，因为它们指示了不同的工具操作模式。|
|WrnUnableToLoadContractForValidation|加载协定类型时出错。 已指定类型和详细信息。|
|WrnAttributeReflectionErrors|从指定位置加载的程序集中的某些类型的属性反射失败。 请验证此程序集是否可以从此位置使用正确的安全特权加载。|
|HelpMetadataExportCategory|-= 元数据导出 =-|
|HelpValidationCategory|-= 服务验证 =-|
|ValidationError|验证错误：|
|GeneratingFiles|正在生成文件...|
|ErrCannotSpecifyMultipleMappingsForNamespace|向指定选项传递了无效值。 指定的目标命名空间无法像指定的那样映射到多个 CLR 命名空间。|
|ErrCouldNotLoadReferenceAssemblyAt|无法加载指定的引用程序集。|
|ParametersOut|\<file>|
|NoCodeWasGeneratedSuggestDCOnly|若要从架构生成协定，请使用指定选项。|
|ErrUnableToLoadInputConfig|无法加载指定的配置文件。|
|ErrUnexpectedDelimiter|无效的参数分隔符（“:”或“=”）无法启动选项。|
|ErrMergeConfigUsedWithoutConfig|无法在未指定其他指定选项的情况下使用该指定选项。|
|ErrUnableToExportContract|导出从指定类型加载的协定时出错。|
|GeneratingMetadata|正在生成元数据文件...|
|ErrNotCodeDomType|传递给指定参数的指定类型不是指定的派生类。|
|WrnNoTypeForServices|传递的程序集都不包含具有指定配置名称的服务类型。|
|ErrAssemblyLoadFailed|无法将指定的文件加载为程序集。 有关详细信息，请参考 FusionLog。|
|NoMetadataWasGenerated|未生成任何元数据文件。 未导出任何服务协定。<br /><br /> 若要导出服务，请使用指定选项。 若要导出数据协定，请指定选项。|
|WrnCannotResolveServiceForExport|无法加载具有指定 configName 的服务。 若要导出服务，请同时提供包含服务类型的程序集和具有此服务的配置的可执行文件。|
|ParametersCollectionType|\<type>|
|ErrOptionConflictsWithTarget|不支持将指定的选项与设置为指定值的指定选项一起使用。|
|ErrCodegenError|生成指定语言的代码时出错。<br /><br /> 该语言不支持所有正在生成的代码元素。 应使用另一种语言。|
|ErrInputWsdlParseError|读取指定的内容时发生 WSDL 分析错误。 请验证 XML 是否格式正确且有效。|
|ErrCouldNotCreateInstance|无法创建传递给指定参数的指定类型的实例。|
|ParametersNamespace|\<string,string>|
|HelpNostdlib|不引用标准库（默认情况下，引用 mscorlib.dll 和 system.servicemodel.dll）。|
|WrnCannotLoadConfigFileForExport|处理从指定位置加载的配置文件时出错。 无法加载此配置文件中定义的服务。|
|WrnUnableToLoadContractForExport|加载协定类型时出错。 无法导出该指定类型。|
