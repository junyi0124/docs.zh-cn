---
title: 将 Windows 应用商店应用迁移到 .NET Native
ms.date: 03/30/2017
ms.assetid: 4153aa18-6f56-4a0a-865b-d3da743a1d05
ms.openlocfilehash: ee17e50590a80d8973197b46910d5e22296c265f
ms.sourcegitcommit: 30a686fd4377fe6472aa04e215c0de711bc1c322
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2020
ms.locfileid: "94440889"
---
# <a name="migrate-your-windows-store-app-to-net-native"></a>将 Windows 应用商店应用迁移到 .NET Native

.NET Native 提供 Windows 应用商店中或开发人员计算机上的应用的静态编译。 这不同于及时生成 (JIT) 编译器或 [本地映像生成器 (Ngen.exe)](../tools/ngen-exe-native-image-generator.md) 在该设备上为 Windows 应用商店应用执行的动态编译。 尽管存在差异，但 .NET Native 会尝试保持与 [.net For Windows 应用商店应用](/previous-versions/windows/apps/br230302(v=vs.140))的兼容性。 大多数情况下，适用于 Windows 应用商店应用的 .NET 上的功能也适用于 .NET Native。  然而，在某些情况下，你可能会遇到行为变更。 本文档讨论了 Windows 应用商店应用的标准 .NET 与以下几个方面的 .NET Native 之间的差异：

