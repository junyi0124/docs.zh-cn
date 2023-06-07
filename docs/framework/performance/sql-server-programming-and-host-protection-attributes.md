---
title: SQL Server 编程和宿主保护特性
description: SQL Server 编程和宿主保护属性入门。 查看 SQL Server 权限集和编程模型限制。
ms.date: 03/30/2017
helpviewer_keywords:
- SQL Server [.NET Framework]
- permission sets, SQL Server
- SQL Server Programming and Host Protection Attributes
- managed code, SQL Server
- reliability [.NET Framework]
- writing reliable code
- hosts, reliability
- host protection attributes
- HostProtectionAttribute class, reliability
ms.assetid: 7dfa36b4-e773-4c75-a3ff-ff1af3ce4c4f
ms.openlocfilehash: 3ded0a9ac95d56e000ef7870ec19721764c96a66
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96236229"
---
# <a name="sql-server-programming-and-host-protection-attributes"></a>SQL Server 编程和宿主保护特性

在 SQL Server 主机中加载和执行托管代码需要满足主机对代码访问安全性和主机资源保护的要求。  代码访问安全性要求由三个 SQL Server 权限集（SAFE、EXTERNAL-ACCESS 或 UNSAFE）其中之一指定。 在 SAFE 或 EXTERNAL-ACCESS 权限集内执行的代码必须避免某些类型或应用了 <xref:System.Security.Permissions.HostProtectionAttribute> 属性的成员。 <xref:System.Security.Permissions.HostProtectionAttribute> 不是可靠性保证的安全权限，因为它标识主机可能禁止的特定代码结构（类型或方法）。  使用 <xref:System.Security.Permissions.HostProtectionAttribute> 会强制执行可帮助保护宿主稳定性的编程模型。  
  
## <a name="host-protection-attributes"></a>宿主保护属性  

 主机保护属性识别不符合主机编程模型的类型或成员，并表示以下级别的可靠性威胁：  
  
- 在其他方面为良性。  
  
- 可能会导致反序列化服务器托管的用户代码。  
  
- 可能会导致反序列化服务器进程本身。  
  
 SQL Server 不允许使用带有 <xref:System.Security.Permissions.HostProtectionAttribute> 的类型或成员，该属性指定 <xref:System.Security.Permissions.HostProtectionResource.SharedState>、<xref:System.Security.Permissions.HostProtectionResource.Synchronization>、<xref:System.Security.Permissions.HostProtectionResource.MayLeakOnAbort> 或 <xref:System.Security.Permissions.HostProtectionResource.ExternalProcessMgmt> 的 <xref:System.Security.Permissions.HostProtectionResource> 值。 这样可防止程序集调用启用共享状态、执行同步、终止时可能导致资源泄漏或影响 SQL Server 进程的完整性的成员。  
  
### <a name="disallowed-types-and-members"></a>不允许的类型和成员  

 下表标识了 SQL Server 禁止其 <xref:System.Security.Permissions.HostProtectionResource> 值的类型和成员。  
  
