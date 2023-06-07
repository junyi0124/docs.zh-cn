---
title: 运行时设置架构
ms.date: 03/30/2017
helpviewer_keywords:
- schema runtime settings
- configuration schema [.NET Framework], runtime settings
- runtime settings schema
ms.assetid: f04816ab-110d-4e28-9283-845d6d9a4a68
ms.openlocfilehash: 1797afe3e6347da1aef916d13be7678b7b8d4acf
ms.sourcegitcommit: cbacb5d2cebbf044547f6af6e74a9de866800985
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/05/2020
ms.locfileid: "89495168"
---
# <a name="run-time-settings-schema"></a>运行时设置架构

运行时设置由公共语言运行时用于配置面向 .NET Framework 的应用程序。

## <a name="the-runtime-section-and-its-parent-and-child-elements"></a>\<runtime>节及其父元素和子元素

[\<configuration>](../configuration-element.md)\
&nbsp;&nbsp;[\<runtime>](runtime-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<alwaysFlowImpersonationPolicy>](alwaysflowimpersonationpolicy-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<AppContextSwitchOverrides>](appcontextswitchoverrides-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<appDomainManagerAssembly>](appdomainmanagerassembly-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<appDomainManagerType>](appdomainmanagertype-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<appDomainResourceMonitoring>](appdomainresourcemonitoring-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<assemblyBinding>](assemblybinding-element-for-runtime.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[\<dependentAssembly>](dependentassembly-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[\<assemblyIdentity>](assemblyidentity-element-for-runtime.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[\<bindingRedirect>](bindingredirect-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[\<codeBase>](codebase-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[\<publisherPolicy>](publisherpolicy-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[\<probing>](probing-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[\<qualifyAssembly>](qualifyassembly-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[\<supportPortability>](supportportability-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<bypassTrustedAppStrongNames>](bypasstrustedappstrongnames-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<CompatSortNLSVersion>](compatsortnlsversion-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<developmentMode>](developmentmode-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<disableCachingBindingFailures>](disablecachingbindingfailures-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<disableCommitThreadStack>](disablecommitthreadstack-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<disableFusionUpdatesFromADManager>](disablefusionupdatesfromadmanager-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<EnableAmPmParseAdjustment>](enableampmparseadjustment-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<enforceFIPSPolicy>](enforcefipspolicy-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<etwEnable>](etwenable-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<forcePerformanceCounterUniqueSharedMemoryReads>](forceperformancecounteruniquesharedmemoryreads-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<gcAllowVeryLargeObjects>](gcallowverylargeobjects-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<gcConcurrent>](gcconcurrent-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<GCCpuGroup>](gccpugroup-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<GCHeapAffinitizeMask>](gcheapaffinitizemask-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<GCHeapCount>](gcheapcount-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<GCLOHThreshold>](gclohthreshold-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<GCNoAffinitize>](gcnoaffinitize-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<gcServer>](gcserver-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<generatePublisherEvidence>](generatepublisherevidence-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<legacyCorruptedStateExceptionsPolicy>](legacycorruptedstateexceptionspolicy-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<legacyImpersonationPolicy>](legacyimpersonationpolicy-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<loadfromRemoteSources>](loadfromremotesources-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<NetFx40_LegacySecurityPolicy>](netfx40-legacysecuritypolicy-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<NetFx40_PInvokeStackResilience>](netfx40-pinvokestackresilience-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<NetFx45_CultureAwareComparerGetHashCode_LongStrings>](netfx45-cultureawarecomparergethashcode-longstrings-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<PreferComInsteadOfManagedRemoting>](prefercominsteadofmanagedremoting-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<relativeBindForResources>](relativebindforresources-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<shadowCopyVerifyByTimeStamp>](shadowcopyverifybytimestamp-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<Thread_UseAllCpuGroups>](thread-useallcpugroups-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<ThrowUnobservedTaskExceptions>](throwunobservedtaskexceptions-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<TimeSpan_LegacyFormatMode>](runtime-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<useLegacyJit>](uselegacyjit-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<UseRandomizedStringHashAlgorithm>](userandomizedstringhashalgorithm-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<UseSmallInternalThreadStacks>](usesmallinternalthreadstacks-element.md)\
&nbsp;&nbsp;[\<system.runtime.caching>](system-runtime-caching-element-cache-settings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<memoryCache>](memorycache-element-cache-settings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[\<namedCaches>](namedcaches-element-cache-settings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[\<add>](add-element-for-namedcaches.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[\<clear>](clear-element-for-namedcaches.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[\<remove>](remove-element-for-namedcaches.md)

## <a name="alphabetical-list-of-runtime-elements"></a>按字母顺序排列的 \<runtime> 元素列表

|元素|说明|
|-------------|-----------------|
|[\<add>](add-element-for-namedcaches.md)|向内存缓存的 `namedCaches` 集合添加一个命名的缓存。|
|[\<alwaysFlowImpersonationPolicy>](alwaysflowimpersonationpolicy-element.md)|指定 Windows 标识始终流经异步点，而不考虑执行模拟的方式。|
|[\<AppContextSwitchOverrides>](appcontextswitchoverrides-element.md)|定义 <xref:System.AppContext> 类使用的一个或多个开关，用于提供新功能的选择退出机制。|
|[\<appDomainManagerAssembly>](appdomainmanagerassembly-element.md)|指定为过程中的默认应用程序域提供应用程序域管理器的程序集。|
|[\<appDomainManagerType>](appdomainmanagertype-element.md)|指定用作默认应用程序域的应用程序域管理器的类型。|
|[\<appDomainResourceMonitoring>](appdomainresourcemonitoring-element.md)|指示运行时在过程的生命周期过程中收集所有应用程序域的统计数据。|
|[\<assemblyBinding>](assemblybinding-element-for-runtime.md)|包含有关程序集版本重定向和程序集位置的信息。|
|[\<assemblyIdentity>](assemblyidentity-element-for-runtime.md)|包含有关程序集的标识信息。|
|[\<bindingRedirect>](bindingredirect-element.md)|将一个程序集版本重定向到另一个版本。|
|[\<bypassTrustedAppStrongNames>](bypasstrustedappstrongnames-element.md)|指定是否应绕过对受信任的程序集进行强名称验证。|
|[\<clear>](clear-element-for-namedcaches.md)|清除内存缓存的 `namedCaches` 集合。|
|[\<codeBase>](codebase-element.md)|指定运行时可以找到程序集的位置。|
|[\<CompatSortNLSVersion>](compatsortnlsversion-element.md)|指定在执行字符串比较时，运行时应使用旧排序行为|
|[\<dependentAssembly>](dependentassembly-element.md)|封装每个程序集的绑定策略和程序集位置。|
|[\<developmentMode>](developmentmode-element.md)|指定运行时是否搜索由 DEVPATH 环境变量指定的目录中的程序集。|
|[\<disableCachingBindingFailures>](disablecachingbindingfailures-element.md)|指定是否禁用绑定故障缓存，这是 .NET Framework 2.0 中的默认行为。|
|[\<disableCommitThreadStack>](disablecommitthreadstack-element.md)|指定在线程启动时是否提交完整线程堆栈。|
|[\<disableFusionUpdatesFromADManager>](disablefusionupdatesfromadmanager-element.md)|指定是否禁用允许运行时主机为应用程序域重写配置设置的默认行为。|
|[\<EnableAmPmParseAdjustment>](enableampmparseadjustment-element.md)|确定日期和时间分析方法是否使用调整后的一组规则来分析仅包含天、月、小时和 AM/PM 指示符的日期字符串。|
|[\<enforceFIPSPolicy>](enforcefipspolicy-element.md)|指定是否强制执行以下计算机配置要求：加密算法必须符合美国联邦信息处理标准 (FIPS)。|
|[\<etwEnable>](etwenable-element.md)|指定是否为公共语言运行时事件启用 Windows 事件跟踪 (ETW)。|
|[\<forcePerformanceCounterUniqueSharedMemoryReads>](forceperformancecounteruniquesharedmemoryreads-element.md)|指定 PerfCounter.dll 是否使用 .NET Framework 版本 1.1 应用程序中的 CategoryOptions 注册表设置，以确定是否加载来自特定于类别的共享内存或全局内存的性能计数器数据。|
|[\<gcAllowVeryLargeObjects>](gcallowverylargeobjects-element.md)|在 64 位平台上，启用总大小大于 2 千兆字节 (GB) 的数组。|
|[\<gcConcurrent>](gcconcurrent-element.md)|指定运行时是否并发运行服务器垃圾回收。|
|[\<GCCpuGroup>](gccpugroup-element.md)|指定垃圾回收是否支持多个 CPU 组。|
|[\<GCHeapAffinitizeMask>](gcheapaffinitizemask-element.md)|定义 GC 堆和单个处理器之间的关联。|
|[\<GCHeapCount>](gcheapcount-element.md)|指定用于服务器垃圾回收的堆/线程数。  |
|[\<GCLOHThreshold>](gclohthreshold-element.md)|指定导致对象在大型对象堆上 (LOH) 的阈值大小。|
|[\<GCNoAffinitize>](gcnoaffinitize-element.md)|指定是否关联服务器 GC 线程与 Cpu。|
|[\<gcServer>](gcserver-element.md)|指定公共语言运行时是否运行服务器垃圾回收。|
|[\<generatePublisherEvidence>](generatepublisherevidence-element.md)|指定运行时是否使用代码访问安全性 (CAS) 发布服务器策略。|
|[\<legacyCorruptedStateExceptionsPolicy>](legacycorruptedstateexceptionspolicy-element.md)|指定运行时是否允许托管的代码捕获访问冲突和其他损坏状态异常。|
|[\<legacyImpersonationPolicy>](legacyimpersonationpolicy-element.md)|指定 Windows 标识不流经异步点，而不考虑当前线程上执行上下文的流设置。|
|[\<loadfromRemoteSources>](loadfromremotesources-element.md)|指定是否将来自远程源的程序集加载为完全信任。|
|[\<memoryCache>](memorycache-element-cache-settings.md)|定义一个用于配置基于 <xref:System.Runtime.Caching.MemoryCache> 类的缓存的元素。|
|[\<namedCaches>](namedcaches-element-cache-settings.md)|包含 `namedCache` 实例的配置设置的集合。|
|[\<NetFx40_LegacySecurityPolicy>](netfx40-legacysecuritypolicy-element.md)|指定运行时是否使用旧版代码访问安全性 (CAS) 策略。|
|[\<NetFx40_PInvokeStackResilience>](netfx40-pinvokestackresilience-element.md)|指定运行时是否以减慢托管和非托管代码之间的转换速度为代价，在运行时自动修复不正确的平台调用声明。|
|[\<NetFx45_CultureAwareComparerGetHashCode_LongStrings>](netfx45-cultureawarecomparergethashcode-longstrings-element.md)|指定运行时是否使用固定的内存量来计算 <xref:System.StringComparer.GetHashCode%2A?displayProperty=nameWithType> 方法的哈希代码。|
|[\<PreferComInsteadOfManagedRemoting>](prefercominsteadofmanagedremoting-element.md)|指定运行时将使用 COM 互操作来代替跨应用程序域边界的远程。|
|[\<probing>](probing-element.md)|指定加载程序集时运行时搜索的子目录。|
|[\<publisherPolicy>](publisherpolicy-element.md)|指定运行时是否使用发布者策略。|
|[\<qualifyAssembly>](qualifyassembly-element.md)|指定使用部分名称时应动态加载的程序集全名。|
|[\<relativeBindForResources>](relativebindforresources-element.md)|优化附属程序集的探测。|
|[\<remove>](remove-element-for-namedcaches.md)|从内存缓存的 `namedCaches` 集合中删除一个命名的缓存条目。|
|[\<runtime>](runtime-element.md)|包含有关程序集绑定和垃圾回收行为的信息。|
|[\<shadowCopyTimeStampVerification>](shadowcopyverifybytimestamp-element.md)|指定卷影复制是否使用 .NET Framework 4 中引入的默认启动行为，或恢复为 .NET Framework 早期版本的启动行为。|
|[\<supportPortability>](supportportability-element.md)|通过禁用将程序集视为等效于应用程序可移植性用途的默认行为来指定应用程序可以在两种不同的 .NET Framework 实现中引用同一程序集。|
|[\<system.runtime.caching>](system-runtime-caching-element-cache-settings.md)|提供默认内存中对象缓存的配置信息。|
|[\<Thread_UseAllCpuGroups>](thread-useallcpugroups-element.md)|指定运行时是否跨所有 CPU 组分发托管的线程。|
|[\<ThrowUnobservedTaskExceptions>](throwunobservedtaskexceptions-element.md)|指定未经处理的任务异常是否应终止正在运行的进程。|
|[\<TimeSpan_LegacyFormatMode>](runtime-element.md)|指定运行时是否使用 <xref:System.TimeSpan> 值的旧格式。|
|[\<useLegacyJit>](uselegacyjit-element.md)|确定公共语言运行时是否使用实时编译的旧版 64 位 JIT 编译器。|
|[\<UseRandomizedStringHashAlgorithm>](userandomizedstringhashalgorithm-element.md)|指定运行时是否按应用程序域计算字符串的哈希代码。|
|[\<UseSmallInternalThreadStacks>](usesmallinternalthreadstacks-element.md)|请求运行时在创建内部使用的某些线程时使用显式堆栈大小，而不是默认堆栈大小。|

## <a name="see-also"></a>请参阅

- [配置文件架构](../index.md)
- [禁用并发垃圾回收](gcconcurrent-element.md#to-disable-background-garbage-collection)
- [重定向程序集版本](../../redirect-assembly-versions.md)