- [常规运行时差异](#Runtime)

- [动态编程差异](#Dynamic)

- [其他与反射相关的差异](#Reflection)

- [不受支持的方案和 API](#Unsupported)

- [Visual Studio 差异](#VS)

<a name="Runtime"></a>

## <a name="general-runtime-differences"></a>常规运行时差异

- <xref:System.TypeLoadException>当应用程序在公共语言运行时 (CLR) 上运行时，由 JIT 编译器引发的异常（如）通常会导致 .NET Native 处理时出现编译时错误。

- 请勿从一个应用的 UI 线程调用 <xref:System.GC.WaitForPendingFinalizers%2A?displayProperty=nameWithType> 方法。 这可能会导致 .NET Native 死锁。

- 请勿依赖静态类构造函数调用排序。 在 .NET Native 中，调用顺序不同于标准运行时的顺序。 （即使可以使用标准运行时，你也不应依赖静态类构造函数的执行顺序。）

- 不在任何线程上执行调用（例如， `while(true);`）的无线循环可能会使应用异常终止。 同样，长期或无限等待也可能使应用异常终止。

- 在 .NET Native 中，某些泛型初始化循环不会引发异常。 例如，以下代码导致标准 CLR 上 <xref:System.TypeLoadException> 发生了一个异常。 在 .NET Native 中，它不会。

  [!code-csharp[ProjectN#8](../../../samples/snippets/csharp/VS_Snippets_CLR/projectn/cs/compat1.cs#8)]

- 在某些情况下，.NET Native 提供 .NET Framework 类库的不同实现。 从一个方法返回的对象总是实施返回类型的成员。 然而，由于它的后备实施是不同的，你可能无法像在其他 .NET Framework 平台上的操作一样将其转换到相同的类型集。 例如，在某些情况下，你可能无法将 <xref:System.Collections.Generic.IEnumerable%601> 或 <xref:System.Reflection.TypeInfo.DeclaredMembers%2A?displayProperty=nameWithType> 等方法返回的 <xref:System.Reflection.TypeInfo.DeclaredProperties%2A?displayProperty=nameWithType> 界面对象转换为 `T[]`。

- Windows 应用商店应用的 .NET 上的 WinInet 缓存默认情况下未启用，但在 .NET Native 上。 这提高了性能但会影响工作集。 开发者无需执行任何操作。

<a name="Dynamic"></a>

## <a name="dynamic-programming-differences"></a>动态编程差异

.NET Native 来自 .NET Framework 的代码中的静态链接，以使代码本地应用程序获得最大性能。 然而，由于二进制大小必须保持较小，这就使得整个 .NET Framework 无法进入。 .NET Native 编译器通过使用删除对未使用代码的引用的依赖项化简器来解决此限制。 但是，当不能在编译时静态推断信息，而是在运行时动态检索，.NET Native 可能无法维护或生成某些类型信息和代码。

.NET Native 启用反射和动态编程。 然而，并非所有类型都可以标记为反射，因为这会导致生成的代码太大（尤其因为 .NET Framework 中支持反射到公共 API）。 .NET Native 编译器可以明智地选择哪些类型应支持反射，并且它将保留元数据并仅为这些类型生成代码。

例如，绑定数据需要一个应用能够将属性名映射到函数。 在 Windows 应用商店应用的 .NET 中，公共语言运行时自动使用反射来向托管类型和公开可用的本机类型提供该能力。 在 .NET Native 中，编译器将自动包含要将数据绑定到的类型的元数据。

.NET Native 编译器还可以处理通常使用的泛型类型（如 <xref:System.Collections.Generic.List%601> 和 <xref:System.Collections.Generic.Dictionary%602> ），而无需任何提示或指令。 [动态](../../csharp/language-reference/builtin-types/reference-types.md#the-dynamic-type) 关键字在某些限制内也受到支持。

> [!NOTE]
> 在将应用程序移植到 .NET Native 时，应彻底测试所有动态代码路径。

.NET Native 的默认配置对于大多数开发人员来说已经足够，但某些开发人员可能希望通过使用运行时指令 ( # A0) 文件来微调其配置。 此外，在某些情况下，.NET Native 编译器无法确定哪些元数据必须可用于反射，并依赖于提示，尤其是在以下情况下：

- <xref:System.Type.MakeGenericType%2A?displayProperty=nameWithType> 和 <xref:System.Reflection.MethodInfo.MakeGenericMethod%2A?displayProperty=nameWithType> 等一些构造无法静态确定。

- 因为编译器无法确定实例化，因此你想要反射到的泛型类型必须通过运行时指令来指定。 这并不仅是因为所有的代码必须包含在内，还因为在泛型类型上的反射会形成一个无限循环（例如，当某泛型方法在某泛型类型上调用时）。

> [!NOTE]
> 运行时指令是在运行时指令 (.rd.xml) 文件中定义的。 有关使用此文件的常规信息，请参阅[入门](getting-started-with-net-native.md)。 有关运行时指令的信息，请参阅 [Runtime Directives (rd.xml) Configuration File Reference](runtime-directives-rd-xml-configuration-file-reference.md)。

.NET Native 还包含分析工具，可帮助开发人员确定默认集外的哪些类型应支持反射。

<a name="Reflection"></a>

## <a name="other-reflection-related-differences"></a>其他与反射相关的差异

对于 Windows 应用商店应用和 .NET Native，.NET 中的行为之间存在许多其他与反射相关的不同之处。

在 .NET Native 中：

- 反射到 .NET Framework 类库中的类型和成员的私有反射不受支持。 然而，你可以反射到自己的私有类型和成员以及第三方库的类型和成员。

- <xref:System.Reflection.ParameterInfo.HasDefaultValue%2A?displayProperty=nameWithType> 属性为表示返回值的 `false` 对象正确返回 <xref:System.Reflection.ParameterInfo> 。 在 Windows 应用商店应用的 .NET 中，它返回 `true`。 中间语言 (IL) 不能直接支持此，而是将解释留给语言。

- 位于 <xref:System.RuntimeFieldHandle> 和 <xref:System.RuntimeMethodHandle> 结构上的公共成员不受支持。 这些受到支持的类型仅用于 LINQ、表达式树和静态阵列初始化。

- <xref:System.Reflection.RuntimeReflectionExtensions.GetRuntimeProperties%2A?displayProperty=nameWithType> 和 <xref:System.Reflection.RuntimeReflectionExtensions.GetRuntimeEvents%2A?displayProperty=nameWithType> 在基类中包含隐藏成员，因此可能会在没有显示重写的情况下遭到重写。 这也适用于其他 [RuntimeReflectionExtensions.GetRuntime*](xref:System.Reflection.RuntimeReflectionExtensions) 方法。

- <xref:System.Type.MakeArrayType%2A?displayProperty=nameWithType><xref:System.Type.MakeByRefType%2A?displayProperty=nameWithType>如果尝试创建某些组合 (例如，) 对象的数组，则不会失败 `byref` 。

- 你无法使用反射来调用具有指针参数的成员。

- 你无法使用反射来获取或设置一个指针字段。

- 当参数计数错误并且其中一个参数的类型不正确时，.NET Native 引发 <xref:System.ArgumentException> 而不是 <xref:System.Reflection.TargetParameterCountException> 。

- 异常的二进制序列化通常不受支持。 因此，不可序列化的对象可添加到 <xref:System.Exception.Data%2A?displayProperty=nameWithType> 字典。

<a name="Unsupported"></a>

## <a name="unsupported-scenarios-and-apis"></a>不受支持的方案和 API

以下部分列出了不受一般开发支持的方案和 API、互操作以及 HTTPClient 和 Windows Communication Foundation (WCF) 等技术：

- [常规开发](#General)

- [HttpClient](#HttpClient)

- [Interop](#Interop)

- [不受支持的 API](#APIs)

<a name="General"></a>

### <a name="general-development-differences"></a>常规开发差异

**值类型**

- 如果你替代了一个值类型的 <xref:System.ValueType.Equals%2A?displayProperty=nameWithType> 和 <xref:System.ValueType.GetHashCode%2A?displayProperty=nameWithType> 方法，请勿调用基类实施。 在 Windows 应用商店应用的 .NET 中，这些方法依赖反射。 在编译时，.NET Native 生成一个不依赖运行时反射的实现。 这意味着，如果不重写这两种方法，则它们将按预期方式工作，因为 .NET Native 会在编译时生成实现。 然而，替代这些方法并调用基类实施会产生一个异常。

- 不支持大于 1 mb 的值类型。

- 在 .NET Native 中，值类型不能有无参数的构造函数。  (c # 和 Visual Basic 在值类型上禁止无参数的构造函数。 然而，这些可以在 IL 中进行创建。）

**数组**

- 界限低于零的阵列不受支持。 通常，这些阵列是通过调用 <xref:System.Array.CreateInstance%28System.Type%2CSystem.Int32%5B%5D%2CSystem.Int32%5B%5D%29?displayProperty=nameWithType> 重载来创建的。

- 不支持动态创建多维阵列。 此类阵列一般是通过调用包括 <xref:System.Array.CreateInstance%2A?displayProperty=nameWithType> 参数的 `lengths` 方法重载来创建，或通过调用 <xref:System.Type.MakeArrayType%28System.Int32%29?displayProperty=nameWithType> 方法来创建。

- 四维或更多维的多维阵列不受支持；因为它们的 <xref:System.Array.Rank%2A?displayProperty=nameWithType> 属性值是四或者更大。 可使用 [交错阵列](../../csharp/programming-guide/arrays/jagged-arrays.md) （阵列的阵列）。 例如， `array[x,y,z]` 无效，但 `array[x][y][z]` 有效。

- 多维阵列的差异不受支持且会在运行时导致 <xref:System.InvalidCastException> 异常。

**泛型**

- 无限泛型类型扩展会导致编译器错误。 例如，该代码无法编译：

  [!code-csharp[ProjectN#9](../../../samples/snippets/csharp/VS_Snippets_CLR/projectn/cs/compat2.cs#9)]

**指针**

- 指针阵列不受支持。

- 你无法使用反射来获取或设置一个指针字段。

**序列化**

<xref:System.Runtime.Serialization.KnownTypeAttribute.%23ctor%28System.String%29> 特性不受支持。 使用 <xref:System.Runtime.Serialization.KnownTypeAttribute.%23ctor%28System.Type%29> 特性。

**资源**

不支持使用带 <xref:System.Diagnostics.Tracing.EventSource> 类的本地化资源。 <xref:System.Diagnostics.Tracing.EventSourceAttribute.LocalizationResources%2A?displayProperty=nameWithType> 属性无法定义本地化资源。

**委托**

`Delegate.BeginInvoke` 和 `Delegate.EndInvoke` 不受支持。

**其他 API**

- [TypeInfo.GUID](xref:System.Type.GUID) <xref:System.PlatformNotSupportedException> 如果 <xref:System.Runtime.InteropServices.GuidAttribute> 特性未应用于该类型，则 TypeInfo 属性将引发异常。 GUID 主要用于 COM 支持。

- 此 <xref:System.DateTime.Parse%2A?displayProperty=nameWithType> 方法会正确分析在 .NET Native 中包含短日期的字符串。 然而，它不会继续兼容 Microsoft 知识库文章 [KB2803771](https://support.microsoft.com/kb/2803771) 和 [KB2803755](https://support.microsoft.com/kb/2803755)中描述的日期和时间解析的变更。

- <xref:System.Numerics.BigInteger.ToString%2A?displayProperty=nameWithType>`("E")`在 .NET Native 中正确舍入。 在某些版本的 CLR 中，结果字符串会缩短，而不是舍入。

<a name="HttpClient"></a>

### <a name="httpclient-differences"></a>HttpClient 差异

在 .NET Native 中， <xref:System.Net.Http.HttpClientHandler> 类在内部使用 WinINet)  (， <xref:Windows.Web.Http.Filters.HttpBaseProtocolFilter> 而不是使用 <xref:System.Net.WebRequest> <xref:System.Net.WebResponse> 标准 .Net for Windows 应用商店应用中使用的和类。  WinINet 并不支持 <xref:System.Net.Http.HttpClientHandler> 类支持的所有配置选项。  因此：

- 的某些功能属性在 <xref:System.Net.Http.HttpClientHandler> .NET Native 上返回 `false` ，而它们 `true` 在 Windows 应用商店应用的标准 .net 中返回。

- 某些配置属性 `get` 访问器始终在 .NET Native 上返回固定值，而此值不同于 Windows 应用商店应用的 .net 中的默认可配置值。

某些额外的行为差异涵盖在以下子节中。

**Proxy**

<xref:Windows.Web.Http.Filters.HttpBaseProtocolFilter>类不支持基于每个请求配置或重写代理。  这意味着，.NET Native 上的所有请求都使用系统配置的代理服务器或没有代理服务器，具体取决于属性的值 <xref:System.Net.Http.HttpClientHandler.UseProxy%2A?displayProperty=nameWithType> 。  在 Windows 应用商店应用的 .NET 中，代理服务器由 <xref:System.Net.Http.HttpClientHandler.Proxy%2A?displayProperty=nameWithType> 属性定义。  在 .NET Native 上，将设置 <xref:System.Net.Http.HttpClientHandler.Proxy%2A?displayProperty=nameWithType> 为之外的值会 `null` 引发 <xref:System.PlatformNotSupportedException> 异常。  <xref:System.Net.Http.HttpClientHandler.SupportsProxy%2A?displayProperty=nameWithType>属性在 `false` .NET Native 上返回，而在 `true` Windows 应用商店应用的标准 .NET Framework 中返回。

**自动重定向**

<xref:Windows.Web.Http.Filters.HttpBaseProtocolFilter>类不允许配置最大数目的自动重定向。  Windows 应用商店应用的标准 .NET 中 <xref:System.Net.Http.HttpClientHandler.MaxAutomaticRedirections%2A?displayProperty=nameWithType> 属性的值默认为 50，且可修改。 在 .NET Native 上，此属性的值为10，尝试对其进行修改会引发 <xref:System.PlatformNotSupportedException> 异常。  <xref:System.Net.Http.HttpClientHandler.SupportsRedirectConfiguration%2A?displayProperty=nameWithType>属性在 `false` .NET Native 上返回，而 `true` 在 Windows 应用商店应用的 .net 中返回。

**自动解压缩**

Windows 应用商店应用的 .NET 支持将 <xref:System.Net.Http.HttpClientHandler.AutomaticDecompression%2A?displayProperty=nameWithType> 属性设置为 <xref:System.Net.DecompressionMethods.Deflate>、 <xref:System.Net.DecompressionMethods.GZip>（ <xref:System.Net.DecompressionMethods.Deflate> 和 <xref:System.Net.DecompressionMethods.GZip>都可以）或 <xref:System.Net.DecompressionMethods.None>。  .NET Native 仅支持和 <xref:System.Net.DecompressionMethods.Deflate> <xref:System.Net.DecompressionMethods.GZip> 、或 <xref:System.Net.DecompressionMethods.None> 。  尝试仅将 <xref:System.Net.Http.HttpClientHandler.AutomaticDecompression%2A> 属性设置为 <xref:System.Net.DecompressionMethods.Deflate> 或 <xref:System.Net.DecompressionMethods.GZip> 会自动将其设置为 <xref:System.Net.DecompressionMethods.Deflate> 和 <xref:System.Net.DecompressionMethods.GZip>。

**Cookie**

Cookie 处理由 <xref:System.Net.Http.HttpClient> 和 WinINet 同时执行。  来自 <xref:System.Net.CookieContainer> 的 Cookies 与 WinINet cookie 缓存结合。  从 <xref:System.Net.CookieContainer> 删除 cookie 可防止 <xref:System.Net.Http.HttpClient> 发送 cookie，但如果 cookie 已被 WinINet 检测到且 cookies 未被用户删除，则 WinINet 将会发送 cookie。  无法通过编程的方式使用 <xref:System.Net.Http.HttpClient>、 <xref:System.Net.Http.HttpClientHandler>或 <xref:System.Net.CookieContainer> API 从 WinINet 中删除 cookie。  将 <xref:System.Net.Http.HttpClientHandler.UseCookies%2A?displayProperty=nameWithType> 属性设置为 `false` 只会导致 <xref:System.Net.Http.HttpClient> 停止发送 cookies；WinINet 仍可能会在请求中包括 cookies。

**凭据**

在 Windows 应用商店应用的 .NET 中， <xref:System.Net.Http.HttpClientHandler.UseDefaultCredentials%2A?displayProperty=nameWithType> 和 <xref:System.Net.Http.HttpClientHandler.Credentials%2A?displayProperty=nameWithType> 属性独立工作。  此外， <xref:System.Net.Http.HttpClientHandler.Credentials%2A> 属性接受实施 <xref:System.Net.ICredentials> 接口的任意对象。  在 .NET Native 中，将 <xref:System.Net.Http.HttpClientHandler.UseDefaultCredentials%2A> 属性设置为 `true` 会导致 <xref:System.Net.Http.HttpClientHandler.Credentials%2A> 属性变为 `null` 。  此外， <xref:System.Net.Http.HttpClientHandler.Credentials%2A> 属性只能设置为 `null`、 <xref:System.Net.CredentialCache.DefaultCredentials%2A>或类型 <xref:System.Net.NetworkCredential>的对象。  将其他任意 <xref:System.Net.ICredentials> 对象（其中最常见的是 <xref:System.Net.CredentialCache>）分配至 <xref:System.Net.Http.HttpClientHandler.Credentials%2A> 属性都会导致 <xref:System.PlatformNotSupportedException>。

**其他不受支持或不可配置的功能**

在 .NET Native 中：

- <xref:System.Net.Http.HttpClientHandler.ClientCertificateOptions%2A?displayProperty=nameWithType> 属性的值始终为 <xref:System.Net.Http.ClientCertificateOption.Automatic>。  在 Windows 应用商店应用的 .NET 中，默认为 <xref:System.Net.Http.ClientCertificateOption.Manual>。

- <xref:System.Net.Http.HttpClientHandler.MaxRequestContentBufferSize%2A?displayProperty=nameWithType> 属性无法配置。

- <xref:System.Net.Http.HttpClientHandler.PreAuthenticate%2A?displayProperty=nameWithType> 属性始终为 `true`。  在 Windows 应用商店应用的 .NET 中，默认为 `false`。

- 响应中的 `SetCookie2` 标题忽略为废弃。

<a name="Interop"></a>

### <a name="interop-differences"></a>互操作差异性

 **弃用的 API**

 多个带有托管代码的不常使用的互操作性 API 已弃用。 与 .NET Native 一起使用时，这些 Api 可能会引发 <xref:System.NotImplementedException> 或 <xref:System.PlatformNotSupportedException> 异常，或导致编译器错误。 在 Windows 应用商店应用的 .NET 中，这些 API 标记为废弃，尽管调用这些 API 会生成编译器警告而非编译器错误。

 封送处理的不推荐使用的 Api `VARIANT` 包括：

- <xref:System.Runtime.InteropServices.BStrWrapper?displayProperty=nameWithType>
- <xref:System.Runtime.InteropServices.CurrencyWrapper?displayProperty=nameWithType>
- <xref:System.Runtime.InteropServices.DispatchWrapper?displayProperty=nameWithType>
- <xref:System.Runtime.InteropServices.ErrorWrapper?displayProperty=nameWithType>
- <xref:System.Runtime.InteropServices.UnknownWrapper?displayProperty=nameWithType>
- <xref:System.Runtime.InteropServices.VariantWrapper?displayProperty=nameWithType>
- <xref:System.Runtime.InteropServices.UnmanagedType.IDispatch?displayProperty=nameWithType>
- <xref:System.Runtime.InteropServices.UnmanagedType.SafeArray?displayProperty=nameWithType>
- <xref:System.Runtime.InteropServices.VarEnum?displayProperty=nameWithType>

 <xref:System.Runtime.InteropServices.UnmanagedType.Struct?displayProperty=nameWithType> 支持，但在某些情况下会引发异常，如与 [IDispatch](/previous-versions/windows/desktop/api/oaidl/nn-oaidl-idispatch) 或 variant 一起使用时 `byref` 。

 适用于 [IDispatch](/previous-versions/windows/desktop/api/oaidl/nn-oaidl-idispatch) 支持的不推荐使用的 api 包括：

- <xref:System.Runtime.InteropServices.ClassInterfaceType.AutoDispatch?displayProperty=fullName>
- <xref:System.Runtime.InteropServices.ClassInterfaceType.AutoDual?displayProperty=fullName>
- <xref:System.Runtime.InteropServices.ComDefaultInterfaceAttribute?displayProperty=nameWithType>

适用于经典 COM 事件的不推荐使用的 Api 包括：

- <xref:System.Runtime.InteropServices.ComEventsHelper?displayProperty=nameWithType>
- <xref:System.Runtime.InteropServices.ComSourceInterfacesAttribute>

接口中弃用的 Api <xref:System.Runtime.InteropServices.ICustomQueryInterface?displayProperty=nameWithType> ，在 .NET Native 中不受支持，包括：

- <xref:System.Runtime.InteropServices.ICustomQueryInterface?displayProperty=nameWithType>（所有成员）
- <xref:System.Runtime.InteropServices.CustomQueryInterfaceMode?displayProperty=nameWithType>（所有成员）
- <xref:System.Runtime.InteropServices.CustomQueryInterfaceResult?displayProperty=nameWithType>（所有成员）
- <xref:System.Runtime.InteropServices.Marshal.GetComInterfaceForObject%28System.Object%2CSystem.Type%2CSystem.Runtime.InteropServices.CustomQueryInterfaceMode%29?displayProperty=fullName>

其他不受支持的互操作功能包括：

- <xref:System.Runtime.InteropServices.ICustomAdapter?displayProperty=nameWithType>（所有成员）
- <xref:System.Runtime.InteropServices.SafeBuffer?displayProperty=nameWithType>（所有成员）
- <xref:System.Runtime.InteropServices.UnmanagedType.Currency?displayProperty=fullName>
- <xref:System.Runtime.InteropServices.UnmanagedType.VBByRefStr?displayProperty=fullName>
- <xref:System.Runtime.InteropServices.UnmanagedType.AnsiBStr?displayProperty=fullName>
- <xref:System.Runtime.InteropServices.UnmanagedType.AsAny?displayProperty=fullName>
- <xref:System.Runtime.InteropServices.UnmanagedType.CustomMarshaler?displayProperty=fullName>

 很少使用的封送 Api：

- <xref:System.Runtime.InteropServices.Marshal.ReadByte%28System.Object%2CSystem.Int32%29?displayProperty=fullName>
- <xref:System.Runtime.InteropServices.Marshal.ReadInt16%28System.Object%2CSystem.Int32%29?displayProperty=fullName>
- <xref:System.Runtime.InteropServices.Marshal.ReadInt32%28System.Object%2CSystem.Int32%29?displayProperty=fullName>
- <xref:System.Runtime.InteropServices.Marshal.ReadInt64%28System.Object%2CSystem.Int32%29?displayProperty=fullName>
- <xref:System.Runtime.InteropServices.Marshal.ReadIntPtr%28System.Object%2CSystem.Int32%29?displayProperty=fullName>
- <xref:System.Runtime.InteropServices.Marshal.WriteByte%28System.Object%2CSystem.Int32%2CSystem.Byte%29?displayProperty=fullName>
- <xref:System.Runtime.InteropServices.Marshal.WriteInt16%28System.Object%2CSystem.Int32%2CSystem.Int16%29?displayProperty=fullName>
- <xref:System.Runtime.InteropServices.Marshal.WriteInt32%28System.Object%2CSystem.Int32%2CSystem.Int32%29?displayProperty=fullName>
- <xref:System.Runtime.InteropServices.Marshal.WriteInt64%28System.Object%2CSystem.Int32%2CSystem.Int64%29?displayProperty=fullName>
- <xref:System.Runtime.InteropServices.Marshal.WriteIntPtr%28System.Object%2CSystem.Int32%2CSystem.IntPtr%29?displayProperty=fullName>

 **平台调用和 COM 互操作兼容性**

 大多数平台调用和 COM 互操作方案在 .NET Native 中仍受支持。 特别地，Windows Runtime (WinRT) API 的所有互操作和 Windows Runtime 所需的所有封送都受支持。 这包括针对以下内容的封送支持：

- 阵列（包括 <xref:System.Runtime.InteropServices.UnmanagedType.ByValArray?displayProperty=nameWithType>）

- `BStr`

- 委托

- String (Unicode、ANSI 和 HSTRING) 

- 结构（`byref` 和 `byval`）

- Unions

- Win32 句柄

- 所有 WinRT 构造

- 部分支持封送变体类型。 支持以下各项：

  - <xref:System.Boolean>

  - <xref:System.Byte>

  - <xref:System.Decimal>

  - <xref:System.Double>

  - <xref:System.Int16>

  - <xref:System.Int32>

  - <xref:System.Int64>

  - <xref:System.SByte>

  - <xref:System.Single>

  - <xref:System.UInt16>

  - <xref:System.UInt32>

  - <xref:System.UInt64>

  - `BStr`

  - [IUnknown](/windows/desktop/api/unknwn/nn-unknwn-iunknown)

但 .NET Native 不支持以下操作：

- 使用经典 COM 事件

- 在托管类型上实施 <xref:System.Runtime.InteropServices.ICustomQueryInterface?displayProperty=nameWithType> 接口

- 通过 [属性，在托管类型上实现](/previous-versions/windows/desktop/api/oaidl/nn-oaidl-idispatch) IDispatch <xref:System.Runtime.InteropServices.ComDefaultInterfaceAttribute?displayProperty=nameWithType> 接口。 但是，你无法通过调用 COM 对象 `IDispatch` ，而且你的托管对象无法实现 `IDispatch` 。

使用反射来调用平台调用方法不受支持。 你可以巧妙绕过这种限制，具体做法是将方法调用包装在另一种方法中，并使用反射调用包装方法。

<a name="APIs"></a>

### <a name="other-differences-from-net-apis-for-windows-store-apps"></a>与 Windows 应用商店应用 .NET API 的其他差异

本部分列出了 .NET Native 不支持的其余 Api。 不受支持的最大 Api 集是 Windows Communication Foundation (WCF) Api。

**DataAnnotations (System.ComponentModel.DataAnnotations)**

<xref:System.ComponentModel.DataAnnotations> <xref:System.ComponentModel.DataAnnotations.Schema> .NET Native 不支持和命名空间中的类型。 其中包括适用于 Windows 8 的 Windows 应用商店应用的 .NET 中出现的以下类型：

- <xref:System.ComponentModel.DataAnnotations.AssociationAttribute?displayProperty=nameWithType>
- <xref:System.ComponentModel.DataAnnotations.ConcurrencyCheckAttribute?displayProperty=nameWithType>
- <xref:System.ComponentModel.DataAnnotations.CustomValidationAttribute?displayProperty=nameWithType>
- <xref:System.ComponentModel.DataAnnotations.DataType?displayProperty=nameWithType>
- <xref:System.ComponentModel.DataAnnotations.DataTypeAttribute?displayProperty=nameWithType>
- <xref:System.ComponentModel.DataAnnotations.DisplayAttribute?displayProperty=nameWithType>
- <xref:System.ComponentModel.DataAnnotations.DisplayColumnAttribute?displayProperty=nameWithType>
- <xref:System.ComponentModel.DataAnnotations.DisplayFormatAttribute>
- <xref:System.ComponentModel.DataAnnotations.EditableAttribute>
- <xref:System.ComponentModel.DataAnnotations.EnumDataTypeAttribute>
- <xref:System.ComponentModel.DataAnnotations.FilterUIHintAttribute?displayProperty=nameWithType>
- <xref:System.ComponentModel.DataAnnotations.KeyAttribute?displayProperty=nameWithType>
- <xref:System.ComponentModel.DataAnnotations.RangeAttribute?displayProperty=nameWithType>
- <xref:System.ComponentModel.DataAnnotations.RegularExpressionAttribute?displayProperty=nameWithType>
- <xref:System.ComponentModel.DataAnnotations.RequiredAttribute?displayProperty=nameWithType>
- <xref:System.ComponentModel.DataAnnotations.StringLengthAttribute?displayProperty=nameWithType>
- <xref:System.ComponentModel.DataAnnotations.TimestampAttribute?displayProperty=nameWithType>
- <xref:System.ComponentModel.DataAnnotations.UIHintAttribute?displayProperty=nameWithType>
- <xref:System.ComponentModel.DataAnnotations.ValidationAttribute?displayProperty=nameWithType>
- <xref:System.ComponentModel.DataAnnotations.ValidationContext?displayProperty=nameWithType>
- <xref:System.ComponentModel.DataAnnotations.ValidationException?displayProperty=nameWithType>
- <xref:System.ComponentModel.DataAnnotations.ValidationResult?displayProperty=nameWithType>
- <xref:System.ComponentModel.DataAnnotations.Validator?displayProperty=nameWithType>
- <xref:System.ComponentModel.DataAnnotations.Schema.DatabaseGeneratedAttribute?displayProperty=nameWithType>
- <xref:System.ComponentModel.DataAnnotations.Schema.DatabaseGeneratedOption?displayProperty=nameWithType>

 **Visual Basic**

.NET Native 当前不支持 Visual Basic。 和命名空间中的以下类型在 <xref:Microsoft.VisualBasic> <xref:Microsoft.VisualBasic.CompilerServices> .NET Native 中不可用：

- <xref:Microsoft.VisualBasic.CallType?displayProperty=nameWithType>
- <xref:Microsoft.VisualBasic.Constants?displayProperty=nameWithType>
- <xref:Microsoft.VisualBasic.HideModuleNameAttribute?displayProperty=nameWithType>
- <xref:Microsoft.VisualBasic.Strings?displayProperty=nameWithType>
- <xref:Microsoft.VisualBasic.CompilerServices.Conversions?displayProperty=nameWithType>
- <xref:Microsoft.VisualBasic.CompilerServices.DesignerGeneratedAttribute?displayProperty=nameWithType>
- <xref:Microsoft.VisualBasic.CompilerServices.IncompleteInitialization?displayProperty=nameWithType>
- <xref:Microsoft.VisualBasic.CompilerServices.NewLateBinding?displayProperty=nameWithType>
- <xref:Microsoft.VisualBasic.CompilerServices.ObjectFlowControl?displayProperty=nameWithType>
- <xref:Microsoft.VisualBasic.CompilerServices.ObjectFlowControl.ForLoopControl?displayProperty=nameWithType>
- <xref:Microsoft.VisualBasic.CompilerServices.Operators?displayProperty=nameWithType>
- <xref:Microsoft.VisualBasic.CompilerServices.OptionCompareAttribute?displayProperty=nameWithType>
- <xref:Microsoft.VisualBasic.CompilerServices.OptionTextAttribute?displayProperty=nameWithType>
- <xref:Microsoft.VisualBasic.CompilerServices.ProjectData?displayProperty=nameWithType>
- <xref:Microsoft.VisualBasic.CompilerServices.StandardModuleAttribute?displayProperty=nameWithType>
- <xref:Microsoft.VisualBasic.CompilerServices.StaticLocalInitFlag?displayProperty=nameWithType>
- <xref:Microsoft.VisualBasic.CompilerServices.Utils?displayProperty=nameWithType>

**反射上下文（System.Reflection.Context 命名空间）**

<xref:System.Reflection.Context.CustomReflectionContext?displayProperty=nameWithType>.NET Native 中不支持类。

**RTC (System.Net.Http.Rtc)**

`System.Net.Http.RtcRequestFactory`.NET Native 中不支持类。

**Windows Communication Foundation (WCF) (System.ServiceModel.\*)**

.NET Native 中不支持 [system.servicemodel. * 命名空间](xref:System.ServiceModel) 中的类型。 这包括以下类型：

- <xref:System.ServiceModel.ActionNotSupportedException?displayProperty=nameWithType>
- <xref:System.ServiceModel.BasicHttpBinding?displayProperty=nameWithType>
- <xref:System.ServiceModel.BasicHttpMessageCredentialType?displayProperty=nameWithType>
- <xref:System.ServiceModel.BasicHttpSecurity?displayProperty=nameWithType>
- <xref:System.ServiceModel.BasicHttpSecurityMode?displayProperty=nameWithType>
- <xref:System.ServiceModel.CallbackBehaviorAttribute?displayProperty=nameWithType>
- <xref:System.ServiceModel.ChannelFactory?displayProperty=nameWithType>
- <xref:System.ServiceModel.ChannelFactory%601?displayProperty=nameWithType>
- <xref:System.ServiceModel.ClientBase%601?displayProperty=nameWithType>
- <xref:System.ServiceModel.ClientBase%601.BeginOperationDelegate?displayProperty=nameWithType>
- <xref:System.ServiceModel.ClientBase%601.ChannelBase%601?displayProperty=nameWithType>
- <xref:System.ServiceModel.ClientBase%601.EndOperationDelegate?displayProperty=nameWithType>
- <xref:System.ServiceModel.ClientBase%601.InvokeAsyncCompletedEventArgs?displayProperty=nameWithType>
- <xref:System.ServiceModel.CommunicationException?displayProperty=nameWithType>
- <xref:System.ServiceModel.CommunicationObjectAbortedException?displayProperty=nameWithType>
- <xref:System.ServiceModel.CommunicationObjectFaultedException?displayProperty=nameWithType>
- <xref:System.ServiceModel.CommunicationState?displayProperty=nameWithType>
- <xref:System.ServiceModel.DataContractFormatAttribute?displayProperty=nameWithType>
- <xref:System.ServiceModel.DnsEndpointIdentity?displayProperty=nameWithType>
- <xref:System.ServiceModel.DuplexChannelFactory%601?displayProperty=nameWithType>
- <xref:System.ServiceModel.DuplexClientBase%601?displayProperty=nameWithType>
- <xref:System.ServiceModel.EndpointAddress?displayProperty=nameWithType>
- <xref:System.ServiceModel.EndpointAddressBuilder?displayProperty=nameWithType>
- <xref:System.ServiceModel.EndpointIdentity?displayProperty=nameWithType>
- <xref:System.ServiceModel.EndpointNotFoundException?displayProperty=nameWithType>
- <xref:System.ServiceModel.EnvelopeVersion?displayProperty=nameWithType>
- <xref:System.ServiceModel.ExceptionDetail?displayProperty=nameWithType>
- <xref:System.ServiceModel.FaultCode?displayProperty=nameWithType>
- <xref:System.ServiceModel.FaultContractAttribute?displayProperty=nameWithType>
- <xref:System.ServiceModel.FaultException?displayProperty=nameWithType>
- <xref:System.ServiceModel.FaultException%601?displayProperty=nameWithType>
- <xref:System.ServiceModel.FaultReason?displayProperty=nameWithType>
- <xref:System.ServiceModel.FaultReasonText?displayProperty=nameWithType>
- <xref:System.ServiceModel.HttpBindingBase?displayProperty=nameWithType>
- <xref:System.ServiceModel.HttpClientCredentialType?displayProperty=nameWithType>
- <xref:System.ServiceModel.HttpTransportSecurity?displayProperty=nameWithType>
- <xref:System.ServiceModel.IClientChannel?displayProperty=nameWithType>
- <xref:System.ServiceModel.ICommunicationObject?displayProperty=nameWithType>
- <xref:System.ServiceModel.IContextChannel?displayProperty=nameWithType>
- <xref:System.ServiceModel.IDefaultCommunicationTimeouts?displayProperty=nameWithType>
- <xref:System.ServiceModel.IExtensibleObject%601?displayProperty=nameWithType>
- <xref:System.ServiceModel.IExtension%601?displayProperty=nameWithType>
- <xref:System.ServiceModel.IExtensionCollection%601?displayProperty=nameWithType>
- <xref:System.ServiceModel.InstanceContext?displayProperty=nameWithType>
- <xref:System.ServiceModel.InvalidMessageContractException?displayProperty=nameWithType>
- <xref:System.ServiceModel.MessageBodyMemberAttribute?displayProperty=nameWithType>
- <xref:System.ServiceModel.MessageContractAttribute?displayProperty=nameWithType>
- <xref:System.ServiceModel.MessageContractMemberAttribute?displayProperty=nameWithType>
- <xref:System.ServiceModel.MessageCredentialType?displayProperty=nameWithType>
- <xref:System.ServiceModel.MessageHeader%601?displayProperty=nameWithType>
- <xref:System.ServiceModel.MessageHeaderException?displayProperty=nameWithType>
- <xref:System.ServiceModel.MessageParameterAttribute?displayProperty=nameWithType>
- <xref:System.ServiceModel.MessageSecurityOverTcp?displayProperty=nameWithType>
- <xref:System.ServiceModel.MessageSecurityVersion?displayProperty=nameWithType>
- <xref:System.ServiceModel.NetHttpBinding?displayProperty=nameWithType>
- <xref:System.ServiceModel.NetHttpMessageEncoding?displayProperty=nameWithType>
- <xref:System.ServiceModel.NetTcpBinding?displayProperty=nameWithType>
- <xref:System.ServiceModel.NetTcpSecurity?displayProperty=nameWithType>
- <xref:System.ServiceModel.OperationContext?displayProperty=nameWithType>
- <xref:System.ServiceModel.OperationContextScope?displayProperty=nameWithType>
- <xref:System.ServiceModel.OperationContractAttribute?displayProperty=nameWithType>
- <xref:System.ServiceModel.OperationFormatStyle?displayProperty=nameWithType>
- <xref:System.ServiceModel.ProtocolException?displayProperty=nameWithType>
- <xref:System.ServiceModel.QuotaExceededException?displayProperty=nameWithType>
- <xref:System.ServiceModel.SecurityMode?displayProperty=nameWithType>
- <xref:System.ServiceModel.ServerTooBusyException?displayProperty=nameWithType>
- <xref:System.ServiceModel.ServiceActivationException?displayProperty=nameWithType>
- <xref:System.ServiceModel.ServiceContractAttribute?displayProperty=nameWithType>
- <xref:System.ServiceModel.ServiceKnownTypeAttribute?displayProperty=nameWithType>
- <xref:System.ServiceModel.SpnEndpointIdentity?displayProperty=nameWithType>
- <xref:System.ServiceModel.TcpClientCredentialType?displayProperty=nameWithType>
- <xref:System.ServiceModel.TcpTransportSecurity?displayProperty=nameWithType>
- <xref:System.ServiceModel.TransferMode?displayProperty=nameWithType>
- <xref:System.ServiceModel.UnknownMessageReceivedEventArgs?displayProperty=nameWithType>
- <xref:System.ServiceModel.UpnEndpointIdentity?displayProperty=nameWithType>
- <xref:System.ServiceModel.XmlSerializerFormatAttribute?displayProperty=nameWithType>
- <xref:System.ServiceModel.Channels.AddressHeader?displayProperty=nameWithType>
- <xref:System.ServiceModel.Channels.AddressHeaderCollection?displayProperty=nameWithType>
- <xref:System.ServiceModel.Channels.AddressingVersion?displayProperty=nameWithType>
- <xref:System.ServiceModel.Channels.BinaryMessageEncodingBindingElement?displayProperty=nameWithType>
- <xref:System.ServiceModel.Channels.ChannelManagerBase?displayProperty=nameWithType>
- <xref:System.ServiceModel.Channels.ChannelParameterCollection?displayProperty=nameWithType>
- <xref:System.ServiceModel.Channels.CommunicationObject?displayProperty=nameWithType>
- <xref:System.ServiceModel.Channels.CompressionFormat?displayProperty=nameWithType>
- <xref:System.ServiceModel.Channels.ConnectionOrientedTransportBindingElement?displayProperty=nameWithType>
- <xref:System.ServiceModel.Channels.CustomBinding?displayProperty=nameWithType>
- <xref:System.ServiceModel.Channels.FaultConverter?displayProperty=nameWithType>
- <xref:System.ServiceModel.Channels.HttpRequestMessageProperty?displayProperty=nameWithType>
- <xref:System.ServiceModel.Channels.HttpResponseMessageProperty?displayProperty=nameWithType>
- <xref:System.ServiceModel.Channels.HttpsTransportBindingElement?displayProperty=nameWithType>
- <xref:System.ServiceModel.Channels.HttpTransportBindingElement?displayProperty=nameWithType>
- <xref:System.ServiceModel.Channels.IChannel?displayProperty=nameWithType>
- <xref:System.ServiceModel.Channels.IChannelFactory?displayProperty=nameWithType>
- <xref:System.ServiceModel.Channels.IChannelFactory%601?displayProperty=nameWithType>
- <xref:System.ServiceModel.Channels.IDuplexChannel?displayProperty=nameWithType>
- <xref:System.ServiceModel.Channels.IDuplexSession?displayProperty=nameWithType>
- <xref:System.ServiceModel.Channels.IDuplexSessionChannel?displayProperty=nameWithType>
- <xref:System.ServiceModel.Channels.IHttpCookieContainerManager?displayProperty=nameWithType>
- <xref:System.ServiceModel.Channels.IInputChannel?displayProperty=nameWithType>
- <xref:System.ServiceModel.Channels.IInputSession?displayProperty=nameWithType>
- <xref:System.ServiceModel.Channels.IInputSessionChannel?displayProperty=nameWithType>
- <xref:System.ServiceModel.Channels.IMessageProperty?displayProperty=nameWithType>
- <xref:System.ServiceModel.Channels.IOutputChannel?displayProperty=nameWithType>
- <xref:System.ServiceModel.Channels.IOutputSession?displayProperty=nameWithType>
- <xref:System.ServiceModel.Channels.IOutputSessionChannel?displayProperty=nameWithType>
- <xref:System.ServiceModel.Channels.IRequestChannel?displayProperty=nameWithType>
- <xref:System.ServiceModel.Channels.IRequestSessionChannel?displayProperty=nameWithType>
- <xref:System.ServiceModel.Channels.ISession?displayProperty=nameWithType>
- <xref:System.ServiceModel.Channels.ISessionChannel%601?displayProperty=nameWithType>
- <xref:System.ServiceModel.Channels.LocalClientSecuritySettings?displayProperty=nameWithType>
- <xref:System.ServiceModel.Channels.Message?displayProperty=nameWithType>
- <xref:System.ServiceModel.Channels.MessageBuffer?displayProperty=nameWithType>
- <xref:System.ServiceModel.Channels.MessageEncoder?displayProperty=nameWithType>
- <xref:System.ServiceModel.Channels.MessageEncoderFactory?displayProperty=nameWithType>
- <xref:System.ServiceModel.Channels.MessageEncodingBindingElement?displayProperty=nameWithType>
- <xref:System.ServiceModel.Channels.MessageFault?displayProperty=nameWithType>
- <xref:System.ServiceModel.Channels.MessageHeader?displayProperty=nameWithType>
- <xref:System.ServiceModel.Channels.MessageHeaderInfo?displayProperty=nameWithType>
- <xref:System.ServiceModel.Channels.MessageHeaders?displayProperty=nameWithType>
- <xref:System.ServiceModel.Channels.MessageProperties?displayProperty=nameWithType>
- <xref:System.ServiceModel.Channels.MessageState?displayProperty=nameWithType>
- <xref:System.ServiceModel.Channels.MessageVersion?displayProperty=nameWithType>
- <xref:System.ServiceModel.Channels.RequestContext?displayProperty=nameWithType>
- <xref:System.ServiceModel.Channels.SecurityBindingElement?displayProperty=nameWithType>
- <xref:System.ServiceModel.Channels.SecurityHeaderLayout?displayProperty=nameWithType>
- <xref:System.ServiceModel.Channels.SslStreamSecurityBindingElement?displayProperty=nameWithType>
- <xref:System.ServiceModel.Channels.TcpConnectionPoolSettings?displayProperty=nameWithType>
- <xref:System.ServiceModel.Channels.TcpTransportBindingElement?displayProperty=nameWithType>
- <xref:System.ServiceModel.Channels.TextMessageEncodingBindingElement?displayProperty=nameWithType>
- <xref:System.ServiceModel.Channels.TransportBindingElement?displayProperty=nameWithType>
- <xref:System.ServiceModel.Channels.TransportSecurityBindingElement?displayProperty=nameWithType>
- <xref:System.ServiceModel.Channels.WebSocketTransportSettings?displayProperty=nameWithType>
- <xref:System.ServiceModel.Channels.WebSocketTransportUsage?displayProperty=nameWithType>
- <xref:System.ServiceModel.Channels.WindowsStreamSecurityBindingElement?displayProperty=nameWithType>
- <xref:System.ServiceModel.Description.ClientCredentials?displayProperty=nameWithType>
- <xref:System.ServiceModel.Description.ContractDescription?displayProperty=nameWithType>
- <xref:System.ServiceModel.Description.DataContractSerializerOperationBehavior?displayProperty=nameWithType>
- <xref:System.ServiceModel.Description.FaultDescription?displayProperty=nameWithType>
- <xref:System.ServiceModel.Description.FaultDescriptionCollection?displayProperty=nameWithType>
- <xref:System.ServiceModel.Description.IContractBehavior?displayProperty=nameWithType>
- <xref:System.ServiceModel.Description.IEndpointBehavior?displayProperty=nameWithType>
- <xref:System.ServiceModel.Description.IOperationBehavior?displayProperty=nameWithType>
- <xref:System.ServiceModel.Description.MessageBodyDescription?displayProperty=nameWithType>
- <xref:System.ServiceModel.Description.MessageDescription?displayProperty=nameWithType>
- <xref:System.ServiceModel.Description.MessageDescriptionCollection?displayProperty=nameWithType>
- <xref:System.ServiceModel.Description.MessageDirection?displayProperty=nameWithType>
- <xref:System.ServiceModel.Description.MessageHeaderDescription?displayProperty=nameWithType>
- <xref:System.ServiceModel.Description.MessageHeaderDescriptionCollection?displayProperty=nameWithType>
- <xref:System.ServiceModel.Description.MessagePartDescription?displayProperty=nameWithType>
- <xref:System.ServiceModel.Description.MessagePartDescriptionCollection?displayProperty=nameWithType>
- <xref:System.ServiceModel.Description.MessagePropertyDescription?displayProperty=nameWithType>
- <xref:System.ServiceModel.Description.MessagePropertyDescriptionCollection?displayProperty=nameWithType>
- <xref:System.ServiceModel.Description.OperationDescription?displayProperty=nameWithType>
- <xref:System.ServiceModel.Description.OperationDescriptionCollection?displayProperty=nameWithType>
- <xref:System.ServiceModel.Description.ServiceEndpoint?displayProperty=nameWithType>
- <xref:System.ServiceModel.Dispatcher.ClientOperation?displayProperty=nameWithType>
- <xref:System.ServiceModel.Dispatcher.ClientRuntime?displayProperty=nameWithType>
- <xref:System.ServiceModel.Dispatcher.DispatchOperation?displayProperty=nameWithType>
- <xref:System.ServiceModel.Dispatcher.DispatchRuntime?displayProperty=nameWithType>
- <xref:System.ServiceModel.Dispatcher.EndpointDispatcher?displayProperty=nameWithType>
- <xref:System.ServiceModel.Dispatcher.IClientMessageFormatter?displayProperty=nameWithType>
- <xref:System.ServiceModel.Dispatcher.IClientMessageInspector?displayProperty=nameWithType>
- <xref:System.ServiceModel.Dispatcher.IClientOperationSelector?displayProperty=nameWithType>
- <xref:System.ServiceModel.Dispatcher.IParameterInspector?displayProperty=nameWithType>
- <xref:System.ServiceModel.Security.BasicSecurityProfileVersion?displayProperty=nameWithType>
- <xref:System.ServiceModel.Security.HttpDigestClientCredential?displayProperty=nameWithType>
- <xref:System.ServiceModel.Security.MessageSecurityException?displayProperty=nameWithType>
- <xref:System.ServiceModel.Security.SecureConversationVersion?displayProperty=nameWithType>
- <xref:System.ServiceModel.Security.SecurityAccessDeniedException?displayProperty=nameWithType>
- <xref:System.ServiceModel.Security.SecurityPolicyVersion?displayProperty=nameWithType>
- <xref:System.ServiceModel.Security.SecurityVersion?displayProperty=nameWithType>
- <xref:System.ServiceModel.Security.TrustVersion?displayProperty=nameWithType>
- <xref:System.ServiceModel.Security.UserNamePasswordClientCredential?displayProperty=nameWithType>
- <xref:System.ServiceModel.Security.WindowsClientCredential?displayProperty=nameWithType>
- <xref:System.ServiceModel.Security.Tokens.SecureConversationSecurityTokenParameters?displayProperty=nameWithType>
- <xref:System.ServiceModel.Security.Tokens.SecurityTokenParameters?displayProperty=nameWithType>
- <xref:System.ServiceModel.Security.Tokens.SupportingTokenParameters?displayProperty=nameWithType>
- <xref:System.ServiceModel.Security.Tokens.UserNameSecurityTokenParameters?displayProperty=nameWithType>

### <a name="differences-in-serializers"></a>序列化程序中的差异

与 <xref:System.Runtime.Serialization.DataContractSerializer>、 <xref:System.Runtime.Serialization.Json.DataContractJsonSerializer>和 <xref:System.Xml.Serialization.XmlSerializer> 类的序列化和反序列化有关的以下差异：

- 在 .NET Native 中， <xref:System.Runtime.Serialization.DataContractSerializer> 和 <xref:System.Runtime.Serialization.Json.DataContractJsonSerializer> 无法序列化或反序列化一个派生类，该派生类具有类型不是根序列化类型的基类成员。 例如，在以下代码中，尝试序列化或反序列化 `Y` 会导致出现错误：

  [!code-csharp[ProjectN#10](../../../samples/snippets/csharp/VS_Snippets_CLR/projectn/cs/compat3.cs#10)]

  类型 `InnerType` 对序列程序而言是未知的，因为基类的成员在序列化期间无法遍历。

- <xref:System.Runtime.Serialization.DataContractSerializer> 和 <xref:System.Runtime.Serialization.Json.DataContractJsonSerializer> 无法序列化实施 <xref:System.Collections.Generic.IEnumerable%601> 接口的类或结构。 例如，以下类无法序列化或反序列化：

- <xref:System.Xml.Serialization.XmlSerializer> 无法序列化以下对象值，因为它不知道将序列化的对象的类型：

- 如果将序列化的对象的类型是<xref:System.Xml.Serialization.XmlSerializer> ，则 <xref:System.Xml.XmlQualifiedName>无法序列化或反序列化。

- 所有序列程序（<xref:System.Runtime.Serialization.DataContractSerializer>、 <xref:System.Runtime.Serialization.Json.DataContractJsonSerializer>和 <xref:System.Xml.Serialization.XmlSerializer>）都无法为 <xref:System.Xml.Linq.XElement?displayProperty=nameWithType> 类型或包含 <xref:System.Xml.Linq.XElement>的类型生成序列代码。 它们会显示生成时间错误。

- 以下序列化类型的构造函数无法保证按照预期工作：

  - <xref:System.Runtime.Serialization.DataContractSerializer.%23ctor%28System.Type%2CSystem.Collections.Generic.IEnumerable%7BSystem.Type%7D%29>

  - <xref:System.Runtime.Serialization.DataContractSerializer.%23ctor%28System.Type%2CSystem.Runtime.Serialization.DataContractSerializerSettings%29>

  - <xref:System.Runtime.Serialization.DataContractSerializer.%23ctor%28System.Type%2CSystem.String%2CSystem.String%2CSystem.Collections.Generic.IEnumerable%7BSystem.Type%7D%29>

  - <xref:System.Runtime.Serialization.DataContractSerializer.%23ctor%28System.Type%2CSystem.Xml.XmlDictionaryString%2CSystem.Xml.XmlDictionaryString%2CSystem.Collections.Generic.IEnumerable%7BSystem.Type%7D%29>

  - <xref:System.Runtime.Serialization.Json.DataContractJsonSerializer.%23ctor%28System.Type%2CSystem.Runtime.Serialization.Json.DataContractJsonSerializerSettings%29>

  - <xref:System.Runtime.Serialization.Json.DataContractJsonSerializer.%23ctor%28System.Type%2CSystem.Collections.Generic.IEnumerable%7BSystem.Type%7D%29>

  - <xref:System.Xml.Serialization.XmlSerializer.%23ctor%28System.Type%2CSystem.String%29>

  - <xref:System.Xml.Serialization.XmlSerializer.%23ctor%28System.Type%2CSystem.Type%5B%5D%29>

  - <xref:System.Xml.Serialization.XmlSerializer.%23ctor%28System.Type%2CSystem.Xml.Serialization.XmlAttributeOverrides%29>

  - <xref:System.Xml.Serialization.XmlSerializer.%23ctor%28System.Type%2CSystem.Xml.Serialization.XmlRootAttribute%29>

  - <xref:System.Xml.Serialization.XmlSerializer.%23ctor%28System.Type%2CSystem.Xml.Serialization.XmlAttributeOverrides%2CSystem.Type%5B%5D%2CSystem.Xml.Serialization.XmlRootAttribute%2CSystem.String%29>

- <xref:System.Xml.Serialization.XmlSerializer> 无法为具有以下任意特性的方法的类型生成代码：

  - <xref:System.Runtime.Serialization.OnSerializingAttribute>

  - <xref:System.Runtime.Serialization.OnSerializedAttribute>

  - <xref:System.Runtime.Serialization.OnDeserializingAttribute>

  - <xref:System.Runtime.Serialization.OnDeserializedAttribute>

- <xref:System.Xml.Serialization.XmlSerializer> 不兼容 <xref:System.Xml.Serialization.IXmlSerializable> 自定义序列化接口。 如果你的类实施此接口， <xref:System.Xml.Serialization.XmlSerializer> 会考虑普通旧 CLR 对象 (POCO) 类型并仅对其公共属性进行序列化。

- 序列化纯 <xref:System.Exception> 对象并不适合 <xref:System.Runtime.Serialization.DataContractSerializer> 和 <xref:System.Runtime.Serialization.Json.DataContractJsonSerializer> 。

<a name="VS"></a>

## <a name="visual-studio-differences"></a>Visual Studio 差异

**异常和调试**

在调试器中运行使用 .NET Native 编译的应用时，会为以下异常类型启用第一次异常：

- <xref:System.MemberAccessException>

- <xref:System.TypeAccessException>

**构建应用程序**

使用 Visual Studio 默认使用的 x86 构建工具。 我们不推荐使用 AMD64 MSBuild 工具（位置路径为 C:\Program Files (x86)\MSBuild\12.0\bin\amd64），因为这些工具会导致构建问题。

**探查器**

- Visual Studio CPU 探查器和 XAML 内存探查器不会正确显示 Just-My-Code。

- XAML 内存探查器不会精确显示托管的堆数据。

- CPU 探查器不会正确区分模块和显示前缀的功能名。

**单元测试库项目**

不支持在 Windows 应用商店应用项目的单元测试库上启用 .NET Native，并导致项目未能生成。

## <a name="see-also"></a>另请参阅

- [入门](getting-started-with-net-native.md)
- [运行时指令 (rd.xml) 配置文件引用](runtime-directives-rd-xml-configuration-file-reference.md)
- [适用于 Windows 应用商店应用的 .NET 概述](/previous-versions/windows/apps/br230302(v=vs.140))
- [.NET Framework 对 Windows 应用商店应用和 Windows 运行时的支持情况](../cross-platform/support-for-windows-store-apps-and-windows-runtime.md)