|命名空间|类型或成员|  
|---------------|--------------------|  
|`Microsoft.Win32`|<xref:Microsoft.Win32.PowerModeChangedEventArgs> 类<br /><br /> <xref:Microsoft.Win32.PowerModeChangedEventHandler> 委托<br /><br /> <xref:Microsoft.Win32.SessionEndedEventArgs> 类<br /><br /> <xref:Microsoft.Win32.SessionEndedEventHandler> 委托<br /><br /> <xref:Microsoft.Win32.SessionEndingEventArgs> 类<br /><br /> <xref:Microsoft.Win32.SessionEndingEventHandler> 委托<br /><br /> <xref:Microsoft.Win32.SessionSwitchEventArgs> 类<br /><br /> <xref:Microsoft.Win32.SessionSwitchEventHandler> 委托<br /><br /> <xref:Microsoft.Win32.SystemEvents> 类<br /><br /> <xref:Microsoft.Win32.TimerElapsedEventArgs> 类<br /><br /> <xref:Microsoft.Win32.TimerElapsedEventHandler> 委托<br /><br /> <xref:Microsoft.Win32.UserPreferenceChangedEventArgs> 类<br /><br /> <xref:Microsoft.Win32.UserPreferenceChangingEventArgs> 类|  
|`System.Collections`|<xref:System.Collections.ArrayList.Synchronized%2A?displayProperty=nameWithType> 方法<br /><br /> <xref:System.Collections.Hashtable.Synchronized%2A?displayProperty=nameWithType> 方法<br /><br /> <xref:System.Collections.Queue.Synchronized%2A?displayProperty=nameWithType> 方法<br /><br /> <xref:System.Collections.SortedList.Synchronized%2A?displayProperty=nameWithType> 方法<br /><br /> <xref:System.Collections.Stack.Synchronized%2A?displayProperty=nameWithType> 方法|  
|`System.ComponentModel`|<xref:System.ComponentModel.AddingNewEventArgs> 类<br /><br /> <xref:System.ComponentModel.AddingNewEventHandler> 委托<br /><br /> <xref:System.ComponentModel.ArrayConverter> 类<br /><br /> <xref:System.ComponentModel.AsyncCompletedEventArgs> 类<br /><br /> <xref:System.ComponentModel.AsyncCompletedEventHandler> 委托<br /><br /> <xref:System.ComponentModel.AsyncOperation> 类<br /><br /> <xref:System.ComponentModel.AsyncOperationManager> 类<br /><br /> <xref:System.ComponentModel.AttributeCollection> 类<br /><br /> <xref:System.ComponentModel.BackgroundWorker> 类<br /><br /> <xref:System.ComponentModel.BaseNumberConverter> 类<br /><br /> <xref:System.ComponentModel.BindingList%601> 类<br /><br /> <xref:System.ComponentModel.BooleanConverter> 类<br /><br /> <xref:System.ComponentModel.ByteConverter> 类<br /><br /> <xref:System.ComponentModel.CancelEventArgs> 类<br /><br /> <xref:System.ComponentModel.CancelEventHandler> 委托<br /><br /> <xref:System.ComponentModel.CharConverter> 类<br /><br /> <xref:System.ComponentModel.CollectionChangeEventArgs> 类<br /><br /> <xref:System.ComponentModel.CollectionChangeEventHandler> 委托<br /><br /> <xref:System.ComponentModel.CollectionConverter> 类<br /><br /> <xref:System.ComponentModel.ComponentCollection> 类<br /><br /> <xref:System.ComponentModel.ComponentConverter> 类<br /><br /> <xref:System.ComponentModel.ComponentEditor> 类<br /><br /> <xref:System.ComponentModel.ComponentResourceManager> 类<br /><br /> <xref:System.ComponentModel.Container> 类<br /><br /> <xref:System.ComponentModel.ContainerFilterService> 类<br /><br /> <xref:System.ComponentModel.CultureInfoConverter> 类<br /><br /> <xref:System.ComponentModel.CustomTypeDescriptor> 类<br /><br /> <xref:System.ComponentModel.DateTimeConverter> 类<br /><br /> <xref:System.ComponentModel.DecimalConverter> 类<br /><br /> <xref:System.ComponentModel.Design.ActiveDesignerEventArgs> 类<br /><br /> <xref:System.ComponentModel.Design.ActiveDesignerEventHandler> 委托<br /><br /> <xref:System.ComponentModel.Design.CheckoutException> 类<br /><br /> <xref:System.ComponentModel.Design.CommandID> 类<br /><br /> <xref:System.ComponentModel.Design.ComponentChangedEventArgs> 类<br /><br /> <xref:System.ComponentModel.Design.ComponentChangedEventHandler> 委托<br /><br /> <xref:System.ComponentModel.Design.ComponentChangingEventArgs> 类<br /><br /> <xref:System.ComponentModel.Design.ComponentChangingEventHandler> 委托<br /><br /> <xref:System.ComponentModel.Design.ComponentEventArgs> 类<br /><br /> <xref:System.ComponentModel.Design.ComponentEventHandler> 委托<br /><br /> <xref:System.ComponentModel.Design.ComponentRenameEventArgs> 类<br /><br /> <xref:System.ComponentModel.Design.ComponentRenameEventHandler> 委托<br /><br /> <xref:System.ComponentModel.Design.DesignerCollection> 类<br /><br /> <xref:System.ComponentModel.Design.DesignerEventArgs> 类<br /><br /> <xref:System.ComponentModel.Design.DesignerEventHandler> 委托<br /><br /> <xref:System.ComponentModel.Design.DesignerOptionService> 类<br /><br /> <xref:System.ComponentModel.Design.DesignerTransaction> 类<br /><br /> <xref:System.ComponentModel.Design.DesignerTransactionCloseEventArgs> 类<br /><br /> <xref:System.ComponentModel.Design.DesignerTransactionCloseEventHandler> 委托<br /><br /> <xref:System.ComponentModel.Design.DesignerVerb> 类<br /><br /> <xref:System.ComponentModel.Design.DesignerVerbCollection> 类<br /><br /> <xref:System.ComponentModel.Design.DesigntimeLicenseContext> 类<br /><br /> <xref:System.ComponentModel.Design.DesigntimeLicenseContextSerializer> 类<br /><br /> <xref:System.ComponentModel.Design.MenuCommand> 类<br /><br /> <xref:System.ComponentModel.Design.Serialization.ComponentSerializationService> 类<br /><br /> <xref:System.ComponentModel.Design.Serialization.ContextStack> 类<br /><br /> <xref:System.ComponentModel.Design.Serialization.DesignerLoader> 类<br /><br /> <xref:System.ComponentModel.Design.Serialization.InstanceDescriptor> 类<br /><br /> <xref:System.ComponentModel.Design.Serialization.MemberRelationshipService> 类<br /><br /> <xref:System.ComponentModel.Design.Serialization.ResolveNameEventArgs> 类<br /><br /> <xref:System.ComponentModel.Design.Serialization.ResolveNameEventHandler> 委托<br /><br /> <xref:System.ComponentModel.Design.Serialization.SerializationStore> 类<br /><br /> <xref:System.ComponentModel.Design.ServiceContainer> 类<br /><br /> <xref:System.ComponentModel.Design.ServiceCreatorCallback> 委托<br /><br /> <xref:System.ComponentModel.Design.StandardCommands> 类<br /><br /> <xref:System.ComponentModel.Design.StandardToolWindows> 类<br /><br /> <xref:System.ComponentModel.DoubleConverter> 类<br /><br /> <xref:System.ComponentModel.DoWorkEventArgs> 类<br /><br /> <xref:System.ComponentModel.DoWorkEventHandler> 委托<br /><br /> <xref:System.ComponentModel.EnumConverter> 类<br /><br /> <xref:System.ComponentModel.EventDescriptor> 类<br /><br /> <xref:System.ComponentModel.EventDescriptorCollection> 类<br /><br /> <xref:System.ComponentModel.EventHandlerList> 类<br /><br /> <xref:System.ComponentModel.ExpandableObjectConverter> 类<br /><br /> <xref:System.ComponentModel.HandledEventArgs> 类<br /><br /> <xref:System.ComponentModel.HandledEventHandler> 委托<br /><br /> <xref:System.ComponentModel.InstanceCreationEditor> 类<br /><br /> <xref:System.ComponentModel.Int16Converter> 类<br /><br /> <xref:System.ComponentModel.Int32Converter> 类<br /><br /> <xref:System.ComponentModel.Int64Converter> 类<br /><br /> <xref:System.ComponentModel.InvalidAsynchronousStateException> 类<br /><br /> <xref:System.ComponentModel.InvalidEnumArgumentException> 类<br /><br /> <xref:System.ComponentModel.ISynchronizeInvoke.BeginInvoke%2A> 方法<br /><br /> <xref:System.ComponentModel.License> 类<br /><br /> <xref:System.ComponentModel.LicenseContext> 类<br /><br /> <xref:System.ComponentModel.LicenseException> 类<br /><br /> <xref:System.ComponentModel.LicenseManager> 类<br /><br /> <xref:System.ComponentModel.LicenseProvider> 类<br /><br /> <xref:System.ComponentModel.LicFileLicenseProvider> 类<br /><br /> <xref:System.ComponentModel.ListChangedEventArgs> 类<br /><br /> <xref:System.ComponentModel.ListChangedEventHandler> 委托<br /><br /> <xref:System.ComponentModel.ListSortDescription> 类<br /><br /> <xref:System.ComponentModel.ListSortDescriptionCollection> 类<br /><br /> <xref:System.ComponentModel.MaskedTextProvider> 类<br /><br /> <xref:System.ComponentModel.MemberDescriptor> 类<br /><br /> <xref:System.ComponentModel.MultilineStringConverter> 类<br /><br /> <xref:System.ComponentModel.NestedContainer> 类<br /><br /> <xref:System.ComponentModel.NullableConverter> 类<br /><br /> <xref:System.ComponentModel.ProgressChangedEventArgs> 类<br /><br /> <xref:System.ComponentModel.ProgressChangedEventHandler> 委托<br /><br /> <xref:System.ComponentModel.PropertyChangedEventArgs> 类<br /><br /> <xref:System.ComponentModel.PropertyChangedEventHandler> 委托<br /><br /> <xref:System.ComponentModel.PropertyDescriptor> 类<br /><br /> <xref:System.ComponentModel.PropertyDescriptorCollection> 类<br /><br /> <xref:System.ComponentModel.ReferenceConverter> 类<br /><br /> <xref:System.ComponentModel.RefreshEventArgs> 类<br /><br /> <xref:System.ComponentModel.RefreshEventHandler> 委托<br /><br /> <xref:System.ComponentModel.RunWorkerCompletedEventArgs> 类<br /><br /> <xref:System.ComponentModel.RunWorkerCompletedEventHandler> 委托<br /><br /> <xref:System.ComponentModel.SByteConverter> 类<br /><br /> <xref:System.ComponentModel.SingleConverter> 类<br /><br /> <xref:System.ComponentModel.StringConverter> 类<br /><br /> <xref:System.ComponentModel.SyntaxCheck> 类<br /><br /> <xref:System.ComponentModel.TimeSpanConverter> 类<br /><br /> <xref:System.ComponentModel.TypeConverter> 类<br /><br /> <xref:System.ComponentModel.TypeDescriptionProvider> 类<br /><br /> <xref:System.ComponentModel.TypeDescriptor> 类<br /><br /> <xref:System.ComponentModel.TypeListConverter> 类<br /><br /> <xref:System.ComponentModel.UInt16Converter> 类<br /><br /> <xref:System.ComponentModel.UInt32Converter> 类<br /><br /> <xref:System.ComponentModel.UInt64Converter> 类<br /><br /> <xref:System.ComponentModel.WarningException> 类<br /><br /> <xref:System.ComponentModel.Win32Exception> 类|  
|`System.Diagnostics`|<xref:System.Diagnostics.Debug.Listeners%2A?displayProperty=nameWithType> 属性<br /><br /> <xref:System.Diagnostics.Trace.Listeners%2A?displayProperty=nameWithType> 属性<br /><br /> <xref:System.Diagnostics.EventLog.SynchronizingObject%2A?displayProperty=nameWithType> 属性<br /><br /> <xref:System.Diagnostics.ConsoleTraceListener> 类<br /><br /> <xref:System.Diagnostics.DefaultTraceListener> 类<br /><br /> <xref:System.Diagnostics.DelimitedListTraceListener> 类<br /><br /> <xref:System.Diagnostics.EventLogTraceListener> 类<br /><br /> <xref:System.Diagnostics.PerformanceCounter> 类<br /><br /> <xref:System.Diagnostics.PerformanceCounterCategory> 类<br /><br /> <xref:System.Diagnostics.Process> 类<br /><br /> <xref:System.Diagnostics.ProcessStartInfo> 类<br /><br /> <xref:System.Diagnostics.TextWriterTraceListener> 类<br /><br /> <xref:System.Diagnostics.TraceListener> 类<br /><br /> <xref:System.Diagnostics.XmlWriterTraceListener> 类<br /><br /> <xref:System.Diagnostics.TraceSource.Listeners%2A?displayProperty=nameWithType> 属性|  
|`System.IO`|<xref:System.IO.Stream.Synchronized%2A?displayProperty=nameWithType> 方法<br /><br /> <xref:System.IO.TextReader.Synchronized%2A?displayProperty=nameWithType> 方法<br /><br /> <xref:System.IO.TextWriter.Synchronized%2A?displayProperty=nameWithType> 方法|  
|`System.Reflection.Emit`|<xref:System.Reflection.Emit.ConstructorBuilder> 类<br /><br /> <xref:System.Reflection.Emit.EventBuilder> 类<br /><br /> <xref:System.Reflection.Emit.FieldBuilder> 类<br /><br /> <xref:System.Reflection.Emit.MethodBuilder> 类<br /><br /> <xref:System.Reflection.Emit.CustomAttributeBuilder> 类<br /><br /> <xref:System.Reflection.Emit.MethodRental> 类<br /><br /> <xref:System.Reflection.Emit.ModuleBuilder> 类<br /><br /> <xref:System.Reflection.Emit.PropertyBuilder> 类<br /><br /> <xref:System.Reflection.Emit.TypeBuilder> 类<br /><br /> <xref:System.Reflection.Emit.UnmanagedMarshal> 类|  
|`System.Text`|<xref:System.Text.RegularExpressions.Group.Synchronized%2A?displayProperty=nameWithType> 方法<br /><br /> <xref:System.Text.RegularExpressions.Match.Synchronized%2A?displayProperty=nameWithType> 方法|  
|`System.Threading`|<xref:System.Threading.AutoResetEvent> 类<br /><br /> <xref:System.Threading.EventWaitHandle> 类<br /><br /> <xref:System.Threading.ManualResetEvent> 类<br /><br /> <xref:System.Threading.Monitor> 类<br /><br /> <xref:System.Threading.Mutex> 类<br /><br /> <xref:System.Threading.ReaderWriterLock> 类<br /><br /> <xref:System.Threading.Semaphore> 类<br /><br /> <xref:System.Threading.Thread.AllocateNamedDataSlot%2A?displayProperty=nameWithType> 方法<br /><br /> <xref:System.Threading.Thread.BeginCriticalRegion%2A?displayProperty=nameWithType> 方法<br /><br /> <xref:System.Threading.Thread.EndCriticalRegion%2A?displayProperty=nameWithType> 方法<br /><br /> <xref:System.Threading.Thread.FreeNamedDataSlot%2A?displayProperty=nameWithType> 方法<br /><br /> <xref:System.Threading.Thread.GetData%2A?displayProperty=nameWithType> 方法<br /><br /> <xref:System.Threading.Thread.Join%2A?displayProperty=nameWithType> 方法<br /><br /> <xref:System.Threading.Thread.SetApartmentState%2A?displayProperty=nameWithType> 方法<br /><br /> <xref:System.Threading.Thread.SetData%2A?displayProperty=nameWithType> 方法<br /><br /> <xref:System.Threading.Thread.SpinWait%2A?displayProperty=nameWithType> 方法<br /><br /> <xref:System.Threading.Thread.Start%2A?displayProperty=nameWithType> 方法<br /><br /> <xref:System.Threading.Thread.TrySetApartmentState%2A?displayProperty=nameWithType> 方法<br /><br /> <xref:System.Threading.ThreadPool> 类<br /><br /> <xref:System.Threading.Timer> 类|  
|`System.Timers`|<xref:System.Timers.Timer> 类|  
|`System.Web.Configuration`|<xref:System.Web.Configuration.MachineKeyValidationConverter> 类|  
|`System.Windows.Forms`|<xref:System.Windows.Forms.AutoCompleteStringCollection.SyncRoot%2A?displayProperty=nameWithType> 属性|  
  
