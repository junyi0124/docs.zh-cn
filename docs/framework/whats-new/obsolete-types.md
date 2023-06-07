---
title: .NET Framework 中的已过时类型
description: 请参阅 .NET Framework 4.5 和 .NET Framework 4.6 中由程序集组织的已过时类型列表。 建议的替代项也已列出。
ms.date: 03/30/2017
helpviewer_keywords:
- .NET Framework 4.5, obsolete types
- types, obsolete in .NET Framework 4.5
- obsolete types [.NET Framework]
ms.assetid: e636d024-0fac-45eb-b721-25a8c0ceca8f
ms.openlocfilehash: ccf39283152ecdc6b7827f2185de215c17ecc845
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95707910"
---
# <a name="obsolete-types-in-the-net-framework"></a>.NET Framework 中的过时类型

<a name="introduction"></a>本文中的表列出了 .NET Framework 4.5 和 .NET Framework 4.6 中由程序集组织的已过时的类型。 使用以下链接可查看每个程序集中过时的类型和建议的备选项的列表。 由于这些类型已过时，因此其所有成员也已过时。 有关 .NET Framework 类库中其他过时成员的列表，请参阅[过时成员](obsolete-members.md)。

- [系统程序集中的过时类型](#obsolete_types_in_system_assemblies)

  - [mscorlib.dll](#mscorlib)

  - [System.Core.dll](#Core)

  - [System.Data.dll](#data)

  - [System.Data.OracleClient.dll](#oracleclient)

  - [System.Design.dll](#design)

  - [System.dll](#system)

  - [System.EnterpriseServices.dll](#enterpriseservices)

  - [System.Net.dll](#net)

  - [System.ServiceModel.dll](#servicemodel)

  - [System.Web.dll](#web)

  - [System.Web.Mobile.dll](#mobile)

  - [System.Workflow.Activities.dll](#workflow_activities)

  - [System.Workflow.ComponentModel.dll](#workflow_componentmodel)

  - [System.Workflow.Runtime.dll](#workflow_runtime)

  - [System.WorkflowServices.dll](#workflowservices)

  - [System.Xaml.dll](#xaml)

  - [System.Xml.dll](#xml)

  - [WindowsBase.dll](#WindowsBase)

- [Microsoft 程序集中的过时类型](#obsolete_types_in_microsoft_assemblies)

  - [IEHost.dll and IEExec.exe](#IEHost)

  - [Microsoft.Build.Engine.dll](#Engine)

  - [Microsoft.JScript.dll](#jscript)

  - [Microsoft.VisualBasic.Compatibility.dll](#VBCompat)

  - [Microsoft.VisualBasic.Compatibility.Data.dll](#VBCompatData)

  - [Microsoft.VisualC.dll](#visualc)

<a name="obsolete_types_in_system_assemblies"></a>

## <a name="obsolete-types-in-system-assemblies"></a>系统程序集中的过时类型

以下各表列出了系统程序集中已声明为过时的类型。 这些程序集用于面向 .NET Framework 的通用\-应用程序开发。

<a name="mscorlib"></a>

### <a name="assembly-mscorlibdll"></a>程序集：mscorlib.dll

|类型|消息|
|----------|-------------|
|<xref:System.ExecutionEngineException?displayProperty=nameWithType>|该类型以前指示运行时中未指定的错误。 由于运行时不再引发此异常，因此该类型已过时。|
|<xref:System.Collections.CaseInsensitiveHashCodeProvider?displayProperty=nameWithType>|请改用 <xref:System.StringComparer?displayProperty=nameWithType>。|
|<xref:System.Collections.IHashCodeProvider?displayProperty=nameWithType>|请改用 <xref:System.Collections.IEqualityComparer?displayProperty=nameWithType>。|
|<xref:System.Configuration.Assemblies.AssemblyHash?displayProperty=nameWithType>|<xref:System.Configuration.Assemblies.AssemblyHash> 类已弃用。|
|<xref:System.Diagnostics.Contracts.Internal.ContractHelper?displayProperty=nameWithType>|在 .NET Framework 4.5 中首次被废弃。 请改用 System.Runtime.CompilerServices 命名空间中的 <xref:System.Runtime.CompilerServices.ContractHelper?displayProperty=nameWithType> 类。|
|<xref:System.Reflection.Emit.UnmanagedMarshal?displayProperty=nameWithType>|有一个可用的备用 API：发出 <xref:System.Runtime.InteropServices.MarshalAsAttribute?displayProperty=nameWithType> 自定义特性。|
|<xref:System.Runtime.InteropServices.BIND_OPTS?displayProperty=nameWithType>|请改用 <xref:System.Runtime.InteropServices.ComTypes.BIND_OPTS?displayProperty=nameWithType>。|
|<xref:System.Runtime.InteropServices.BINDPTR?displayProperty=nameWithType>|请改用 <xref:System.Runtime.InteropServices.ComTypes.BINDPTR?displayProperty=nameWithType>。|
|<xref:System.Runtime.InteropServices.CALLCONV?displayProperty=nameWithType>|请改用 <xref:System.Runtime.InteropServices.ComTypes.CALLCONV?displayProperty=nameWithType>。|
|<xref:System.Runtime.InteropServices.CONNECTDATA?displayProperty=nameWithType>|请改用 <xref:System.Runtime.InteropServices.ComTypes.CONNECTDATA?displayProperty=nameWithType>。|
|<xref:System.Runtime.InteropServices.DESCKIND?displayProperty=nameWithType>|请改用 <xref:System.Runtime.InteropServices.ComTypes.DESCKIND?displayProperty=nameWithType>。|
|<xref:System.Runtime.InteropServices.DISPPARAMS?displayProperty=nameWithType>|请改用 <xref:System.Runtime.InteropServices.ComTypes.DISPPARAMS?displayProperty=nameWithType>。|
|<xref:System.Runtime.InteropServices.ELEMDESC?displayProperty=nameWithType>|请改用 <xref:System.Runtime.InteropServices.ComTypes.ELEMDESC?displayProperty=nameWithType>。|
|<xref:System.Runtime.InteropServices.EXCEPINFO?displayProperty=nameWithType>|请改用 <xref:System.Runtime.InteropServices.ComTypes.EXCEPINFO?displayProperty=nameWithType>。|
|<xref:System.Runtime.InteropServices.FILETIME?displayProperty=nameWithType>|请改用 <xref:System.Runtime.InteropServices.ComTypes.FILETIME?displayProperty=nameWithType>。|
|<xref:System.Runtime.InteropServices.FUNCDESC?displayProperty=nameWithType>|请改用 <xref:System.Runtime.InteropServices.ComTypes.FUNCDESC?displayProperty=nameWithType>。|
|<xref:System.Runtime.InteropServices.FUNCFLAGS?displayProperty=nameWithType>|请改用 <xref:System.Runtime.InteropServices.ComTypes.FUNCFLAGS?displayProperty=nameWithType>。|
|<xref:System.Runtime.InteropServices.FUNCKIND?displayProperty=nameWithType>|请改用 <xref:System.Runtime.InteropServices.ComTypes.FUNCKIND?displayProperty=nameWithType>。|
|<xref:System.Runtime.InteropServices.IDispatchImplAttribute?displayProperty=nameWithType>|此特性已弃用，并将从未来版本中移除。|
|<xref:System.Runtime.InteropServices.IDispatchImplType?displayProperty=nameWithType>|<xref:System.Runtime.InteropServices.IDispatchImplAttribute?displayProperty=nameWithType> 已弃用。|
|<xref:System.Runtime.InteropServices.IDLDESC?displayProperty=nameWithType>|请改用 <xref:System.Runtime.InteropServices.ComTypes.IDLDESC?displayProperty=nameWithType>。|
|<xref:System.Runtime.InteropServices.IDLFLAG?displayProperty=nameWithType>|请改用 <xref:System.Runtime.InteropServices.ComTypes.IDLFLAG?displayProperty=nameWithType>。|
|<xref:System.Runtime.InteropServices.IMPLTYPEFLAGS?displayProperty=nameWithType>|请改用 <xref:System.Runtime.InteropServices.ComTypes.IMPLTYPEFLAGS?displayProperty=nameWithType>。|
|<xref:System.Runtime.InteropServices.INVOKEKIND?displayProperty=nameWithType>|请改用 <xref:System.Runtime.InteropServices.ComTypes.INVOKEKIND?displayProperty=nameWithType>。|
|<xref:System.Runtime.InteropServices.LIBFLAGS?displayProperty=nameWithType>|请改用 <xref:System.Runtime.InteropServices.ComTypes.LIBFLAGS?displayProperty=nameWithType>。|
|<xref:System.Runtime.InteropServices.PARAMDESC?displayProperty=nameWithType>|请改用 <xref:System.Runtime.InteropServices.ComTypes.PARAMDESC?displayProperty=nameWithType>。|
|<xref:System.Runtime.InteropServices.PARAMFLAG?displayProperty=nameWithType>|请改用 <xref:System.Runtime.InteropServices.ComTypes.PARAMFLAG?displayProperty=nameWithType>。|
|<xref:System.Runtime.InteropServices.SetWin32ContextInIDispatchAttribute?displayProperty=nameWithType>|此特性已弃用。 应用程序域不再考虑 IDispatch 调用中的激活上下文边界。|
|<xref:System.Runtime.InteropServices.STATSTG?displayProperty=nameWithType>|请改用 <xref:System.Runtime.InteropServices.ComTypes.STATSTG?displayProperty=nameWithType>。|
|<xref:System.Runtime.InteropServices.SYSKIND?displayProperty=nameWithType>|请改用 <xref:System.Runtime.InteropServices.ComTypes.SYSKIND?displayProperty=nameWithType>。|
|<xref:System.Runtime.InteropServices.TYPEATTR?displayProperty=nameWithType>|请改用 <xref:System.Runtime.InteropServices.ComTypes.TYPEATTR?displayProperty=nameWithType>。|
|<xref:System.Runtime.InteropServices.TYPEDESC?displayProperty=nameWithType>|请改用 <xref:System.Runtime.InteropServices.ComTypes.TYPEDESC?displayProperty=nameWithType>。|
|<xref:System.Runtime.InteropServices.TYPEFLAGS?displayProperty=nameWithType>|请改用 <xref:System.Runtime.InteropServices.ComTypes.TYPEFLAGS?displayProperty=nameWithType>。|
|<xref:System.Runtime.InteropServices.TYPEKIND?displayProperty=nameWithType>|请改用 <xref:System.Runtime.InteropServices.ComTypes.TYPEKIND?displayProperty=nameWithType>。|
|<xref:System.Runtime.InteropServices.TYPELIBATTR?displayProperty=nameWithType>|请改用 <xref:System.Runtime.InteropServices.ComTypes.TYPELIBATTR?displayProperty=nameWithType>。|
|<xref:System.Runtime.InteropServices.UCOMIBindCtx?displayProperty=nameWithType>|请改用 <xref:System.Runtime.InteropServices.ComTypes.IBindCtx?displayProperty=nameWithType>。|
|<xref:System.Runtime.InteropServices.UCOMIConnectionPoint?displayProperty=nameWithType>|请改用 <xref:System.Runtime.InteropServices.ComTypes.IConnectionPoint?displayProperty=nameWithType>。|
|<xref:System.Runtime.InteropServices.UCOMIConnectionPointContainer?displayProperty=nameWithType>|请改用 <xref:System.Runtime.InteropServices.ComTypes.IConnectionPointContainer?displayProperty=nameWithType>。|
|<xref:System.Runtime.InteropServices.UCOMIEnumConnectionPoints?displayProperty=nameWithType>|请改用 <xref:System.Runtime.InteropServices.ComTypes.IEnumConnectionPoints?displayProperty=nameWithType>。|
|<xref:System.Runtime.InteropServices.UCOMIEnumConnections?displayProperty=nameWithType>|请改用 <xref:System.Runtime.InteropServices.ComTypes.IEnumConnections?displayProperty=nameWithType>。|
|<xref:System.Runtime.InteropServices.UCOMIEnumMoniker?displayProperty=nameWithType>|请改用 <xref:System.Runtime.InteropServices.ComTypes.IEnumMoniker?displayProperty=nameWithType>。|
|<xref:System.Runtime.InteropServices.UCOMIEnumString?displayProperty=nameWithType>|请改用 <xref:System.Runtime.InteropServices.ComTypes.IEnumString?displayProperty=nameWithType>。|
|<xref:System.Runtime.InteropServices.UCOMIEnumVARIANT?displayProperty=nameWithType>|请改用 <xref:System.Runtime.InteropServices.ComTypes.IEnumVARIANT?displayProperty=nameWithType>。|
|<xref:System.Runtime.InteropServices.UCOMIMoniker?displayProperty=nameWithType>|请改用 <xref:System.Runtime.InteropServices.ComTypes.IMoniker?displayProperty=nameWithType>。|
|<xref:System.Runtime.InteropServices.UCOMIPersistFile?displayProperty=nameWithType>|请改用 <xref:System.Runtime.InteropServices.ComTypes.IPersistFile?displayProperty=nameWithType>。|
|<xref:System.Runtime.InteropServices.UCOMIRunningObjectTable?displayProperty=nameWithType>|请改用 <xref:System.Runtime.InteropServices.ComTypes.IRunningObjectTable?displayProperty=nameWithType>。|
|<xref:System.Runtime.InteropServices.UCOMIStream?displayProperty=nameWithType>|请改用 <xref:System.Runtime.InteropServices.ComTypes.IStream?displayProperty=nameWithType>。|
|<xref:System.Runtime.InteropServices.UCOMITypeComp?displayProperty=nameWithType>|请改用 <xref:System.Runtime.InteropServices.ComTypes.ITypeComp?displayProperty=nameWithType>。|
|<xref:System.Runtime.InteropServices.UCOMITypeInfo?displayProperty=nameWithType>|请改用 <xref:System.Runtime.InteropServices.ComTypes.ITypeInfo?displayProperty=nameWithType>。|
|<xref:System.Runtime.InteropServices.UCOMITypeLib?displayProperty=nameWithType>|请改用 <xref:System.Runtime.InteropServices.ComTypes.ITypeLib?displayProperty=nameWithType>。|
|<xref:System.Runtime.InteropServices.VARDESC?displayProperty=nameWithType>|请改用 <xref:System.Runtime.InteropServices.ComTypes.VARDESC?displayProperty=nameWithType>。|
|<xref:System.Runtime.InteropServices.VARFLAGS?displayProperty=nameWithType>|请改用 <xref:System.Runtime.InteropServices.ComTypes.VARFLAGS?displayProperty=nameWithType>。|
|<xref:System.Security.SecurityCriticalScope?displayProperty=nameWithType>|<xref:System.Security.SecurityCriticalScope> 仅用于实现 .NET Framework 2.0 透明度兼容性。|
|<xref:System.Security.SecurityTreatAsSafeAttribute?displayProperty=nameWithType>|<xref:System.Security.SecurityTreatAsSafeAttribute> 仅用于实现 .NET Framework 2.0 透明度兼容性。 请改用 <xref:System.Security.SecuritySafeCriticalAttribute?displayProperty=nameWithType>。|
|<xref:System.Security.Policy.FirstMatchCodeGroup?displayProperty=nameWithType>|此类型已过时，并将从 .NET Framework 的未来版本中移除。|
|<xref:System.Security.Policy.PermissionRequestEvidence?displayProperty=nameWithType>|程序集级别声明性安全已过时，默认情况下不再由 CLR 强制实施。|
|<xref:System.Security.Policy.UnionCodeGroup?displayProperty=nameWithType>|此类型已过时，并将从 .NET Framework 的未来版本中移除。|

[返回页首](#introduction)

<a name="Core"></a>

### <a name="assembly-systemcoredll"></a>程序集：System.Core.dll

|类型|消息|
|----------|-------------|
|<xref:System.Runtime.CompilerServices.ExecutionScope?displayProperty=nameWithType>|使用此类型会生成编译器错误。<br /><br /> 不要使用此类型。|

[返回页首](#introduction)

<a name="data"></a>

### <a name="assembly-systemdatadll"></a>程序集：System.Data.dll

|类型|消息|
|----------|-------------|
|<xref:System.Data.DataSysDescriptionAttribute?displayProperty=nameWithType>|<xref:System.Data.DataSysDescriptionAttribute> 已弃用。|
|<xref:System.Data.PropertyAttributes?displayProperty=nameWithType>|<xref:System.Data.PropertyAttributes> 已弃用。|
|<xref:System.Data.TypedDataSetGenerator?displayProperty=nameWithType>|将从未来版本中移除 <xref:System.Data.TypedDataSetGenerator> 类。 请使用 System.Design.dll 中的 <xref:System.Data.Design.TypedDataSetGenerator?displayProperty=nameWithType>。|
|<xref:System.Xml.XmlDataDocument?displayProperty=nameWithType>|将从未来版本中移除 <xref:System.Xml.XmlDataDocument> 类。|

[返回页首](#introduction)

<a name="oracleclient"></a>

### <a name="assembly-systemdataoracleclientdll"></a>程序集：System.Data.OracleClient.dll

|类型|消息|
|----------|-------------|
|<xref:System.Data.OracleClient.OracleClientFactory?displayProperty=nameWithType>|<xref:System.Data.OracleClient.OracleClientFactory> 已弃用。|
|<xref:System.Data.OracleClient.OracleCommand?displayProperty=nameWithType>|<xref:System.Data.OracleClient.OracleCommand> 已弃用。|
|<xref:System.Data.OracleClient.OracleCommandBuilder?displayProperty=nameWithType>|<xref:System.Data.OracleClient.OracleCommandBuilder> 已弃用。|
|<xref:System.Data.OracleClient.OracleConnection?displayProperty=nameWithType>|<xref:System.Data.OracleClient.OracleConnection> 已弃用。|
|<xref:System.Data.OracleClient.OracleConnectionStringBuilder?displayProperty=nameWithType>|<xref:System.Data.OracleClient.OracleConnectionStringBuilder> 已弃用。|
|<xref:System.Data.OracleClient.OracleDataAdapter?displayProperty=nameWithType>|<xref:System.Data.OracleClient.OracleDataAdapter> 已弃用。|
|<xref:System.Data.OracleClient.OraclePermission?displayProperty=nameWithType>|<xref:System.Data.OracleClient.OraclePermission> 已弃用。|
|<xref:System.Data.OracleClient.OraclePermissionAttribute?displayProperty=nameWithType>|<xref:System.Data.OracleClient.OraclePermissionAttribute?displayProperty=nameWithType> 已弃用。|

[返回页首](#introduction)

<a name="design"></a>

### <a name="assembly-systemdesigndll"></a>程序集：System.Design.dll

|类型|消息|
|----------|-------------|
|<xref:System.ComponentModel.Design.LocalizationExtenderProvider?displayProperty=nameWithType>|此类已弃用。 请改用 <xref:System.ComponentModel.Design.Serialization.CodeDomLocalizationProvider?displayProperty=nameWithType>。|
|<xref:System.Web.UI.Design.DataBindingCollectionConverter?displayProperty=nameWithType>|建议不要使用此类型，因为 DataBindings 编辑是通过 <xref:System.ComponentModel.Design.DesignerActionList?displayProperty=nameWithType> 而不是属性网格启动的。|
|<xref:System.Web.UI.Design.DataBindingCollectionEditor?displayProperty=nameWithType>|建议不要使用此类型，因为 DataBindings 编辑是通过 <xref:System.ComponentModel.Design.DesignerActionList?displayProperty=nameWithType> 而不是属性网格启动的。|
|<xref:System.Web.UI.Design.IControlDesignerBehavior?displayProperty=nameWithType>|建议的替代项为 <xref:System.Web.UI.Design.IControlDesignerTag?displayProperty=nameWithType> 和 <xref:System.Web.UI.Design.IControlDesignerView?displayProperty=nameWithType>。|
|<xref:System.Web.UI.Design.IHtmlControlDesignerBehavior?displayProperty=nameWithType>|建议的替代项为 <xref:System.Web.UI.Design.IControlDesignerTag?displayProperty=nameWithType> 和 <xref:System.Web.UI.Design.IControlDesignerView?displayProperty=nameWithType>。|
|<xref:System.Web.UI.Design.ITemplateEditingFrame?displayProperty=nameWithType>|建议不要使用此类型，因为模板编辑是在 <xref:System.Web.UI.Design.ControlDesigner?displayProperty=nameWithType> 中处理的。 若要支持模板编辑，请在 <xref:System.Web.UI.Design.ControlDesigner.TemplateGroups%2A?displayProperty=nameWithType> 属性中公开模板数据，并调用 <xref:System.Web.UI.Design.ControlDesigner.SetViewFlags%2A?displayProperty=nameWithType>。|
|<xref:System.Web.UI.Design.IWebFormReferenceManager?displayProperty=nameWithType>|建议的替代项为 <xref:System.Web.UI.Design.WebFormsReferenceManager?displayProperty=nameWithType>。 <xref:System.Web.UI.Design.WebFormsReferenceManager> 包含其他功能并允许更大的扩展性。 若要获取 <xref:System.Web.UI.Design.WebFormsReferenceManager>，请使用 `RootDesigner.ReferenceManager` 中的 <xref:System.Web.UI.Design.ControlDesigner?displayProperty=nameWithType> 属性。|
|<xref:System.Web.UI.Design.IWebFormsDocumentService?displayProperty=nameWithType>|建议的替代项为 <xref:System.Web.UI.Design.WebFormsRootDesigner?displayProperty=nameWithType>。 <xref:System.Web.UI.Design.WebFormsRootDesigner> 包含其他功能并允许更大的扩展性。 若要获取 <xref:System.Web.UI.Design.WebFormsRootDesigner>，请使用 <xref:System.Web.UI.Design.ControlDesigner.RootDesigner%2A> 中的 <xref:System.Web.UI.Design.ControlDesigner?displayProperty=nameWithType> 属性。|
|<xref:System.Web.UI.Design.ITemplateEditingService?displayProperty=nameWithType>|建议不要使用此类型，因为模板编辑是在 <xref:System.Web.UI.Design.ControlDesigner?displayProperty=nameWithType> 中处理的。 若要支持模板编辑，请在 <xref:System.Web.UI.Design.ControlDesigner.TemplateGroups%2A?displayProperty=nameWithType> 属性中公开模板数据，并调用 <xref:System.Web.UI.Design.ControlDesigner.SetViewFlags%2A?displayProperty=nameWithType>。|
|<xref:System.Web.UI.Design.ReadWriteControlDesigner?displayProperty=nameWithType>|建议的替代项为 <xref:System.Web.UI.Design.ContainerControlDesigner?displayProperty=nameWithType>，因为它使用 <xref:System.Web.UI.Design.EditableDesignerRegion?displayProperty=nameWithType> 来编辑内容。 利用设计器区域，可更好地控制正在编辑的内容。|
|<xref:System.Web.UI.Design.TemplateEditingService?displayProperty=nameWithType>|建议不要使用此类型，因为模板编辑是在 <xref:System.Web.UI.Design.ControlDesigner?displayProperty=nameWithType> 中处理的。 若要支持模板编辑，请在 <xref:System.Web.UI.Design.ControlDesigner.TemplateGroups%2A?displayProperty=nameWithType> 属性中公开模板数据，并调用 <xref:System.Web.UI.Design.ControlDesigner.SetViewFlags%2A?displayProperty=nameWithType>。|
|<xref:System.Web.UI.Design.TemplateEditingVerb?displayProperty=nameWithType>|建议不要使用此类型，因为模板编辑是在 <xref:System.Web.UI.Design.ControlDesigner?displayProperty=nameWithType> 中处理的。 若要支持模板编辑，请在 <xref:System.Web.UI.Design.ControlDesigner.TemplateGroups%2A?displayProperty=nameWithType> 属性中公开模板数据，并调用 <xref:System.Web.UI.Design.ControlDesigner.SetViewFlags%2A?displayProperty=nameWithType>。|
|<xref:System.Web.UI.Design.WebControls.CalendarAutoFormatDialog?displayProperty=nameWithType>|建议不要使用此类型，因为 AutoFormat 对话框是由设计器宿主启动的。 <xref:System.Web.UI.Design.ControlDesigner?displayProperty=nameWithType> 属性中的 <xref:System.Web.UI.Design.ControlDesigner.AutoFormats%2A?displayProperty=nameWithType> 上公开了可用的 AutoFormat 的列表。|
|<xref:System.Web.UI.Design.WebControls.PanelDesigner?displayProperty=nameWithType>|建议的替代项为 <xref:System.Web.UI.Design.WebControls.PanelContainerDesigner?displayProperty=nameWithType>，因为它使用 <xref:System.Web.UI.Design.EditableDesignerRegion?displayProperty=nameWithType> 来编辑内容。 利用设计器区域，可更好地控制正在编辑的内容。|

[返回页首](#introduction)

<a name="system"></a>

### <a name="assembly-systemdll"></a>程序集：System.dll

|类型|消息|
|----------|-------------|
|<xref:System.ComponentModel.IComNativeDescriptorHandler?displayProperty=nameWithType>|此接口已弃用。 改为将 <xref:System.ComponentModel.TypeDescriptionProvider?displayProperty=nameWithType> 添加到句柄类型 <xref:System.ComponentModel.TypeDescriptor.ComObjectType%2A?displayProperty=nameWithType>。|
|<xref:System.ComponentModel.RecommendedAsConfigurableAttribute?displayProperty=nameWithType>|改用 <xref:System.ComponentModel.SettingsBindableAttribute?displayProperty=nameWithType> 来处理新的设置模型。|
|<xref:System.ComponentModel.Design.Serialization.RootDesignerSerializerAttribute?displayProperty=nameWithType>|此特性已弃用。 请改用 <xref:System.ComponentModel.Design.Serialization.DesignerSerializerAttribute?displayProperty=nameWithType>。|
|<xref:System.Diagnostics.DiagnosticsConfigurationHandler?displayProperty=nameWithType>|此类已弃用。|
|<xref:System.Diagnostics.PerformanceCounterManager?displayProperty=nameWithType>|此类已弃用。 改为通过 <xref:System.Diagnostics.PerformanceCounter?displayProperty=nameWithType> 类使用性能计数器。|
|<xref:System.Net.GlobalProxySelection?displayProperty=nameWithType>|此类已弃用。 请改用 <xref:System.Net.WebRequest.DefaultWebProxy%2A?displayProperty=nameWithType> 以访问和设置全局默认代理。 使用“null”而非 <xref:System.Net.GlobalProxySelection.GetEmptyWebProxy%2A?displayProperty=nameWithType>。|
|<xref:System.Net.Sockets.SocketClientAccessPolicyProtocol?displayProperty=nameWithType>|在 .NET Framework 4.5 中首次被废弃。<br /><br /> 使用此类型会生成编译器错误。<br /><br /> 此 API 支持 .NET Framework 基础结构，但不应在代码中直接使用。|

[返回页首](#introduction)

<a name="enterpriseservices"></a>

### <a name="assembly-systementerpriseservicesdll"></a>程序集：System.EnterpriseServices.dll

|类型|消息|
|----------|-------------|
|<xref:System.EnterpriseServices.RegistrationHelperTx?displayProperty=nameWithType>|<xref:System.EnterpriseServices.RegistrationHelperTx> 类已弃用。|

[返回页首](#introduction)

<a name="net"></a>

### <a name="assembly-systemnetdll"></a>程序集：System.Net.dll

|类型|消息|
|----------|-------------|
|<xref:System.Net.INetworkProgress?displayProperty=nameWithType>|在 .NET Framework 4.5 中首次被废弃。<br /><br /> 使用此类型会生成编译器错误。<br /><br /> 此 API 支持 .NET Framework 基础结构，但不应在代码中直接使用。|
|<xref:System.Net.IUnsafeWebRequestCreate?displayProperty=nameWithType>|在 .NET Framework 4.5 中首次被废弃。<br /><br /> 使用此类型会生成编译器错误。<br /><br /> 此 API 支持 .NET Framework 基础结构，但不应在代码中直接使用。|
|<xref:System.Net.NetworkProgressChangedEventArgs?displayProperty=nameWithType>|在 .NET Framework 4.5 中首次被废弃。<br /><br /> 使用此类型会生成编译器错误。<br /><br /> 此 API 支持 .NET Framework 基础结构，但不应在代码中直接使用。|
|<xref:System.Net.UiSynchronizationContext?displayProperty=nameWithType>|在 .NET Framework 4.5 中首次被废弃。<br /><br /> 使用此类型会生成编译器错误。<br /><br /> 此 API 支持 .NET Framework 基础结构，但不应在代码中直接使用。|
|<xref:System.Net.Sockets.HttpPolicyDownloaderProtocol?displayProperty=nameWithType>|在 .NET Framework 4.5 中首次被废弃。<br /><br /> 使用此类型会生成编译器错误。<br /><br /> 此 API 支持 .NET Framework 基础结构，但不应在代码中直接使用。|
|<xref:System.Net.Sockets.SecurityCriticalAction?displayProperty=nameWithType>|在 .NET Framework 4.5 中首次被废弃。<br /><br /> 使用此类型会生成编译器错误。<br /><br /> 此 API 支持 .NET Framework 基础结构，但不应在代码中直接使用。|
|<xref:System.Net.Sockets.SocketPolicy?displayProperty=nameWithType>|在 .NET Framework 4.5 中首次被废弃。<br /><br /> 使用此类型会生成编译器错误。<br /><br /> 此 API 支持 .NET Framework 基础结构，但不应在代码中直接使用。|
|<xref:System.Net.Sockets.UdpAnySourceMulticastClient?displayProperty=nameWithType>|在 .NET Framework 4.5 中首次被废弃。<br /><br /> 使用此类型会生成编译器错误。<br /><br /> 此 API 支持 .NET Framework 基础结构，但不应在代码中直接使用。|
|<xref:System.Net.Sockets.UdpSingleSourceMulticastClient?displayProperty=nameWithType>|在 .NET Framework 4.5 中首次被废弃。<br /><br /> 使用此类型会生成编译器错误。<br /><br /> 此 API 支持 .NET Framework 基础结构，但不应在代码中直接使用。|

[返回页首](#introduction)

<a name="servicemodel"></a>

### <a name="assembly-systemservicemodeldll"></a>程序集：System.ServiceModel.dll

|类型|消息|
|----------|-------------|
|<xref:System.ServiceModel.NetPeerTcpBinding?displayProperty=nameWithType>|在 .NET Framework 4.5 中首次被废弃。<br /><br /> 对等通道功能已过时，并将在以后删除。|
|<xref:System.ServiceModel.Channels.HttpCookieContainerBindingElement?displayProperty=nameWithType>|在 .NET Framework 4.5 中首次被废弃。<br /><br /> 此类型已过时。 若要启用 Http <xref:System.Net.CookieContainer>，请在 Http 绑定或 `AllowCookies` 上使用 <xref:System.ServiceModel.Channels.HttpTransportBindingElement> 属性。|
|<xref:System.ServiceModel.Channels.PeerCustomResolverBindingElement?displayProperty=nameWithType>|在 .NET Framework 4.5 中首次被废弃。<br /><br /> 对等通道功能已过时，并将在以后删除。|
|<xref:System.ServiceModel.Channels.PeerTransportBindingElement?displayProperty=nameWithType>|在 .NET Framework 4.5 中首次被废弃。<br /><br /> 对等通道功能已过时，并将在以后删除。|
|<xref:System.ServiceModel.Configuration.NetPeerTcpBindingCollectionElement?displayProperty=nameWithType>|在 .NET Framework 4.5 中首次被废弃。<br /><br /> 对等通道功能已过时，并将在以后删除。|
|<xref:System.ServiceModel.Configuration.NetPeerTcpBindingElement?displayProperty=nameWithType>|在 .NET Framework 4.5 中首次被废弃。<br /><br /> 对等通道功能已过时，并将在以后删除。|
|<xref:System.ServiceModel.Configuration.PeerTransportElement?displayProperty=nameWithType>|在 .NET Framework 4.5 中首次被废弃。<br /><br /> 对等通道功能已过时，并将在以后删除。|
|<xref:System.ServiceModel.PeerResolvers.CustomPeerResolverService?displayProperty=nameWithType>|在 .NET Framework 4.5 中首次被废弃。<br /><br /> 对等通道功能已过时，并将在以后删除。|

[返回页首](#introduction)

<a name="web"></a>

### <a name="assembly-systemwebdll"></a>程序集：System.Web.dll

|类型|消息|
|----------|-------------|
|<xref:System.Web.Configuration.PassportAuthentication?displayProperty=nameWithType>|此类型已过时。 Passport 身份验证产品不再受支持，并且已由 [Microsoft 帐户](https://account.microsoft.com/account/Account?destrt=home-index)取代。|
|<xref:System.Web.Mail.MailAttachment?displayProperty=nameWithType>|建议的替代项为 <xref:System.Net.Mail.Attachment?displayProperty=nameWithType>。|
|<xref:System.Web.Mail.MailEncoding?displayProperty=nameWithType>|建议的替代项为 <xref:System.Net.Mime.TransferEncoding?displayProperty=nameWithType>。|
|<xref:System.Web.Mail.MailFormat?displayProperty=nameWithType>|建议的替代项为 <xref:System.Net.Mail.MailMessage.IsBodyHtml%2A?displayProperty=nameWithType>。|
|<xref:System.Web.Mail.MailMessage?displayProperty=nameWithType>|建议的替代项为 <xref:System.Net.Mail.MailMessage?displayProperty=nameWithType>。|
|<xref:System.Web.Mail.MailPriority?displayProperty=nameWithType>|建议的替代项为 <xref:System.Net.Mail.MailPriority?displayProperty=nameWithType>。|
|<xref:System.Web.Mail.SmtpMail?displayProperty=nameWithType>|建议的替代项为 <xref:System.Net.Mail.SmtpClient?displayProperty=nameWithType>。|
|<xref:System.Web.Security.PassportAuthenticationEventArgs?displayProperty=nameWithType>|此类型已过时。 Passport 身份验证产品不再受支持，并且已由 [Microsoft 帐户](https://account.microsoft.com/account/Account?destrt=home-index)取代。|
|<xref:System.Web.Security.PassportAuthenticationEventHandler?displayProperty=nameWithType>|此类型已过时。 Passport 身份验证产品不再受支持，并且已由 [Microsoft 帐户](https://account.microsoft.com/account/Account?destrt=home-index)取代。|
|<xref:System.Web.Security.PassportAuthenticationModule?displayProperty=nameWithType>|此类型已过时。 Passport 身份验证产品不再受支持，并且已由 [Microsoft 帐户](https://account.microsoft.com/account/Account?destrt=home-index)取代。|
|<xref:System.Web.Security.PassportIdentity?displayProperty=nameWithType>|此类型已过时。 Passport 身份验证产品不再受支持，并且已由 [Microsoft 帐户](https://account.microsoft.com/account/Account?destrt=home-index)取代。|
|<xref:System.Web.Security.PassportPrincipal?displayProperty=nameWithType>|此类型已过时。 Passport 身份验证产品不再受支持，并且已由 [Microsoft 帐户](https://account.microsoft.com/account/Account?destrt=home-index)取代。|
|<xref:System.Web.UI.ObjectConverter?displayProperty=nameWithType>|建议的替代项为 <xref:System.Convert?displayProperty=nameWithType> 和 <xref:System.String.Format%2A?displayProperty=nameWithType>。|

[返回页首](#introduction)

<a name="mobile"></a>

### <a name="assembly-systemwebmobiledll"></a>程序集：System.Web.Mobile.dll

|类型|消息|
|----------|-------------|
|<xref:System.Web.Mobile.CookielessData?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.Mobile.DeviceFilterElement?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.Mobile.DeviceFilterElementCollection?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.Mobile.DeviceFiltersSection?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.Mobile.ErrorHandlerModule?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.Mobile.MobileCapabilities?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.Mobile.MobileDeviceCapabilitiesSectionHandler?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.Mobile.MobileErrorInfo?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.Mobile.MobileFormsAuthentication?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.Design.MobileControls.IMobileDesigner?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.Design.MobileControls.IMobileWebFormServices?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.Design.MobileControls.MobileResource?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.Design.MobileControls.Converters.DataFieldConverter?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.Design.MobileControls.Converters.DataMemberConverter?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.AdRotator?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.Alignment?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.ArrayListCollectionBase?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.BaseValidator?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.BooleanOption?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.Calendar?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.Command?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.CommandFormat?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.CompareValidator?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.Constants?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.ControlElement?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.ControlElementCollection?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.ControlPager?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.CustomValidator?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.DesignerAdapterAttribute?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.DeviceElement?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.DeviceElementCollection?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.DeviceOverridableAttribute?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.DeviceSpecific?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.DeviceSpecificChoice?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.DeviceSpecificChoiceCollection?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.DeviceSpecificChoiceControlBuilder?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.DeviceSpecificChoiceTemplateBuilder?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.DeviceSpecificChoiceTemplateContainer?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.DeviceSpecificControlBuilder?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.ErrorFormatterPage?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.FontInfo?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.FontSize?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.Form?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.FormControlBuilder?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.FormMethod?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.IControlAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.Image?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.IObjectListFieldCollection?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.IPageAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.ItemPager?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.ITemplateable?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.Label?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.Link?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.List?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.ListCommandEventArgs?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.ListCommandEventHandler?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.ListControlBuilder?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.ListDataBindEventArgs?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.ListDataBindEventHandler?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.ListDecoration?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.ListSelectType?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.LiteralLink?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.LiteralText?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.LiteralTextContainerControlBuilder?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.LiteralTextControlBuilder?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.LoadItemsEventArgs?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.LoadItemsEventHandler?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.MobileControl?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.MobileControlBuilder?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.MobileControlsSection?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.MobileControlsSectionHandler?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.MobileListItem?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.MobileListItemCollection?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.MobileListItemType?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.MobilePage?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.MobileTypeNameConverter?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.MobileUserControl?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.ObjectList?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.ObjectListCommand?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.ObjectListCommandCollection?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.ObjectListCommandEventArgs?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.ObjectListCommandEventHandler?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.ObjectListControlBuilder?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.ObjectListDataBindEventArgs?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.ObjectListDataBindEventHandler?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.ObjectListField?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.ObjectListFieldCollection?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.ObjectListItem?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.ObjectListItemCollection?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.ObjectListSelectEventArgs?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.ObjectListSelectEventHandler?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.ObjectListShowCommandsEventArgs?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.ObjectListShowCommandsEventHandler?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.ObjectListTitleAttribute?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.ObjectListViewMode?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.PagedControl?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.PagerStyle?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.Panel?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.PanelControlBuilder?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.PersistNameAttribute?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.PhoneCall?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.RangeValidator?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.RegularExpressionValidator?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.RequiredFieldValidator?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.SelectionList?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.Style?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.StyleSheet?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.StyleSheetControlBuilder?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.TemplateContainer?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.TextBox?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.TextBoxControlBuilder?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.TextControl?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.TextView?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.TextViewElement?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.ValidationSummary?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.Wrapping?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.Adapters.ChtmlCalendarAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.Adapters.ChtmlCommandAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.Adapters.ChtmlFormAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.Adapters.ChtmlImageAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.Adapters.ChtmlLinkAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.Adapters.ChtmlMobileTextWriter?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.Adapters.ChtmlPageAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.Adapters.ChtmlPhoneCallAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.Adapters.ChtmlSelectionListAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.Adapters.ChtmlTextBoxAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.Adapters.ControlAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.Adapters.HtmlCalendarAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.Adapters.HtmlCommandAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.Adapters.HtmlControlAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.Adapters.HtmlFormAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.Adapters.HtmlImageAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.Adapters.HtmlLabelAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.Adapters.HtmlLinkAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.Adapters.HtmlListAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.Adapters.HtmlLiteralTextAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.Adapters.HtmlMobileTextWriter?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.Adapters.HtmlObjectListAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.Adapters.HtmlPageAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.Adapters.HtmlPanelAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.Adapters.HtmlPhoneCallAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.Adapters.HtmlSelectionListAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.Adapters.HtmlTextBoxAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.Adapters.HtmlTextViewAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.Adapters.HtmlValidationSummaryAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.Adapters.HtmlValidatorAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.Adapters.MobileTextWriter?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.Adapters.MultiPartWriter?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.Adapters.UpWmlMobileTextWriter?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.Adapters.UpWmlPageAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.Adapters.WmlCalendarAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.Adapters.WmlCommandAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.Adapters.WmlControlAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.Adapters.WmlFormAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.Adapters.WmlImageAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.Adapters.WmlLabelAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.Adapters.WmlLinkAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.Adapters.WmlListAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.Adapters.WmlLiteralTextAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.Adapters.WmlMobileTextWriter?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.Adapters.WmlObjectListAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.Adapters.WmlPageAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.Adapters.WmlPanelAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.Adapters.WmlPhoneCallAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.Adapters.WmlPostFieldType?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.Adapters.WmlSelectionListAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.Adapters.WmlTextBoxAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.Adapters.WmlTextViewAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.Adapters.WmlValidationSummaryAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.Adapters.WmlValidatorAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.Adapters.XhtmlAdapters.Doctype?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.Adapters.XhtmlAdapters.StyleSheetLocation?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.Adapters.XhtmlAdapters.XhtmlCalendarAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.Adapters.XhtmlAdapters.XhtmlCommandAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.Adapters.XhtmlAdapters.XhtmlControlAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.Adapters.XhtmlAdapters.XhtmlCssHandler?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.Adapters.XhtmlAdapters.XhtmlFormAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.Adapters.XhtmlAdapters.XhtmlImageAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.Adapters.XhtmlAdapters.XhtmlLabelAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.Adapters.XhtmlAdapters.XhtmlLinkAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.Adapters.XhtmlAdapters.XhtmlListAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.Adapters.XhtmlAdapters.XhtmlLiteralTextAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.Adapters.XhtmlAdapters.XhtmlMobileTextWriter?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.Adapters.XhtmlAdapters.XhtmlObjectListAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.Adapters.XhtmlAdapters.XhtmlPageAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.Adapters.XhtmlAdapters.XhtmlPanelAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.Adapters.XhtmlAdapters.XhtmlPhoneCallAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.Adapters.XhtmlAdapters.XhtmlSelectionListAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.Adapters.XhtmlAdapters.XhtmlTextBoxAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.Adapters.XhtmlAdapters.XhtmlTextViewAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.Adapters.XhtmlAdapters.XhtmlValidationSummaryAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|
|<xref:System.Web.UI.MobileControls.Adapters.XhtmlAdapters.XhtmlValidatorAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll 程序集已弃用，因此不应再使用。 有关如何开发 ASP.NET 移动应用程序的信息，请参阅[面向移动应用程序的 ASP.NET](/aspnet/mobile/overview)。|

[返回页首](#introduction)

<a name="workflow_activities"></a>

### <a name="assembly-systemworkflowactivitiesdll"></a>程序集：System.Workflow.Activities.dll

|类型|消息|
|----------|-------------|
|<xref:System.Workflow.Activities?displayProperty=nameWithType> 命名空间中的所有类型|在 .NET Framework 4.5 中首次被废弃。<br /><br /> System.Workflow.\* 类型已弃用。 请改用 <xref:System.Activities>.\* 中的新类型。|
|<xref:System.Workflow.Activities.Configuration.ActiveDirectoryRoleFactoryConfiguration?displayProperty=nameWithType>|在 .NET Framework 4.5 中首次被废弃。<br /><br /> System.Workflow.\* 类型已弃用。 请改用 <xref:System.Activities>.\* 中的新类型。|
|<xref:System.Workflow.Activities.Rules.RuleActionTrackingEvent?displayProperty=nameWithType>|在 .NET Framework 4.5 中首次被废弃。<br /><br /> System.Workflow.\* 类型已弃用。 请改用 <xref:System.Activities>.\* 中的新类型。|
|<xref:System.Workflow.Activities.Rules.RuleConditionReference?displayProperty=nameWithType>|在 .NET Framework 4.5 中首次被废弃。<br /><br /> System.Workflow.\* 类型已弃用。 请改用 <xref:System.Activities>.\* 中的新类型。|
|<xref:System.Workflow.Activities.Rules.RuleSetReference?displayProperty=nameWithType>|在 .NET Framework 4.5 中首次被废弃。<br /><br /> System.Workflow.\* 类型已弃用。 请改用 <xref:System.Activities>.\* 中的新类型。|

[返回页首](#introduction)

<a name="workflow_componentmodel"></a>

### <a name="assembly-systemworkflowcomponentmodeldll"></a>程序集：System.Workflow.ComponentModel.dll

|类型|消息|
|----------|-------------|
|<xref:System.Workflow.ComponentModel> 命名空间中的所有类型，<xref:System.Workflow.ComponentModel.GetValueOverride?displayProperty=nameWithType> 和 <xref:System.Workflow.ComponentModel.SetValueOverride?displayProperty=nameWithType> 除外|在 .NET Framework 4.5 中首次被废弃。<br /><br /> System.Workflow.\* 类型已弃用。 请改用 <xref:System.Activities>.\* 中的新类型。|
|<xref:System.Workflow.ComponentModel.Compiler> 命名空间中的所有类型，<xref:System.Workflow.ComponentModel.Compiler.ValidationError?displayProperty=nameWithType> 和 <xref:System.Workflow.ComponentModel.Compiler.ValidationErrorCollection?displayProperty=nameWithType> 除外|在 .NET Framework 4.5 中首次被废弃。<br /><br /> System.Workflow.\* 类型已弃用。 请改用 <xref:System.Activities>.\* 中的新类型。|
|<xref:System.Workflow.ComponentModel.Design> 命名空间中的所有类型，<xref:System.Workflow.ComponentModel.Design.ConnectorEventHandler> 除外|在 .NET Framework 4.5 中首次被废弃。<br /><br /> System.Workflow.\* 类型已弃用。 请改用 <xref:System.Activities>.\* 中的新类型。|
|<xref:System.Workflow.ComponentModel.Serialization.ActivityCodeDomSerializationManager?displayProperty=nameWithType>|在 .NET Framework 4.5 中首次被废弃。<br /><br /> System.Workflow.\* 类型已弃用。 请改用 <xref:System.Activities>.\* 中的新类型。|
|<xref:System.Workflow.ComponentModel.Serialization.ActivityCodeDomSerializer?displayProperty=nameWithType>|在 .NET Framework 4.5 中首次被废弃。<br /><br /> System.Workflow.\* 类型已弃用。 请改用 <xref:System.Activities>.\* 中的新类型。|
|<xref:System.Workflow.ComponentModel.Serialization.ActivityMarkupSerializer?displayProperty=nameWithType>|在 .NET Framework 4.5 中首次被废弃。<br /><br /> System.Workflow.\* 类型已弃用。 请改用 <xref:System.Activities>.\* 中的新类型。|
|<xref:System.Workflow.ComponentModel.Serialization.ActivitySurrogateSelector?displayProperty=nameWithType>|在 .NET Framework 4.5 中首次被废弃。<br /><br /> System.Workflow.\* 类型已弃用。 请改用 <xref:System.Activities>.\* 中的新类型。|
|<xref:System.Workflow.ComponentModel.Serialization.ActivityTypeCodeDomSerializer?displayProperty=nameWithType>|在 .NET Framework 4.5 中首次被废弃。<br /><br /> System.Workflow.\* 类型已弃用。 请改用 <xref:System.Activities>.\* 中的新类型。|
|<xref:System.Workflow.ComponentModel.Serialization.CompositeActivityMarkupSerializer?displayProperty=nameWithType>|在 .NET Framework 4.5 中首次被废弃。<br /><br /> System.Workflow.\* 类型已弃用。 请改用 <xref:System.Activities>.\* 中的新类型。|
|<xref:System.Workflow.ComponentModel.Serialization.DependencyObjectCodeDomSerializer?displayProperty=nameWithType>|在 .NET Framework 4.5 中首次被废弃。<br /><br /> System.Workflow.\* 类型已弃用。 请改用 <xref:System.Activities>.\* 中的新类型。|

[返回页首](#introduction)

<a name="workflow_runtime"></a>

### <a name="assembly-systemworkflowruntimedll"></a>程序集：System.Workflow.Runtime.dll

|类型|消息|
|----------|-------------|
|<xref:System.Activities.Statements.Interop?displayProperty=nameWithType>|在 .NET Framework 4.5 中首次被废弃。<br /><br />Workflow Foundation 3.0 类型已弃用。 请改用 <xref:System.Activities>.\* 中的 Workflow 4.0 类型。|
|<xref:System.Activities.Tracking.InteropTrackingRecord?displayProperty=nameWithType>|在 .NET Framework 4.5 中首次被废弃。<br /><br />Workflow Foundation 3.0 类型已弃用。 请改用 <xref:System.Activities>.\* 中的 Workflow 4.0 类型。|
|<xref:System.Workflow.Runtime> 命名空间中的所有类型|在 .NET Framework 4.5 中首次被废弃。<br /><br /> System.Workflow.\* 类型已弃用。 请改用 <xref:System.Activities>.\* 中的新类型。|
|<xref:System.Workflow.Runtime.Configuration> 命名空间中的所有类型|在 .NET Framework 4.5 中首次被废弃。<br /><br /> System.Workflow.\* 类型已弃用。 请改用 <xref:System.Activities>.\* 中的新类型。|
|<xref:System.Workflow.Runtime.DebugEngine> 命名空间中的所有类型，<xref:System.Workflow.Runtime.DebugEngine.DebugEngineCallback> 除外|在 .NET Framework 4.5 中首次被废弃。<br /><br /> System.Workflow.\* 类型已弃用。 请改用 <xref:System.Activities>.\* 中的新类型。|
|<xref:System.Workflow.Runtime.Hosting> 命名空间中的所有类型，<xref:System.Workflow.Runtime.Hosting.WorkflowCommitWorkBatchService.CommitWorkBatchCallback> 除外|在 .NET Framework 4.5 中首次被废弃。<br /><br /> System.Workflow.\* 类型已弃用。 请改用 <xref:System.Activities>.\* 中的新类型。|
|<xref:System.Workflow.Runtime.Tracking> 命名空间中的所有类型|在 .NET Framework 4.5 中首次被废弃。<br /><br /> System.Workflow.\* 类型已弃用。 请改用 <xref:System.Activities>.\* 中的新类型。|

[返回页首](#introduction)

<a name="workflowservices"></a>

### <a name="assembly-systemworkflowservicesdll"></a>程序集：System.WorkflowServices.dll

|类型|消息|
|----------|-------------|
|<xref:System.ServiceModel.WorkflowServiceHost?displayProperty=nameWithType>|在 .NET Framework 4.5 中首次被废弃。<br /><br /> WF 3 类型已弃用。 请改用 <xref:System.Activities>.\* 中的新 WF 4 类型。|
|<xref:System.ServiceModel.Activation.WorkflowServiceHostFactory?displayProperty=nameWithType>|在 .NET Framework 4.5 中首次被废弃。<br /><br /> WF 3 类型已弃用。 请改用 <xref:System.Activities>.\* 中的新 WF 4 类型。|
|<xref:System.ServiceModel.Activities.Description.WorkflowRuntimeEndpoint?displayProperty=nameWithType>|在 .NET Framework 4.5 中首次被废弃。<br /><br /> WF 3 类型已弃用。 请改用 <xref:System.Activities>.\* 中的新 WF 4 类型。|
|<xref:System.ServiceModel.Configuration.ExtendedWorkflowRuntimeServiceElementCollection?displayProperty=nameWithType>|在 .NET Framework 4.5 中首次被废弃。<br /><br /> WF 3 类型已弃用。 请改用 <xref:System.Activities>.\* 中的新 WF 4 类型。|
|<xref:System.ServiceModel.Configuration.PersistenceProviderElement?displayProperty=nameWithType>|在 .NET Framework 4.5 中首次被废弃。<br /><br /> WF 3 类型已弃用。 请改用 <xref:System.Activities>.\* 中的新 WF 4 类型。|
|<xref:System.ServiceModel.Configuration.WorkflowRuntimeElement?displayProperty=nameWithType>|在 .NET Framework 4.5 中首次被废弃。<br /><br /> WF 3 类型已弃用。 请改用 <xref:System.Activities>.\* 中的新 WF 4 类型。|
|<xref:System.ServiceModel.Description.DurableOperationAttribute?displayProperty=nameWithType>|在 .NET Framework 4.5 中首次被废弃。<br /><br /> WF 3 类型已弃用。 请改用 <xref:System.Activities>.\* 中的新 WF 4 类型。|
|<xref:System.ServiceModel.Description.DurableServiceAttribute?displayProperty=nameWithType>|在 .NET Framework 4.5 中首次被废弃。<br /><br /> WF 3 类型已弃用。 请改用 <xref:System.Activities>.\* 中的新 WF 4 类型。|
|<xref:System.ServiceModel.Description.PersistenceProviderBehavior?displayProperty=nameWithType>|在 .NET Framework 4.5 中首次被废弃。<br /><br /> WF 3 类型已弃用。 请改用 <xref:System.Activities>.\* 中的新 WF 4 类型。|
|<xref:System.ServiceModel.Description.UnknownExceptionAction?displayProperty=nameWithType>|在 .NET Framework 4.5 中首次被废弃。<br /><br /> WF 3 类型已弃用。 请改用 <xref:System.Activities>.\* 中的新 WF 4 类型。|
|<xref:System.ServiceModel.Description.WorkflowRuntimeBehavior?displayProperty=nameWithType>|在 .NET Framework 4.5 中首次被废弃。<br /><br /> WF 3 类型已弃用。 请改用 <xref:System.Activities>.\* 中的新 WF 4 类型。|
|<xref:System.ServiceModel.Dispatcher.DurableOperationContext?displayProperty=nameWithType>|在 .NET Framework 4.5 中首次被废弃。<br /><br /> WF 3 类型已弃用。 请改用 <xref:System.Activities>.\* 中的新 WF 4 类型。|
|<xref:System.ServiceModel.Persistence.InstanceLockException?displayProperty=nameWithType>|在 .NET Framework 4.5 中首次被废弃。<br /><br /> WF 3 类型已弃用。 请改用 <xref:System.Activities>.\* 中的新 WF 4 类型。|
|<xref:System.ServiceModel.Persistence.InstanceNotFoundException?displayProperty=nameWithType>|在 .NET Framework 4.5 中首次被废弃。<br /><br /> WF 3 类型已弃用。 请改用 <xref:System.Activities>.\* 中的新 WF 4 类型。|
|<xref:System.ServiceModel.Persistence.LockingPersistenceProvider?displayProperty=nameWithType>|在 .NET Framework 4.5 中首次被废弃。<br /><br /> WF 3 类型已弃用。 请改用 <xref:System.Activities>.\* 中的新 WF 4 类型。|
|<xref:System.ServiceModel.Persistence.PersistenceException?displayProperty=nameWithType>|在 .NET Framework 4.5 中首次被废弃。<br /><br /> WF 3 类型已弃用。 请改用 <xref:System.Activities>.\* 中的新 WF 4 类型。|
|<xref:System.ServiceModel.Persistence.PersistenceProvider?displayProperty=nameWithType>|在 .NET Framework 4.5 中首次被废弃。<br /><br /> WF 3 类型已弃用。 请改用 <xref:System.Activities>.\* 中的新 WF 4 类型。|
|<xref:System.ServiceModel.Persistence.PersistenceProviderFactory?displayProperty=nameWithType>|在 .NET Framework 4.5 中首次被废弃。<br /><br /> WF 3 类型已弃用。 请改用 <xref:System.Activities>.\* 中的新 WF 4 类型。|
|<xref:System.ServiceModel.Persistence.SqlPersistenceProviderFactory?displayProperty=nameWithType>|在 .NET Framework 4.5 中首次被废弃。<br /><br /> WF 3 类型已弃用。 请改用 <xref:System.Activities>.\* 中的新 WF 4 类型。|
|<xref:System.Workflow.Activities?displayProperty=nameWithType> 命名空间中的所有类型|在 .NET Framework 4.5 中首次被废弃。<br /><br /> WF 3 类型已弃用。 请改用 <xref:System.Activities>.\* 中的新 WF 4 类型。|
|<xref:System.Workflow.Runtime.Hosting.ChannelManagerService?displayProperty=nameWithType>|在 .NET Framework 4.5 中首次被废弃。<br /><br /> WF 3 类型已弃用。 请改用 <xref:System.Activities>.\* 中的新 WF 4 类型。|

[返回页首](#introduction)

<a name="xaml"></a>

### <a name="assembly-systemxamldll"></a>程序集：System.Xaml.dll

|类型|消息|
|----------|-------------|
|<xref:System.Windows.Markup.AcceptedMarkupExtensionExpressionTypeAttribute?displayProperty=nameWithType>|XAML 分析器不使用此类型。 请查看 <xref:System.Windows.Markup.XamlSetMarkupExtensionAttribute?displayProperty=nameWithType>。|

[返回页首](#introduction)

<a name="xml"></a>

### <a name="assembly-systemxmldll"></a>程序集：System.Xml.dll

|类型|消息|
|----------|-------------|
|<xref:System.Xml.IApplicationResourceStreamResolver?displayProperty=nameWithType>|在 .NET Framework 4.5 中首次被废弃。<br /><br /> 使用此类型会生成编译器错误。<br /><br /> 此 API 支持 .NET Framework 基础结构，但不应在代码中直接使用。|
|<xref:System.Xml.Schema.XmlSchemaCollection?displayProperty=nameWithType>|请使用 <xref:System.Xml.Schema.XmlSchemaSet?displayProperty=nameWithType> 进行架构编译和验证。|
|<xref:System.Xml.XmlValidatingReader?displayProperty=nameWithType>|改用使用适当的 <xref:System.Xml.XmlReader?displayProperty=nameWithType> 的 <xref:System.Xml.XmlReader.Create%2A?displayProperty=nameWithType> 方法创建的 <xref:System.Xml.XmlReaderSettings?displayProperty=nameWithType>。|
|<xref:System.Xml.XmlXapResolver?displayProperty=nameWithType>|使用此类型会生成编译器错误。 此 API 支持 .NET Framework 基础结构，但不应在代码中直接使用。|
|<xref:System.Xml.Xsl.XslTransform?displayProperty=nameWithType>|此类已弃用。 请改用 <xref:System.Xml.Xsl.XslCompiledTransform?displayProperty=nameWithType>。|

[返回页首](#introduction)

<a name="WindowsBase"></a>

### <a name="assembly-windowsbasedll"></a>程序集：WindowsBase.dll

|类型|消息|
|----------|-------------|
|<xref:System.Windows.Markup.IReceiveMarkupExtension?displayProperty=nameWithType>|<xref:System.Windows.Markup.IReceiveMarkupExtension?displayProperty=nameWithType> 已弃用。 不再使用此接口。|

[返回页首](#introduction)

<a name="obsolete_types_in_microsoft_assemblies"></a>

## <a name="obsolete-types-in-microsoft-assemblies"></a>Microsoft 程序集中的过时类型

以下各节列出了 Microsoft 程序集中的过时类型。 这些程序集是具有特殊用途的程序集，如面向单一语言的程序集（如 Microsoft.JScript.dll 或 Microsoft.VisualC.dll）。

<a name="IEHost"></a>

### <a name="assembly-iehostdll-and-ieexecexe"></a>程序集：IEHost.dll 和 IEExec.exe

IEHost.dll 和 IEExec.exe 程序集已从 .NET Framework 中删除。 其所有类型和成员已过时，且从 .NET Framework 4 开始已不支持它们。 这些程序集过去用于承载 Windows 窗体控件和在 Internet Explorer 中运行可执行文件。 建议的替代项包括 ClickOnce、XAML 浏览器应用程序 (XBAP) 和 Microsoft Silverlight。

[返回页首](#introduction)

<a name="Engine"></a>

### <a name="assembly-microsoftbuildenginedll"></a>程序集：Microsoft.Build.Engine.dll

|类型|消息|
|----------|-------------|
|<xref:Microsoft.Build.BuildEngine.Engine?displayProperty=nameWithType>|此类已弃用。 请改用 Microsoft.Build 程序集中的 <xref:Microsoft.Build.Evaluation.ProjectCollection?displayProperty=nameWithType>。|
|<xref:Microsoft.Build.BuildEngine.Project?displayProperty=nameWithType>|此类已弃用。 请改用 Microsoft.Build 程序集中的 <xref:Microsoft.Build.Evaluation.ProjectCollection?displayProperty=nameWithType>。|

[返回页首](#introduction)

<a name="jscript"></a>

### <a name="assembly-microsoftjscriptdll"></a>程序集：Microsoft.JScript.dll

|类型|消息|
|----------|-------------|
|<xref:Microsoft.JScript.Vsa.BaseVsaEngine?displayProperty=nameWithType>|Visual Studio 2005 中已弃用此类型；此功能没有替代项。 有关其他帮助，请参见 <xref:System.CodeDom.Compiler.ICodeCompiler?displayProperty=nameWithType> 文档。|
|<xref:Microsoft.JScript.Vsa.BaseVsaSite?displayProperty=nameWithType>|Visual Studio 2005 中已弃用此类型；此功能没有替代项。 有关其他帮助，请参见 <xref:System.CodeDom.Compiler.ICodeCompiler?displayProperty=nameWithType> 文档。|
|<xref:Microsoft.JScript.Vsa.BaseVsaStartup?displayProperty=nameWithType>|Visual Studio 2005 中已弃用此类型；此功能没有替代项。 有关其他帮助，请参见 <xref:System.CodeDom.Compiler.ICodeCompiler?displayProperty=nameWithType> 文档。|
|<xref:Microsoft.JScript.Vsa.IJSVsaCodeItem?displayProperty=nameWithType>|Visual Studio 2005 中已弃用此类型；此功能没有替代项。 有关其他帮助，请参见 <xref:System.CodeDom.Compiler.ICodeCompiler?displayProperty=nameWithType> 文档。|
|<xref:Microsoft.JScript.Vsa.IJSVsaEngine?displayProperty=nameWithType>|Visual Studio 2005 中已弃用此类型；此功能没有替代项。 有关其他帮助，请参见 <xref:System.CodeDom.Compiler.ICodeCompiler?displayProperty=nameWithType> 文档。|
|<xref:Microsoft.JScript.Vsa.IJSVsaError?displayProperty=nameWithType>|Visual Studio 2005 中已弃用此类型；此功能没有替代项。 有关其他帮助，请参见 <xref:System.CodeDom.Compiler.ICodeCompiler?displayProperty=nameWithType> 文档。|
|<xref:Microsoft.JScript.Vsa.IJSVsaGlobalItem?displayProperty=nameWithType>|Visual Studio 2005 中已弃用此类型；此功能没有替代项。 有关其他帮助，请参见 <xref:System.CodeDom.Compiler.ICodeCompiler?displayProperty=nameWithType> 文档。|
|<xref:Microsoft.JScript.Vsa.IJSVsaItem?displayProperty=nameWithType>|Visual Studio 2005 中已弃用此类型；此功能没有替代项。 有关其他帮助，请参见 <xref:System.CodeDom.Compiler.ICodeCompiler?displayProperty=nameWithType> 文档。|
|<xref:Microsoft.JScript.Vsa.IJSVsaItems?displayProperty=nameWithType>|Visual Studio 2005 中已弃用此类型；此功能没有替代项。 有关其他帮助，请参见 <xref:System.CodeDom.Compiler.ICodeCompiler?displayProperty=nameWithType> 文档。|
|<xref:Microsoft.JScript.Vsa.IJSVsaPersistSite?displayProperty=nameWithType>|Visual Studio 2005 中已弃用此类型；此功能没有替代项。 有关其他帮助，请参见 <xref:System.CodeDom.Compiler.ICodeCompiler?displayProperty=nameWithType> 文档。|
|<xref:Microsoft.JScript.Vsa.IJSVsaReferenceItem?displayProperty=nameWithType>|Visual Studio 2005 中已弃用此类型；此功能没有替代项。 有关其他帮助，请参见 <xref:System.CodeDom.Compiler.ICodeCompiler?displayProperty=nameWithType> 文档。|
|<xref:Microsoft.JScript.Vsa.IJSVsaSite?displayProperty=nameWithType>|Visual Studio 2005 中已弃用此类型；此功能没有替代项。 有关其他帮助，请参见 <xref:System.CodeDom.Compiler.ICodeCompiler?displayProperty=nameWithType> 文档。|
|<xref:Microsoft.JScript.Vsa.JSVsaError?displayProperty=nameWithType>|Visual Studio 2005 中已弃用此类型；此功能没有替代项。 有关其他帮助，请参见 <xref:System.CodeDom.Compiler.ICodeCompiler?displayProperty=nameWithType> 文档。|
|<xref:Microsoft.JScript.Vsa.JSVsaException?displayProperty=nameWithType>|Visual Studio 2005 中已弃用此类型；此功能没有替代项。 有关其他帮助，请参见 <xref:System.CodeDom.Compiler.ICodeCompiler?displayProperty=nameWithType> 文档。|
|<xref:Microsoft.JScript.Vsa.JSVsaItemFlag?displayProperty=nameWithType>|Visual Studio 2005 中已弃用此类型；此功能没有替代项。 有关其他帮助，请参见 <xref:System.CodeDom.Compiler.ICodeCompiler?displayProperty=nameWithType> 文档。|
|<xref:Microsoft.JScript.Vsa.JSVsaItemType?displayProperty=nameWithType>|Visual Studio 2005 中已弃用此类型；此功能没有替代项。 有关其他帮助，请参见 <xref:System.CodeDom.Compiler.ICodeCompiler?displayProperty=nameWithType> 文档。|
|<xref:Microsoft.JScript.Vsa.ResInfo?displayProperty=nameWithType>|Visual Studio 2005 中已弃用此类型；此功能没有替代项。 有关其他帮助，请参见 <xref:System.CodeDom.Compiler.ICodeCompiler?displayProperty=nameWithType> 文档。|
|<xref:Microsoft.JScript.Vsa.VsaEngine?displayProperty=nameWithType>|Visual Studio 2005 中已弃用此类型；此功能没有替代项。 有关其他帮助，请参见 <xref:System.CodeDom.Compiler.ICodeCompiler?displayProperty=nameWithType> 文档。|

[返回页首](#introduction)

<a name="VBCompat"></a>

### <a name="assembly-microsoftvisualbasiccompatibilitydll"></a>程序集：Microsoft.VisualBasic.Compatibility.dll

有关从 Visual Basic 6 迁移的信息，请参阅 [Visual Basic 6.0 资源中心](/previous-versions/visualstudio/visual-basic-6/visual-basic-6.0-documentation)。

|类型|消息|
|----------|-------------|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.BaseControlArray?displayProperty=nameWithType>|此成员已过时。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.BaseOcxArray?displayProperty=nameWithType>|此成员已过时。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.ButtonArray?displayProperty=nameWithType>|此成员已过时。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.CheckBoxArray?displayProperty=nameWithType>|此成员已过时。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.CheckedListBoxArray?displayProperty=nameWithType>|此成员已过时。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.ColorDialogArray?displayProperty=nameWithType>|此成员已过时。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.ComboBoxArray?displayProperty=nameWithType>|此成员已过时。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.DirListBox?displayProperty=nameWithType>|此成员已过时。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.DirListBoxArray?displayProperty=nameWithType>|此成员已过时。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.DriveListBox?displayProperty=nameWithType>|此成员已过时。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.DriveListBoxArray?displayProperty=nameWithType>|此成员已过时。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.FileListBox?displayProperty=nameWithType>|此成员已过时。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.FileListBoxArray?displayProperty=nameWithType>|此成员已过时。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.FixedLengthString?displayProperty=nameWithType>|此成员已过时。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.FontDialogArray?displayProperty=nameWithType>|此成员已过时。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.FormShowConstants?displayProperty=nameWithType>|此成员已过时。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.GroupBoxArray?displayProperty=nameWithType>|此成员已过时。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.HScrollBarArray?displayProperty=nameWithType>|此成员已过时。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.ImageListArray?displayProperty=nameWithType>|此成员已过时。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.LabelArray?displayProperty=nameWithType>|此成员已过时。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.ListBoxArray?displayProperty=nameWithType>|此成员已过时。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.ListBoxItem?displayProperty=nameWithType>|此成员已过时。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.ListViewArray?displayProperty=nameWithType>|此成员已过时。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.LoadResConstants?displayProperty=nameWithType>|此成员已过时。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.MaskedTextBoxArray?displayProperty=nameWithType>|此成员已过时。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.MenuItemArray?displayProperty=nameWithType>|此成员已过时。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.MouseButtonConstants?displayProperty=nameWithType>|此成员已过时。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.OpenFileDialogArray?displayProperty=nameWithType>|此成员已过时。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.PanelArray?displayProperty=nameWithType>|此成员已过时。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.PictureBoxArray?displayProperty=nameWithType>|此成员已过时。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.PrintDialogArray?displayProperty=nameWithType>|此成员已过时。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.ProgressBarArray?displayProperty=nameWithType>|此成员已过时。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.RadioButtonArray?displayProperty=nameWithType>|此成员已过时。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.RichTextBoxArray?displayProperty=nameWithType>|此成员已过时。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.SaveFileDialogArray?displayProperty=nameWithType>|此成员已过时。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.ScaleMode?displayProperty=nameWithType>|此成员已过时。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.ShiftConstants?displayProperty=nameWithType>|此成员已过时。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.StatusBarArray?displayProperty=nameWithType>|此成员已过时。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.StatusStripArray?displayProperty=nameWithType>|此成员已过时。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.Support?displayProperty=nameWithType>|此成员已过时。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.TabControlArray?displayProperty=nameWithType>|此成员已过时。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.TextBoxArray?displayProperty=nameWithType>|此成员已过时。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.TimerArray?displayProperty=nameWithType>|此成员已过时。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.ToolBarArray?displayProperty=nameWithType>|此成员已过时。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.ToolStripArray?displayProperty=nameWithType>|此成员已过时。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.ToolStripMenuItemArray?displayProperty=nameWithType>|此成员已过时。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.TreeViewArray?displayProperty=nameWithType>|此成员已过时。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.VScrollBarArray?displayProperty=nameWithType>|此成员已过时。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.WebBrowserArray?displayProperty=nameWithType>|此成员已过时。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.WebClass?displayProperty=nameWithType>|此成员已过时。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.WebClassContainingClassNotOptional?displayProperty=nameWithType>|此成员已过时。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.WebClassCouldNotFindEvent?displayProperty=nameWithType>|此成员已过时。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.WebClassNextItemCannotBeCurrentWebItem?displayProperty=nameWithType>|此成员已过时。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.WebClassNextItemRespondNotFound?displayProperty=nameWithType>|此成员已过时。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.WebClassUserWebClassNameNotOptional?displayProperty=nameWithType>|此成员已过时。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.WebClassWebClassFileNameNotOptional?displayProperty=nameWithType>|此成员已过时。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.WebClassWebItemNotValid?displayProperty=nameWithType>|此成员已过时。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.WebItem?displayProperty=nameWithType>|此成员已过时。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.WebItemAssociatedWebClassNotOptional?displayProperty=nameWithType>|此成员已过时。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.WebItemClosingTagNotFound?displayProperty=nameWithType>|此成员已过时。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.WebItemCouldNotLoadEmbeddedResource?displayProperty=nameWithType>|此成员已过时。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.WebItemCouldNotLoadTemplateFile?displayProperty=nameWithType>|此成员已过时。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.WebItemNameNotOptional?displayProperty=nameWithType>|此成员已过时。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.WebItemNoTemplateSpecified?displayProperty=nameWithType>|此成员已过时。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.WebItemTooManyNestedTags?displayProperty=nameWithType>|此成员已过时。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.WebItemUnexpectedErrorReadingTemplateFile?displayProperty=nameWithType>|此成员已过时。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.ZOrderConstants?displayProperty=nameWithType>|此成员已过时。|

[返回页首](#introduction)

<a name="VBCompatData"></a>

### <a name="assembly-microsoftvisualbasiccompatibilitydatadll"></a>程序集：Microsoft.VisualBasic.Compatibility.Data.dll

|类型|消息|
|----------|-------------|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.ADODC?displayProperty=nameWithType>|此成员已过时。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.ADODC.BOFActionEnum?displayProperty=nameWithType>|此成员已过时。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.ADODC.EndOfRecordsetDelegate?displayProperty=nameWithType>|此成员已过时。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.ADODC.EOFActionEnum?displayProperty=nameWithType>|此成员已过时。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.ADODC.ErrorDelegate?displayProperty=nameWithType>|此成员已过时。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.ADODC.FetchCompleteDelegate?displayProperty=nameWithType>|此成员已过时。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.ADODC.FetchProgressDelegate?displayProperty=nameWithType>|此成员已过时。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.ADODC.FieldChangeCompleteDelegate?displayProperty=nameWithType>|此成员已过时。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.ADODC.MoveCompleteDelegate?displayProperty=nameWithType>|此成员已过时。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.ADODC.OrientationEnum?displayProperty=nameWithType>|此成员已过时。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.ADODC.RecordChangeCompleteDelegate?displayProperty=nameWithType>|此成员已过时。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.ADODC.RecordsetChangeCompleteDelegate?displayProperty=nameWithType>|此成员已过时。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.ADODC.WillChangeFieldDelegate?displayProperty=nameWithType>|此成员已过时。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.ADODC.WillChangeRecordDelegate?displayProperty=nameWithType>|此成员已过时。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.ADODC.WillChangeRecordsetDelegate?displayProperty=nameWithType>|此成员已过时。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.ADODC.WillMoveDelegate?displayProperty=nameWithType>|此成员已过时。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.ADODCArray?displayProperty=nameWithType>|此成员已过时。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.BaseDataEnvironment?displayProperty=nameWithType>|此成员已过时。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.BindingCollectionEnumerator?displayProperty=nameWithType>|此成员已过时。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.CONNECTDATA?displayProperty=nameWithType>|此成员已过时。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.DBBINDING?displayProperty=nameWithType>|此成员已过时。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.DBCOLUMNINFO?displayProperty=nameWithType>|此成员已过时。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.DBID?displayProperty=nameWithType>|此成员已过时。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.DBinding?displayProperty=nameWithType>|此成员已过时。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.DBindingCollection?displayProperty=nameWithType>|此成员已过时。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.DBKINDENUM?displayProperty=nameWithType>|此成员已过时。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.DBPROPIDSET?displayProperty=nameWithType>|此成员已过时。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.IAccessor?displayProperty=nameWithType>|此成员已过时。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.IChapteredRowset?displayProperty=nameWithType>|此成员已过时。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.IColumnsInfo?displayProperty=nameWithType>|此成员已过时。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.IConnectionPoint?displayProperty=nameWithType>|此成员已过时。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.IConnectionPointContainer?displayProperty=nameWithType>|此成员已过时。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.IDataFormat?displayProperty=nameWithType>|此成员已过时。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.IDataFormatDisp?displayProperty=nameWithType>|此成员已过时。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.IEnumConnectionPoints?displayProperty=nameWithType>|此成员已过时。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.IEnumConnections?displayProperty=nameWithType>|此成员已过时。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.IRowPosition?displayProperty=nameWithType>|此成员已过时。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.IRowPositionChange?displayProperty=nameWithType>|此成员已过时。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.IRowset?displayProperty=nameWithType>|此成员已过时。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.IRowsetChange?displayProperty=nameWithType>|此成员已过时。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.IRowsetIdentity?displayProperty=nameWithType>|此成员已过时。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.IRowsetInfo?displayProperty=nameWithType>|此成员已过时。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.IRowsetNotify?displayProperty=nameWithType>|此成员已过时。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.MBinding?displayProperty=nameWithType>|此成员已过时。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.MBindingCollection?displayProperty=nameWithType>|此成员已过时。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.SRDescriptionAttribute?displayProperty=nameWithType>|此成员已过时。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.UGUID?displayProperty=nameWithType>|此成员已过时。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.UNAME?displayProperty=nameWithType>|此成员已过时。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.UpdateMode?displayProperty=nameWithType>|此成员已过时。|

[返回页首](#introduction)

<a name="visualc"></a>

### <a name="assembly-microsoftvisualcdll"></a>程序集：Microsoft.VisualC.dll

|类型|消息|
|----------|-------------|
|<xref:Microsoft.VisualC.DebugInfoInPDBAttribute?displayProperty=nameWithType>|Microsoft.VisualC.dll 是已过时的程序集，仅出于向后兼容目的而存在。|
|<xref:Microsoft.VisualC.DecoratedNameAttribute?displayProperty=nameWithType>|Microsoft.VisualC.dll 是已过时的程序集，仅出于向后兼容目的而存在。|
|<xref:Microsoft.VisualC.IsConstModifier?displayProperty=nameWithType>|Microsoft.VisualC.dll 是已过时的程序集，仅出于向后兼容目的而存在。|
|<xref:Microsoft.VisualC.IsCXXReferenceModifier?displayProperty=nameWithType>|Microsoft.VisualC.dll 是已过时的程序集，仅出于向后兼容目的而存在。|
|<xref:Microsoft.VisualC.IsLongModifier?displayProperty=nameWithType>|Microsoft.VisualC.dll 是已过时的程序集，仅出于向后兼容目的而存在。|
|<xref:Microsoft.VisualC.IsSignedModifier?displayProperty=nameWithType>|Microsoft.VisualC.dll 是已过时的程序集，仅出于向后兼容目的而存在。|
|<xref:Microsoft.VisualC.IsVolatileModifier?displayProperty=nameWithType>|Microsoft.VisualC.dll 是已过时的程序集，仅出于向后兼容目的而存在。|
|<xref:Microsoft.VisualC.MiscellaneousBitsAttribute?displayProperty=nameWithType>|Microsoft.VisualC.dll 是已过时的程序集，仅出于向后兼容目的而存在。|
|<xref:Microsoft.VisualC.NeedsCopyConstructorModifier?displayProperty=nameWithType>|Microsoft.VisualC.dll 是已过时的程序集，仅出于向后兼容目的而存在。|
|<xref:Microsoft.VisualC.NoSignSpecifiedModifier?displayProperty=nameWithType>|Microsoft.VisualC.dll 是已过时的程序集，仅出于向后兼容目的而存在。|

## <a name="see-also"></a>请参阅

- [类库中过时的内容](whats-obsolete.md)
- [过时成员](obsolete-members.md)
