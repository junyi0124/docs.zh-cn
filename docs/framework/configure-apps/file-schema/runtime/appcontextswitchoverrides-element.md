---
title: AppContextSwitchOverrides 元素
ms.date: 04/18/2019
helpviewer_keywords:
- AppContextSwitchOverrides
- compatibility switches
- configuration switches
- configuration
ms.assetid: 4ce07f47-7ddb-4d91-b067-501bd8b88752
ms.openlocfilehash: ab74886edcc86c900c56017867a3b81c9cb7886e
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91176144"
---
# <a name="appcontextswitchoverrides-element"></a>\<AppContextSwitchOverrides> 元素

定义 <xref:System.AppContext> 类使用的一个或多个开关，用于提供新功能的选择退出机制。

[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<runtime>**](runtime-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;**\<AppContextSwitchOverrides>**

## <a name="syntax"></a>语法

```xml
<AppContextSwitchOverrides value="name1=value1[[;name2=value2];...]" />
```

## <a name="attributes-and-elements"></a>特性和元素

 下列各节描述了特性、子元素和父元素。

### <a name="attributes"></a>特性

|属性|描述|
|---------------|-----------------|
|`value`|必需的特性。<br /><br /> 定义一个或多个开关名称及其关联的布尔值。|

### <a name="value-attribute"></a>值特性

|值|描述|
|-----------|-----------------|
|"名称 = 值"|预定义的开关名称及其值 (`true` 或 `false`) 。 多个开关名称/值对由分号分隔 ( ";") 。 有关 .NET Framework 支持的预定义开关名称的列表，请参阅 "备注" 部分。|

### <a name="child-elements"></a>子元素

 无。

### <a name="parent-elements"></a>父元素

|元素|描述|
|-------------|-----------------|
|`configuration`|公共语言运行时和 .NET Framework 应用程序所使用的每个配置文件中的根元素。|
|`runtime`|包含有关运行时初始化选项的信息。|

## <a name="remarks"></a>备注

 从 .NET Framework 4.6 开始， `<AppContextSwitchOverrides>` 配置文件中的元素允许 API 的调用方确定其应用是否可以利用新功能或保持与以前版本的库的兼容性。 例如，如果 API 的行为在两个版本的库中发生了更改，则 `<AppContextSwitchOverrides>` 元素允许该 api 的调用方在支持新功能的库版本上选择禁用新行为。 对于在 .NET Framework 中调用 Api 的应用程序， `<AppContextSwitchOverrides>` 如果应用程序在包含该功能的 .NET Framework 版本上运行，则该元素还可以允许其应用定位于 .NET Framework 早期版本的调用方选择新的功能。

 `value`元素的属性 `<AppContextSwitchOverrides>` 包含一个字符串，该字符串由一个或多个以分号分隔的名称/值对组成。  每个名称都标识一个兼容性开关，其对应的值为布尔值 (`true` 或 `false`) ，指示是否设置了该开关。 默认情况下，开关是 `false` ，库提供新功能。 如果开关设置 (为，则它们仅提供以前的功能，其值为 `true`) 。 这允许库为现有 API 提供新行为，同时允许依赖于先前行为的调用方选择不使用新功能。

.NET Framework 支持以下开关：

|交换机名称|描述|已引入|
|-----------------|-----------------|----------------|
|`Switch.MS.Internal.`<br/>`DoNotApplyLayoutRoundingToMarginsAndBorderThickness`|控制 Windows Presentation Foundation 是否对控件布局使用旧算法。 有关详细信息，请参阅[缓解：WPF 布局](../../../migration-guide/mitigation-wpf-layout.md)。|.NET Framework 4.6|
|`Switch.MS.Internal.`<br/>`UseSha1AsDefaultHashAlgorithmForDigitalSignatures`|控制 PackageDigitalSignatureManager 用于为包的部分签名的默认算法是 SHA1 还是 SHA256。<br>由于与 SHA1 冲突，Microsoft 建议使用 SHA256。|.NET Framework 4.7.1|
|`Switch.System.Activities.`<br/>`UseMD5CryptoServiceProviderForWFDebugger`|如果设置为 `false` ，则允许在启用 FIPS 时通过 Visual Studio 调试基于 XAML 的工作流项目。 如果没有此 <xref:System.NullReferenceException> 方法，则会在对 system.exception 程序集中的方法的调用中引发。|.NET Framework 4.7|
|`Switch.System.Activities.`<br/>`UseMD5ForWFDebugger`|控制调试器中工作流实例的校验和是否使用 MD5 或 SHA1。 | .NET Framework 4.7|
|`Switch.System.Activities.`<br/>`UseSHA1HashForDebuggerSymbols`|控制工作流校验和是否哈希使用 .NET Framework 4.7 () 中引入为默认值的 SHA1 算法 `true` ，或是否使用默认 SHA256 算法，该算法在 .NET Framework 4.8 () 中引入为默认值 `false` 。<br>由于与 SHA1 冲突，Microsoft 建议使用 SHA256。|.NET Framework 4.8|
|`Switch.System.Diagnostics.`<br/>`IgnorePortablePDBsInStackTraces`|控制在使用便携式 Pdb 时，堆栈跟踪是否获得，可以包括源文件和行信息。 `false` 包括源文件和行信息;否则为 `true` 。|.NET Framework 4.7.2|
|`Switch.System.Drawing.`<br/>`DontSupportPngFramesInIcons`|控制 <xref:System.Drawing.Icon.ToBitmap%2A?displayProperty=nameWithType> 方法在 <xref:System.Drawing.Icon> 对象具有 PNG 帧时是否引发异常。 有关详细信息，请参阅[缓解：图标对象中的 PNG 帧](../../../migration-guide/mitigation-png-frames-in-icon-objects.md)。|.NET Framework 4.6|
|`Switch.System.Drawing.Text.`<br/>`DoNotRemoveGdiFontsResourcesFromFontCollection`|确定 <xref:System.Drawing.Text.PrivateFontCollection?displayProperty=nameWithType> 通过方法将对象添加到集合时是否正确释放对象 <xref:System.Drawing.Text.PrivateFontCollection.AddFontFile(System.String)?displayProperty=nameWithType> 。 `true` 若要保留旧行为，则为;处理 `false` 所有专用字体对象。 |.NET Framework 4.7.2|
|`Switch.System.Drawing.Printing.`<br>`OptimizePrintPreview`|控制是否为 <xref:System.Windows.Forms.PrintPreviewDialog> 网络打印机优化的性能。 有关详细信息，请参阅 [PrintPreviewDialog 控件概述](/dotnet/desktop/winforms/controls/printpreviewdialog-control-overview-windows-forms)。|.NET Framework 4.6|
|`Switch.System.Globalization.EnforceJapaneseEraYearRanges`|控制是否强制执行日本历纪元的年份范围检查。 `true` 若要强制执行年份范围检查并 `false` 禁用它们 (默认行为) 。 有关详细信息，请参阅使用 [日历](../../../../standard/datetime/working-with-calendars.md)。|.NET Framework 4.6|
|`Switch.System.Globalization.EnforceLegacyJapaneseDateParsing`|控制在分析操作中是否仅将 "1" 识别为日本历时代的第一年。 `true` 仅识别 "1"; `false` 若要识别 "1" 或 Gannen (默认行为) 。 有关详细信息，请参阅使用 [日历](../../../../standard/datetime/working-with-calendars.md)。|.NET Framework 4.6|
|`Switch.System.Globalization.FormatJapaneseFirstYearAsANumber`|控制日本历时代的第一年在格式设置操作中是否表示为 "1" 或 Gannen。 `true` 将纪元的第一年的格式设置为 "1"; `false` 若要将其设置为 Gannen (默认行为) 。 有关详细信息，请参阅使用 [日历](../../../../standard/datetime/working-with-calendars.md)。|.NET Framework 4.6|
|`Switch.System.Globalization.NoAsyncCurrentCulture`|控制异步操作是否不从调用线程的上下文中流动。 有关详细信息，请参阅 [跨任务的 CurrentCulture 和 CurrentUICulture 流](../../../migration-guide/retargeting/4.5.2-4.6.md#currentculture-and-currentuiculture-flow-across-tasks)。|.NET Framework 4.6|
|`Switch.System.IdentityModel.`<br/>`DisableMultipleDNSEntriesInSANCertificate`|控制方法是否 <xref:System.IdentityModel.Claims.X509CertificateClaimSet.FindClaims%2A?displayProperty=nameWithType> 尝试仅将声明类型与最后一个 DNS 条目进行匹配。 有关详细信息，请参阅[缓解：X509CertificateClaimSet.FindClaims 方法](../../../migration-guide/mitigation-x509certificateclaimset-findclaims-method.md)。|.NET Framework 4.6.1|
|`Switch.System.IdentityModel.`<br/>`EnableCachedEmptyDefaultAuthorizationContext`|控制是否允许 AuthorizationContext 返回可变对象。|.NET Framework 4.6|
|`Switch.System.IO.BlockLongPaths`|控制超过 `MAX_PATH` 260 个字符的 (路径是否) 引发 <xref:System.IO.PathTooLongException> 。 有关详细信息，请参阅 [长路径支持](../../../migration-guide/retargeting/4.6.1-4.6.2.md#long-path-support)。|.NET Framework 4.6.2|
|`Switch.System.IO.Compression.`<br/>`DoNotUseNativeZipLibraryForDecompression`|控制类是否使用本机操作系统例程进行解压缩 <xref:System.IO.Compression.DeflateStream> 。 `false` 使用本机 Api; `true` 使用该 <xref:System.IO.Compression.DeflateStream> 实现。|.NET Framework 4.7.2|
|`Switch.System.IO.Compression.ZipFile.`<br/>`UseBackslash`|使用反斜杠 ( " \\ " ) 而不是正斜杠 ( "/" ) 作为属性中的路径分隔符 <xref:System.IO.Compression.ZipArchiveEntry.FullName%2A?displayProperty=nameWithType> 。 有关详细信息，请参阅  [缓解： ZipArchiveEntry 路径分隔符](../../../migration-guide/mitigation-ziparchiveentry-fullname-path-separator.md)。|.NET Framework 4.6.1|
|`Switch.System.IO.Ports.`<br/>`DoNotCatchSerialStreamThreadExceptions`|控制在用流创建的后台线程上引发的操作系统异常是否 <xref:System.IO.Ports.SerialPort> 终止进程。|.NET Framework 4.7.1|
|`Switch.System.IO.`<br/>`UseLegacyPathHandling`|控制是否使用旧版路径规范化并且和方法支持 URI 路径 <xref:System.IO.Path.GetDirectoryName%2A?displayProperty=nameWithType> <xref:System.IO.Path.GetPathRoot%2A?displayProperty=nameWithType> 。 有关详细信息，请参阅 [缓解：路径规范化](../../../migration-guide/mitigation-path-normalization.md) 和 [缓解：路径冒号检查](../../../migration-guide/mitigation-path-colon-checks.md)。|.NET Framework 4.6.2|
|`Switch.System.`<br/>`MemberDescriptorEqualsReturnsFalseIfEquivalent`|控制是否对相等性测试将 <xref:System.ComponentModel.MemberDescriptor.Category%2A?displayProperty=nameWithType> 一个对象的属性与 <xref:System.ComponentModel.MemberDescriptor.Description%2A?displayProperty=nameWithType> 第二个对象的属性进行比较。 有关详细信息，请参阅 [system.componentmodel.memberdescriptor.equals 的不正确实现](../../../migration-guide/retargeting/4.6.1-4.6.2.md#incorrect-implementation-of-memberdescriptorequals)。|.NET Framework 4.6.2|
 `Switch.System.Net.`<br/>`DontCheckCertificateEKUs`|禁用 (EKU) 对象标识符 (OID) 验证的证书增强型密钥用法。 增强型密钥使用 (EKU) 扩展是指示使用密钥的应用程序的对象标识符 (OID) 的集合。|.NET Framework 4.6|
|`Switch.System.Net.`<br/>`DontEnableSchSendAuxRecord`|禁用对 SSL/TLS 的 TLS 1.0 浏览器攻击 (BEAST) 通过禁用 SCH_SEND_AUX_RECORD 的使用来进行缓解。|.NET Framework 4.6|
|`Switch.System.Net.`<br/>`DontEnableSchUseStrongCrypto`|控制 <xref:System.Net.ServicePointManager?displayProperty=nameWithType> 和类是否 <xref:System.Net.Security.SslStream?displayProperty=nameWithType> 可以使用 SSL 3.0 协议。 有关详细信息，请参阅[缓解：TLS 协议](../../../migration-guide/mitigation-tls-protocols.md)。|.NET Framework 4.6|
|`Switch.System.Net.`<br/>`DontEnableSystemDefaultTlsVersions`|禁用 SystemDefault TLS 版本恢复为默认的 Tls12、Tls11、Tls。|.NET Framework 4.7|
|`Switch.System.Net.`<br/>`DontEnableTlsAlerts`|禁用 System.net.security.sslstream TLS 服务器端警报。|.NET Framework 4.7|
|`Switch.System.Runtime.InteropServices.`<br/>`DoNotMarshalOutByrefSafeArrayOnInvoke`|控制 COM 互操作事件上的 ByRef SafeArray 参数是否封送回本机代码 (`false`) 或是否禁用了封送回本机代码 (`true`) 。|.NET Framework 4.8|
|`Switch.System.Runtime.Serialization.`<br/>`DoNotUseECMAScriptV6EscapeControlCharacter` |控制 [DataContractJsonSerializer](xref:System.Runtime.Serialization.Json.DataContractJsonSerializer) 是否根据 ECMAScript V6 和 V8 标准序列化一些控制字符。 有关详细信息，请参阅[缓解：使用 DataContractJsonSerializer 对控制字符进行序列化](../../../migration-guide/mitigation-serialization-control-characters.md)| .NET Framework 4.7 |
|`Switch.System.Runtime.Serialization.`<br/>`DoNotUseTimeZoneInfo`|控制是否 <xref:System.Runtime.Serialization.Json.DataContractJsonSerializer> 支持多个调整，或仅支持一个时区的单次调整。 如果为 `true` ，则它使用 <xref:System.TimeZoneInfo> 类型来序列化和反序列化日期和时间数据; 否则，它使用 <xref:System.TimeZone> 类型，该类型不支持多个调整规则。|.NET Framework 4.6.2|
|`Switch.System.Runtime.Serialization.UseNewMaxArraySize`|控制 <xref:System.Runtime.Serialization.ObjectManager?displayProperty=nameWithType> 在对象序列化和反序列化过程中是否使用更大的数组大小。 将此开关设置为可 `true` 通过类型（如）提高大型对象图的序列化和反序列化的性能 <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter> 。 |.NET Framework 4.7.2|
|`Switch.System.Security.ClaimsIdentity.`<br/>`SetActorAsReferenceWhenCopyingClaimsIdentity`|控制 <xref:System.Security.Claims.ClaimsIdentity.%23ctor%28System.Security.Principal.IIdentity%29> 构造函数是否使用现有对象引用来设置新对象的 <xref:System.Security.Claims.ClaimsIdentity.Actor%2A?displayProperty=nameWithType> 属性。 有关详细信息，请参阅[缓解：ClaimsIdentity 构造函数](../../../migration-guide/retargeting/4.6.1-4.6.2.md)。|.NET Framework 4.6.2|
|`Switch.System.Security.Cryptography.`<br/>`AesCryptoServiceProvider.DontCorrectlyResetDecryptor`|控制尝试重用解密器是否会 <xref:System.Security.Cryptography.AesCryptoServiceProvider> 引发 <xref:System.Security.Cryptography.CryptographicException> 。 有关详细信息，请参阅 [AesCryptoServiceProvider 解密器提供可重用的转换](../../../migration-guide/retargeting/4.6.1-4.6.2.md#aescryptoserviceprovider-decryptor-provides-a-reusable-transform)。|.NET Framework 4.6.2|
|`Switch.System.Security.Cryptography.`<br/>`DoNotAddrOfCspParentWindowHandle`|控制 [CspParameters system.security.cryptography.cspparameters.parentwindowhandle](xref:System.Security.Cryptography.CspParameters.ParentWindowHandle) 属性的值是否为表示窗口句柄的内存位置的 [IntPtr](xref:System.IntPtr) ，或者它是否是 HWND)  (的窗口句柄。 有关详细信息，请参阅[缓解：应向 CspParameters.ParentWindowHandle 分配 HWND](../../../migration-guide/retargeting/4.6.2-4.7.md#cspparametersparentwindowhandle-now-expects-hwnd-value)。 |.NET Framework 4.7|
|`Switch.System.Security.Cryptography.`<br/>`UseLegacyFipsThrow`|控制在 FIPS 模式下使用托管加密类是引发 <xref:System.Security.Cryptography.CryptographicException> (`true`) 还是依赖于系统库 (`false`) 实现。|.NET Framework 4.8|
|`Switch.System.Security.Cryptography.Pkcs.`<br/>`UseInsecureHashAlgorithms`|确定某些 SignedCMS 操作的默认值是 SHA1 还是 SHA256。<br>由于与 SHA1 冲突，Microsoft 建议使用 SHA256。|.NET Framework 4.7.1|
|`Switch.System.Security.Cryptography.X509Certificates.`<br/>`ECDsaCertificateExtensions.UseLegacyPublicKeyReader`|控制方法是否 <xref:System.Security.Cryptography.X509Certificates.ECDsaCertificateExtensions.GetECDsaPublicKey%2A?displayProperty=nameWithType> 正确地处理操作系统)  (支持的所有已命名曲线 `false` ，或是否恢复到旧行为。|.NET Framework 4.8|
|`Switch.System.Security.Cryptography.Xml.`<br/>`UseInsecureHashAlgorithms`|确定某些 SignedXML 操作的默认值是 SHA1 还是 SHA256。<br>由于与 SHA1 冲突，Microsoft 建议使用 SHA256。|.NET Framework 4.7.1|
|`Switch.System.ServiceModel.`<br/>`AllowUnsignedToHeader`|确定 `TransportWithMessageCredential` 安全模式是否允许带有无符号 "to" 标头的消息。 这是一个可选的开关。 有关详细信息，请参阅 [.NET Framework 4.6.1 中的运行时更改](../../../migration-guide/runtime/4.5.2-4.6.1.md#windows-communication-foundation-wcf)。|.NET Framework 4.6.1|
|`Switch.System.ServiceModel.`<br/>`DisableAddressHeaderCollectionValidation`>|<xref:System.ServiceModel.Channels.AddressHeaderCollection.%23ctor(System.Collections.Generic.IEnumerable{System.ServiceModel.Channels.AddressHeader})> <xref:System.ArgumentException> 如果其中一个元素为，则控制构造函数是否引发 `null` 。|.NET Framework 4.7.1|
|`Switch.System.ServiceModel.`<br />`DisableCngCertificates`|确定尝试将 X509 证书与 CSG 密钥存储提供程序一起使用是否会引发异常。 有关详细信息，请参阅 [WCF 传输安全性支持使用 CNG 存储的证书](../../../migration-guide/retargeting/4.6.1-4.6.2.md#wcf-transport-security-supports-certificates-stored-using-cng)。|.NET Framework 4.6.1|
|`Switch.System.ServiceModel.`<br/>`DisableExplicitConnectionCloseHeader`|将 HTTP 传输与自承载服务一起使用时，将此值设置为 `true` 会导致 WCF 忽略向 `Connection: close` 请求的响应标头添加标头的应用程序。 如果将此值设置为， `false` 则可将 `Connection: close` 标头添加到响应标头，这会导致在发送响应后关闭请求套接字。|.NET Framework 4.6|
|`Switch.System.ServiceModel.`<br/>`DisableOperationContextAsyncFlow`|处理因将可重入服务的实例限制为一次执行的单个线程而导致的死锁。|.NET Framework 4.6.2|
|`Switch.System.ServiceModel.`<br/>`DisableUsingServicePointManagerSecurityProtocols`|连同 `Switch.System.Net.DontEnableSchUseStrongCrypto` ，还决定 WCF 消息安全是否使用 tls 1.1 和 tls 1.2。|.NET Framework 4.7 |
|`Switch.System.ServiceModel.`<br/>`DontEnableSystemDefaultTlsVersions`|值 `false` 设置为允许操作系统选择协议的默认配置。 值为时， `true` 将默认值设置为最高可用协议。  (在以前的 framework 版本的服务分支中也可用) |.NET Framework 4.7.1|
|`Switch.System.ServiceModel.`<br/>`UseSha1InMsmqEncryptionAlgorithm`|确定 WCF 中 MSMQ 消息的默认消息签名算法是 SHA1 还是 SHA256。<br>由于与 SHA1 冲突，Microsoft 建议使用 SHA256。|.NET Framework 4.7.1|
|`Switch.System.ServiceModel.`<br/>`UseSha1InPipeConnectionGetHashAlgorithm`|控制 WCF 是使用 SHA1 还是 SHA256 哈希为命名管道生成随机名称。<br>由于与 SHA1 冲突，Microsoft 建议使用 SHA256。|.NET Framework 4.7.1|
|`Switch.System.ServiceModel.Internals`<br/>`IncludeNullExceptionMessageInETWTrace`|控制在异常消息为 null 时是否引发 [NullReferenceException](xref:System.NullReferenceException) 。|.NET Framework 4.7|
|`Switch.System.ServiceProcess.`<br/>`DontThrowExceptionsOnStart`|控制是否将在服务启动时引发的异常传播到方法的调用方 <xref:System.ServiceProcess.ServiceBase.Run%2A?displayProperty=nameWithType> 。|.NET Framework 4.7.1|
|`Switch.System.Threading.UseNetCoreTimer`|控制 <xref:System.Threading.Timer> 实例是否利用大规模环境的性能改进。 如果 `true` 为，则启用性能改善; 如果 `false` (默认值) ，则会被禁用。|.NET Framework 4.8|
|`Switch.System.Uri.`<br/>`DontEnableStrictRFC3986ReservedCharacterSets`|确定某些有时被解码的百分号编码字符现在是否一致地进行了编码。 如果 `true` 为，则对其进行解码; 否则为 `false` 。|.NET Framework 4.7.2|
|`Switch.System.Uri.`<br/>`DontKeepUnicodeBidiFormattingCharacters`|确定 Uri 中 Unicode 双向字符的处理。 `true` 从 Uri 中提取它们; `false` 保留并对其进行百分比编码。|.NET Framework 4.7.2|
|`Switch.System.Windows.Controls.Grid.`<br/>`StarDefinitionsCanExceedAvailableSpace` |确定 Windows Presentation Foundation 将旧算法应用于 (`true`) 或新算法 (`false`) 将空间分配到 \* 列。 有关详细信息，请参阅[缓解：网格控件向 *-列分配空间](../../../migration-guide/retargeting/4.6.2-4.7.md#wpf-grid-allocation-of-space-to-star-columns)。 |.NET Framework 4.7 |
|`Switch.System.Windows.Controls.TabControl.`<br/>`SelectionPropertiesCanLagBehindSelectionChangedEvent`|控制选择器或选项卡控件是否始终在引发选择更改事件之前更新其选定值属性的值。|.NET Framework 4.7.1|
|`Switch.System.Windows.Controls.Text.`<br/>`UseAdornerForTextboxSelectionRendering`|确定基于非装饰器的选择呈现是否可用于 <xref:System.Windows.Controls.TextBox> 和控件， <xref:System.Windows.Controls.PasswordBox> 以防止封闭像素文本 (`false`) ，或者是否仅在装饰器层中呈现文本 (`true`) 。|.NET Framework 4.7.2|
|`Switch.System.Windows.Data.Binding.`<br/>`IListIndexerHidesCustomIndexer`|控制是否使用错误地 (自定义 IList 索引器 `true`) 或正确 (`false` <xref:System.Windows.Data.Binding?displayProperty=nameWithType> 类) 。|.NET Framework 4.8|
|`Switch.System.Windows.DoNotScaleForDpiChanges`|确定在每个系统上是否发生了 DPI 更改， (值为 `false`) 或每个监视器 (值 `true`) 。|.NET Framework 4.6.2|
|`Switch.System.Windows.`<br/>`DoNotUsePresentationDpiCapabilityTier2OrGreater`|控制在按监视器识别模式下运行 WPF 时，是否对中的控件大小进行了改进 <xref:System.Windows.Interop.HwndHost?displayProperty=nameWithType> ， (`true`) 或启用了 (`false`) 。|.NET Framework 4.8|
|`Switch.System.Windows.Forms.`<br/>`DomainUpDown.UseLegacyScrolling`|确定在控件文本存在时开发人员是否需要专门处理 <xref:System.Windows.Forms.DomainUpDown.UpButton?displayProperty=nameWithType> 操作。 `true` 若要处理 <xref:System.Windows.Forms.DomainUpDown.UpButton> 操作，则为; 如果 `false` <xref:System.Windows.Forms.DomainUpDown.UpButton?displayProperty=nameWithType> 和操作正确地同步，则为 <xref:System.Windows.Forms.DomainUpDown.DownButton?displayProperty=nameWithType> 。|.NET Framework 4.7.2|
|`Switch.System.Windows.Forms.`<br />`DontSupportReentrantFilterMessage`|在代码中，允许自定义 <xref:System.Windows.Forms.IMessageFilter.PreFilterMessage%2A?displayProperty=nameWithType> 实现安全筛选消息，而不会在调用方法时引发异常 <xref:System.Windows.Forms.Application.FilterMessage%2A?displayProperty=nameWithType> 。 有关详细信息，请参阅[缓解：自定义 IMessageFilter.PreFilterMessage 实现](../../../migration-guide/mitigation-custom-imessagefilter-prefiltermessage-implementations.md)。|.NET Framework 4.6.1|
|`Switch.System.Windows.Forms.`<br/>`UseLegacyContextMenuStripSourceControlValue`|确定 <xref:System.Windows.Forms.ContextMenuStrip.SourceControl?displayProperty=nameWithType> 当用户通过嵌套控件打开菜单时，属性是否返回源代码管理 <xref:System.Windows.Forms.ToolStripMenuItem> 。 `true` 若要返回 `null` ，则为旧行为; `false` 返回源控件。|.NET Framework 4.7.2|
|`Switch.System.Windows.Forms.UseLegacyToolTipDisplay`|控制是否禁用工具提示调用支持 (`true`) 或启用 (`false`) 。 启用工具提示调用支持还需要由、和定义的旧版辅助功能， `Switch.UseLegacyAccessibilityFeatures` `Switch.UseLegacyAccessibilityFeatures.2` `Switch.UseLegacyAccessibilityFeatures.3` (设置为 `false`) 。|.NET Framework 4.8|
|`Switch.System.Windows.Input.Stylus.`<br/>`EnablePointerSupport`|确定是否 `WM_POINTER` 在 WPF 应用程序中启用了基于可选的触控/触笔堆栈。 有关详细信息，请参阅 [缓解：基于指针的触控和触笔支持](../../../migration-guide/mitigation-pointer-based-touch-and-stylus-support.md)|.NET Framework 4.7|
|`Switch.System.Windows.Markup.`<br/>`DoNotUseSha256ForMarkupCompilerChecksumAlgorithm`|确定用于校验和的默认哈希算法是 SHA256 (`false`) 还是 SHA1 (`true`) 。<br>由于与 SHA1 冲突，Microsoft 建议使用 SHA256。|.NET Framework 4.7.2|
|`Switch.System.Windows.Media.ImageSourceConverter.`<br/>`OverrideExceptionWithNullReferenceException`|控制是否引发旧的 [NullReferenceException](xref:System.NullReferenceException) 而不是引发异常，更具体地指出异常的原因 (例如 [DirectoryNotFoundException](xref:System.IO.DirectoryNotFoundException) 或 [system.io.filenotfoundexception](xref:System.IO.FileNotFoundException)。 它旨在供依赖于处理 [NullReferenceException](xref:System.NullReferenceException)的代码使用。 | .NET Framework 4.7 |
|`Switch.System.Workflow.ComponentModel.`<br/>`UseLegacyHashForXomlFileChecksum`|控制工作流项目生成中的 XOML 文件的校验和哈希运算是使用 MD5 算法 (`true`) ，还是使用在 .NET Framework 4.8 中引入为默认值的 SHA256 算法。<br>由于与 MD5 冲突，Microsoft 建议使用 SHA256。|.NET Framework 4.8|
|`Switch.System.Workflow.Runtime.`<br/>`UseLegacyHashForSqlTrackingCacheKey`|控制由 SqlTrackingService 哈希处理是否使用) 缓存字符串的 MD5 (算法 `true` ，或是否使用在 .NET Framework 4.8 中引入默认值的 SHA256 算法。<br>由于与 MD5 冲突，Microsoft 建议使用 SHA256。|.NET Framework 4.8|
|`Switch.System.Workflow.Runtime.`<br/>`UseLegacyHashForWorkflowDefinitionDispenserCacheKey`|控制工作流运行时是否使用 MD5 算法 `true` 为缓存的工作流定义 () ，或者是否使用在 .NET Framework 4.8 中引入默认值的 SHA256 算法。<br>由于与 MD5 冲突，Microsoft 建议使用 SHA256。|.NET Framework 4.8|
|`Switch.UseLegacyAccessibilityFeatures`|控制是否启用或禁用从 .NET Framework 4.7.1 开始可用的辅助功能。 | .NET Framework 4.7.1 |
|`Switch.UseLegacyAccessibilityFeatures.2`|控制是否启用 .NET Framework 4.7.2 中可用的辅助功能， (`false`)  (`true`) 。 如果 `true` 为，则 `Switch.UseLegacyAccessibilityFeatures` 必须同时 `true` 启用 .NET Framework 4.7.1 辅助功能。|.NET Framework 4.7.2|
|`Switch.UseLegacyAccessibilityFeatures.3`|控制是否 `false`) 或禁用 ()  (启用 .NET Framework 4.8 中引入的辅助功能 `true` 。 如果 `true` 、 `Switch.UseLegacyAccessibilityFeatures` 和 `Switch.UseLegacyAccessibilityFeatures.2` 必须也为 `true` 。|.NET Framework 4.8|
|`Switch.UseLegacyToolTipDisplay`|控制在用户将鼠标光标悬停在 () 的 WPF 控件上时是否显示工具提示 `true` ，或者是否在键盘焦点上以及通过键盘快捷键 (`false` 的默认行为) 显示工具提示。 对于在 .NET Framework 4.8 上运行但面向以前版本的 .NET Framework 的应用程序，启用键盘焦点和快捷键支持要求 `Switch.UseLegacyAccessibilityFeatures` 将、 `Switch.UseLegacyAccessibilityFeatures.2` 和 `Switch.UseLegacyAccessibilityFeatures.3` 均设置为 `false` 。|.NET Framework 4.8|
|`Switch.System.Xml.`<br />`IgnoreEmptyKeySequences`|控制 XSD 架构验证是否忽略复合键中的空键顺序。 有关详细信息，请参阅 [缓解： XML 架构验证](../../../migration-guide/mitigation-xml-schema-validation.md)。|.NET Framework 4.6|

> [!NOTE]
> `AppContextSwitchOverrides`你还可以通过在 c # 中调用 (，而不是将元素添加到应用程序配置文件 `static` 中 ) 或 `Shared` (Visual Basic) 方法中进行设置 <xref:System.AppContext.SetSwitch%2A?displayProperty=nameWithType> 。

 库开发人员还可以定义自定义开关，使调用方可以选择不更改其库的更高版本中引入的已更改功能。 有关更多信息，请参见 <xref:System.AppContext> 类。

## <a name="switches-in-aspnet-apps"></a>ASP.NET 应用中的开关

可以通过将 [\<Add>](../appsettings/add-element-for-appsettings.md) 元素添加到 web.config 文件的部分，将 ASP.NET 应用程序配置为使用兼容性设置 [\<appSettings>](../appsettings/index.md) 。

下面的示例使用 `<add>` 元素将两个设置添加到 `<appSettings>` web.config 文件的部分：

```xml
<appSettings>
  <add key="AppContext.SetSwitch:Switch.System.Globalization.NoAsyncCurrentCulture" value="true" />
  <add key="AppContext.SetSwitch:Switch.System.Uri.DontEnableStrictRFC3986ReservedCharacterSets" value="true" />
</appSettings>
```

## <a name="example"></a>示例

 下面的示例使用 `AppContextSwitchOverrides` 元素定义单个应用程序兼容性开关， `Switch.System.Globalization.NoAsyncCurrentCulture` 该开关阻止区域性在异步方法调用中的线程之间流动。

```xml
<configuration>
   <runtime>
      <AppContextSwitchOverrides value="Switch.System.Globalization.NoAsyncCurrentCulture=true" />
   </runtime>
</configuration>
```

 下面的示例使用 `AppContextSwitchOverrides` 元素定义两个应用程序兼容性开关， `Switch.System.Globalization.NoAsyncCurrentCulture` 和 `Switch.System.IO.BlockLongPaths` 。 分号将两个名称/值对隔开。

```xml
<configuration>
    <runtime>
       <AppContextSwitchOverrides
          value="Switch.System.Globalization.NoAsyncCurrentCulture=true;Switch.System.IO.BlockLongPaths=true" />
    </runtime>
</configuration>
```

## <a name="see-also"></a>请参阅

- [在 .NET Framework 4.6 和更高版本中缓解新行为](../../../migration-guide/mitigations.md)
- <xref:System.AppContext?displayProperty=nameWithType>
- [\<runtime> 元素](runtime-element.md)
- [\<configuration> 元素](../configuration-element.md)