## <a name="sql-server-permission-sets"></a>SQL Server 权限集  

 SQL Server 能让用户指定部署到数据库的代码可靠性要求。 将程序集上传到数据库中时，程序集创建者可为该程序集指定三个权限集（SAFE、EXTERNAL-ACCESS 或 UNSAFE）其中之一。  
  
|权限集|SAFE|EXTERNAL-ACCESS|UNSAFE|  
|--------------------|----------|----------------------|------------|  
|代码访问安全性|仅执行|执行和访问外部资源|非受限|  
|编程模型限制|是|是|无限制|  
|可验证性要求|是|是|否|  
|调用本机代码的能力|否|否|是|  
  
 SAFE 是最可靠和安全的模式，并且在允许的编程模型方面也具有相关的限制。 SAFE 代码具有高可靠性和安全性功能。 给 SAFE 程序集授予了足够的权限，以便运行、执行计算以及访问本地数据库。 SAFE 程序集需要具有可验证的类型安全性，并且不允许调用非托管代码。  
  
 EXTERNAL-ACCESS 提供了一个中间安全选项，允许代码访问数据库外部的资源，但仍具有 SAFE 的可靠性和安全性。  
  
 UNSAFE 用于仅能由数据库管理员创建的高度受信任的代码。 这类信任代码没有代码访问限制，可以调用非托管（本机）代码。  
  
 SQL Server 使用主机级别代码访问安全策略层来设置主机策略，此策略根据 SQL Server 目录中存储的权限集授予三组权限之一。 在数据库内运行的托管代码始终获取这些代码访问权限集中的一个。  
  
## <a name="programming-model-restrictions"></a>编程模型限制  

 SQL Server 中托管代码的编程模型需要无需使用跨多个调用的状态保留或跨多个用户会话共享状态的功能、过程和类型。 而且，如前文所述，共享状态的存在可导致能够影响应用程序的可伸缩性和可靠性的严重异常。  
  
 考虑到这些因素，SQL Server 不允许使用静态变量和静态数据成员。 对于 SAFE 和 EXTERNAL-ACCESS 程序集，SQL Server 将在 CREATE ASSEMBLY 时间检查程序集的元数据，如果发现使用静态数据成员和变量，则无法创建此类程序集。  
  
## <a name="see-also"></a>另请参阅

- <xref:System.Security.Permissions.HostProtectionAttribute>
- <xref:System.Security.Permissions.HostProtectionResource>
