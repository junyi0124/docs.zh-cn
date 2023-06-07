---
title: 二进制序列化
description: 本文介绍了 .NET Core 支持的二进制序列化和类型。 请注意二进制序列化和替代项的危险。
ms.date: 01/02/2018
helpviewer_keywords:
- binary serialization
- serialization, about serialization
- deserialization
- binary serialization, about serialization
- binary serialization, .net core serialization
- serialization, cross-framework
ms.assetid: 2b1ea3be-1152-4032-b2b3-07794054c405
author: ViktorHofer
ms.openlocfilehash: bfb504862232345db07bdc92993069fc87afdbeb
ms.sourcegitcommit: 74d05613d6c57106f83f82ce8ee71176874ea3f0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/03/2020
ms.locfileid: "93282090"
---
# <a name="binary-serialization"></a>二进制序列化

可以将序列化定义为一个将对象状态存储到存储介质的过程。 在这个过程中，对象的公共字段和私有字段以及类（包括含有该类的程序集）的名称，将转换成字节流，而字节流接着将写入数据流。 随后对该对象进行反序列化时，将创建原始对象的准确克隆。

在面向对象的环境中实现序列化机制时，必须多在易用性与灵活性之间做出权衡。 很大程度上，这个过程可以自动完成，但前提是您对该过程拥有足够的控制权。 例如，如果简单的二进制序列化不足，或者可能有特定原因决定需要对类中的哪些字段进行序列化，可能就会出现这种情况。 以下章节验证了随 .NET Framework 一起提供的可靠序列化机制，并强调了根据需要自定义该过程所能使用的一些重要功能。

> [!NOTE]
> 如果使用不同的 .NET 版本序列化和反序列化以 UTF-8 或 UTF-7 编码的对象，则不保留该对象的状态。

[!INCLUDE [binary-serialization-warning](../../../includes/binary-serialization-warning.md)]

二进制序列化允许修改对象内部的私有成员，从而更改其状态。 因此，建议采用在公共 API 表面运行的其他序列化框架，例如 <xref:System.Text.Json?displayProperty=fullName>。

## <a name="net-core"></a>.NET Core

.NET Core 支持类型子集的二进制序列化。 可在下面的[可序列化类型](#serializable-types)部分查看受支持的类型列表。 列出的类型是完全可以在 .NET Framework 4.5.1 及更高版本之间和 .NET Core 2.0 及更高版本之间进行序列化的。 其他 .NET 实现（如 Mono）并未正式得到支持，但应该也能使用。

### <a name="serializable-types"></a>可序列化类型

> [!div class="mx-tdCol2BreakAll"]
> | 类型 | 说明 |
> | - | - |
> | <xref:Microsoft.CSharp.RuntimeBinder.RuntimeBinderException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:Microsoft.CSharp.RuntimeBinder.RuntimeBinderInternalCompilerException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.AccessViolationException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.AggregateException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.AppDomainUnloadedException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.ApplicationException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.ArgumentException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.ArgumentNullException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.ArgumentOutOfRangeException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.ArithmeticException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.Array?displayProperty=nameWithType> | |
> | <xref:System.ArraySegment%601?displayProperty=nameWithType> | |
> | <xref:System.ArrayTypeMismatchException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.Attribute?displayProperty=nameWithType> | |
> | <xref:System.BadImageFormatException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.Boolean?displayProperty=nameWithType> | |
> | <xref:System.Byte?displayProperty=nameWithType> | |
> | <xref:System.CannotUnloadAppDomainException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.Char?displayProperty=nameWithType> | |
> | <xref:System.Collections.ArrayList?displayProperty=nameWithType> | |
> | <xref:System.Collections.BitArray?displayProperty=nameWithType> | |
> | <xref:System.Collections.Comparer?displayProperty=nameWithType> | |
> | <xref:System.Collections.DictionaryEntry?displayProperty=nameWithType> | |
> | <xref:System.Collections.Generic.Comparer%601?displayProperty=nameWithType> | |
> | <xref:System.Collections.Generic.Dictionary%602?displayProperty=nameWithType> | |
> | <xref:System.Collections.Generic.EqualityComparer%601?displayProperty=nameWithType> | |
> | <xref:System.Collections.Generic.HashSet%601?displayProperty=nameWithType> | |
> | <xref:System.Collections.Generic.KeyNotFoundException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.Collections.Generic.KeyValuePair%602?displayProperty=nameWithType> | |
> | <xref:System.Collections.Generic.LinkedList%601?displayProperty=nameWithType> | |
> | <xref:System.Collections.Generic.List%601?displayProperty=nameWithType> | |
> | <xref:System.Collections.Generic.Queue%601?displayProperty=nameWithType> | |
> | <xref:System.Collections.Generic.SortedDictionary%602?displayProperty=nameWithType> | |
> | <xref:System.Collections.Generic.SortedList%602?displayProperty=nameWithType> | |
> | <xref:System.Collections.Generic.SortedSet%601?displayProperty=nameWithType> | |
> | <xref:System.Collections.Generic.Stack%601?displayProperty=nameWithType> | |
> | <xref:System.Collections.Hashtable?displayProperty=nameWithType> | |
> | <xref:System.Collections.ObjectModel.Collection%601?displayProperty=nameWithType> | |
> | <xref:System.Collections.ObjectModel.KeyedCollection%602?displayProperty=nameWithType> | |
> | <xref:System.Collections.ObjectModel.ObservableCollection%601?displayProperty=nameWithType> | |
> | <xref:System.Collections.ObjectModel.ReadOnlyCollection%601?displayProperty=nameWithType> | |
> | <xref:System.Collections.ObjectModel.ReadOnlyDictionary%602?displayProperty=nameWithType> | |
> | <xref:System.Collections.ObjectModel.ReadOnlyObservableCollection%601?displayProperty=nameWithType> | |
> | <xref:System.Collections.Queue?displayProperty=nameWithType> | |
> | <xref:System.Collections.SortedList?displayProperty=nameWithType> | |
> | <xref:System.Collections.Specialized.HybridDictionary?displayProperty=nameWithType> | |
> | <xref:System.Collections.Specialized.ListDictionary?displayProperty=nameWithType> | |
> | <xref:System.Collections.Specialized.OrderedDictionary?displayProperty=nameWithType> | |
> | <xref:System.Collections.Specialized.StringCollection?displayProperty=nameWithType> | |
> | <xref:System.Collections.Specialized.StringDictionary?displayProperty=nameWithType> | |
> | <xref:System.Collections.Stack?displayProperty=nameWithType> | |
> | `System.Collections.Generic.NonRandomizedStringEqualityComparer` | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.ComponentModel.BindingList%601?displayProperty=nameWithType> | |
> | <xref:System.ComponentModel.DataAnnotations.ValidationException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.ComponentModel.Design.CheckoutException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.ComponentModel.InvalidAsynchronousStateException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.ComponentModel.InvalidEnumArgumentException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.ComponentModel.LicenseException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。<br/>不支持从 .NET Framework 到 .NET Core 的序列化。 |
> | <xref:System.ComponentModel.WarningException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.ComponentModel.Win32Exception?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.Configuration.ConfigurationErrorsException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.Configuration.ConfigurationException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.Configuration.Provider.ProviderException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.Configuration.SettingsPropertyIsReadOnlyException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.Configuration.SettingsPropertyNotFoundException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.Configuration.SettingsPropertyWrongTypeException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.ContextMarshalException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.DBNull?displayProperty=nameWithType> | 从 .NET Core 2.0.2 和更高版本开始。 |
> | <xref:System.Data.Common.DbException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.Data.ConstraintException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.Data.DBConcurrencyException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.Data.DataException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.Data.DataSet?displayProperty=nameWithType> | |
> | <xref:System.Data.DataTable?displayProperty=nameWithType> | 如果将 `RemotingFormat` 设置为 `SerializationFormat.Binary`，则只能与 .NET Core 2.1 和更高版本进行交换。 |
> | <xref:System.Data.DeletedRowInaccessibleException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.Data.DuplicateNameException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.Data.EvaluateException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.Data.InRowChangingEventException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.Data.InvalidConstraintException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.Data.InvalidExpressionException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.Data.MissingPrimaryKeyException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.Data.NoNullAllowedException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.Data.Odbc.OdbcException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.Data.OperationAbortedException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.Data.PropertyCollection?displayProperty=nameWithType> | |
> | <xref:System.Data.ReadOnlyException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.Data.RowNotInTableException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.Data.SqlClient.SqlException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。<br/>不支持从 .NET Framework 到 .NET Core 的序列化 |
> | <xref:System.Data.SqlTypes.SqlAlreadyFilledException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.Data.SqlTypes.SqlBoolean?displayProperty=nameWithType> | |
> | <xref:System.Data.SqlTypes.SqlByte?displayProperty=nameWithType> | |
> | <xref:System.Data.SqlTypes.SqlDateTime?displayProperty=nameWithType> | |
> | <xref:System.Data.SqlTypes.SqlDouble?displayProperty=nameWithType> | |
> | <xref:System.Data.SqlTypes.SqlGuid?displayProperty=nameWithType> | |
> | <xref:System.Data.SqlTypes.SqlInt16?displayProperty=nameWithType> | |
> | <xref:System.Data.SqlTypes.SqlInt32?displayProperty=nameWithType> | |
> | <xref:System.Data.SqlTypes.SqlInt64?displayProperty=nameWithType> | |
> | <xref:System.Data.SqlTypes.SqlNotFilledException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.Data.SqlTypes.SqlNullValueException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.Data.SqlTypes.SqlString?displayProperty=nameWithType> | |
> | <xref:System.Data.SqlTypes.SqlTruncateException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.Data.SqlTypes.SqlTypeException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.Data.StrongTypingException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.Data.SyntaxErrorException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.Data.VersionNotFoundException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.DataMisalignedException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.DateTime?displayProperty=nameWithType> | |
> | <xref:System.DateTimeOffset?displayProperty=nameWithType> | |
> | <xref:System.Decimal?displayProperty=nameWithType> | |
> | `System.Diagnostics.Contracts.ContractException` | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.Diagnostics.Tracing.EventSourceException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.IO.DirectoryNotFoundException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.DirectoryServices.AccountManagement.MultipleMatchesException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.DirectoryServices.AccountManagement.NoMatchingPrincipalException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.DirectoryServices.AccountManagement.PasswordException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.DirectoryServices.AccountManagement.PrincipalException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.DirectoryServices.AccountManagement.PrincipalExistsException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.DirectoryServices.AccountManagement.PrincipalOperationException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.DirectoryServices.AccountManagement.PrincipalServerDownException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.DirectoryServices.ActiveDirectory.ActiveDirectoryObjectExistsException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.DirectoryServices.ActiveDirectory.ActiveDirectoryObjectNotFoundException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.DirectoryServices.ActiveDirectory.ActiveDirectoryOperationException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.DirectoryServices.ActiveDirectory.ActiveDirectoryServerDownException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.DirectoryServices.ActiveDirectory.ForestTrustCollisionException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.DirectoryServices.ActiveDirectory.SyncFromAllServersOperationException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.DirectoryServices.DirectoryServicesCOMException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.DirectoryServices.Protocols.BerConversionException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.DirectoryServices.Protocols.DirectoryException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.DirectoryServices.Protocols.DirectoryOperationException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.DirectoryServices.Protocols.LdapException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.DirectoryServices.Protocols.TlsOperationException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.DivideByZeroException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.DllNotFoundException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.Double?displayProperty=nameWithType> | |
> | <xref:System.Drawing.Color?displayProperty=nameWithType> | |
> | <xref:System.Drawing.Point?displayProperty=nameWithType> | |
> | <xref:System.Drawing.PointF?displayProperty=nameWithType> | |
> | <xref:System.Drawing.Rectangle?displayProperty=nameWithType> | |
> | <xref:System.Drawing.RectangleF?displayProperty=nameWithType> | |
> | <xref:System.Drawing.Size?displayProperty=nameWithType> | |
> | <xref:System.Drawing.SizeF?displayProperty=nameWithType> | |
> | <xref:System.DuplicateWaitObjectException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.EntryPointNotFoundException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.Enum?displayProperty=nameWithType> | |
> | <xref:System.EventArgs?displayProperty=nameWithType> | 从 .NET Core 2.0.6 开始。 |
> | <xref:System.Exception?displayProperty=nameWithType> | |
> | <xref:System.ExecutionEngineException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.FieldAccessException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.FormatException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.Globalization.CompareInfo?displayProperty=nameWithType> | |
> | <xref:System.Globalization.CultureNotFoundException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.Globalization.SortVersion?displayProperty=nameWithType> | |
> | <xref:System.Guid?displayProperty=nameWithType> | |
> | `System.IO.Compression.ZLibException` | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.IO.DriveNotFoundException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.IO.EndOfStreamException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.IO.FileFormatException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.IO.FileLoadException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.IO.FileNotFoundException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.IO.IOException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.IO.InternalBufferOverflowException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.IO.InvalidDataException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.IO.IsolatedStorage.IsolatedStorageException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.IO.PathTooLongException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.IndexOutOfRangeException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.InsufficientExecutionStackException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.InsufficientMemoryException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.Int16?displayProperty=nameWithType> | |
> | <xref:System.Int32?displayProperty=nameWithType> | |
> | <xref:System.Int64?displayProperty=nameWithType> | |
> | <xref:System.IntPtr?displayProperty=nameWithType> | |
> | <xref:System.InvalidCastException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.InvalidOperationException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.InvalidProgramException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.InvalidTimeZoneException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.MemberAccessException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.MethodAccessException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.MissingFieldException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.MissingMemberException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.MissingMethodException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.MulticastNotSupportedException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.Net.Cookie?displayProperty=nameWithType> | |
> | <xref:System.Net.CookieCollection?displayProperty=nameWithType> | |
> | <xref:System.Net.CookieContainer?displayProperty=nameWithType> | |
> | <xref:System.Net.CookieException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.Net.HttpListenerException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.Net.Mail.SmtpException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.Net.Mail.SmtpFailedRecipientException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.Net.Mail.SmtpFailedRecipientsException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.Net.NetworkInformation.NetworkInformationException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.Net.NetworkInformation.PingException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.Net.ProtocolViolationException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.Net.Sockets.SocketException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.Net.WebException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.Net.WebSockets.WebSocketException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.NotFiniteNumberException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.NotImplementedException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.NotSupportedException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.NullReferenceException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.Nullable%601?displayProperty=nameWithType> | |
> | <xref:System.Numerics.BigInteger?displayProperty=nameWithType> | |
> | <xref:System.Numerics.Complex?displayProperty=nameWithType> | |
> | <xref:System.Object?displayProperty=nameWithType> | |
> | <xref:System.ObjectDisposedException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.OperationCanceledException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.OutOfMemoryException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.OverflowException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.PlatformNotSupportedException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.RankException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.Reflection.AmbiguousMatchException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.Reflection.CustomAttributeFormatException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.Reflection.InvalidFilterCriteriaException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.Reflection.ReflectionTypeLoadException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。<br/>不支持从 .NET Framework 到 .NET Core 的序列化。 |
> | <xref:System.Reflection.TargetException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.Reflection.TargetInvocationException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.Reflection.TargetParameterCountException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.Resources.MissingManifestResourceException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.Resources.MissingSatelliteAssemblyException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.Runtime.CompilerServices.RuntimeWrappedException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.Runtime.InteropServices.COMException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.Runtime.InteropServices.ExternalException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.Runtime.InteropServices.InvalidComObjectException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.Runtime.InteropServices.InvalidOleVariantTypeException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.Runtime.InteropServices.MarshalDirectiveException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.Runtime.InteropServices.SEHException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.Runtime.InteropServices.SafeArrayRankMismatchException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.Runtime.InteropServices.SafeArrayTypeMismatchException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.Runtime.Serialization.InvalidDataContractException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.Runtime.Serialization.SerializationException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.SByte?displayProperty=nameWithType> | |
> | <xref:System.Security.AccessControl.PrivilegeNotHeldException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.Security.Authentication.AuthenticationException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.Security.Authentication.InvalidCredentialException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.Security.Cryptography.CryptographicException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.Security.Cryptography.CryptographicUnexpectedOperationException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | `System.Security.Cryptography.Xml.CryptoSignedXmlRecursionException` | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.Security.HostProtectionException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.Security.Policy.PolicyException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.Security.Principal.IdentityNotMappedException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.Security.SecurityException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。<br/>有限的序列化数据。 |
> | <xref:System.Security.VerificationException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.Security.XmlSyntaxException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.ServiceProcess.TimeoutException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.Single?displayProperty=nameWithType> | |
> | <xref:System.StackOverflowException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.String?displayProperty=nameWithType> | |
> | <xref:System.StringComparer?displayProperty=nameWithType> | |
> | <xref:System.SystemException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.Text.DecoderFallbackException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.Text.EncoderFallbackException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.Text.RegularExpressions.RegexMatchTimeoutException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.Text.StringBuilder?displayProperty=nameWithType> | |
> | <xref:System.Threading.AbandonedMutexException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.Threading.BarrierPostPhaseException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.Threading.LockRecursionException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.Threading.SemaphoreFullException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.Threading.SynchronizationLockException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.Threading.Tasks.TaskCanceledException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.Threading.Tasks.TaskSchedulerException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.Threading.ThreadAbortException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.Threading.ThreadInterruptedException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.Threading.ThreadStartException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.Threading.ThreadStateException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.Threading.WaitHandleCannotBeOpenedException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.TimeSpan?displayProperty=nameWithType> | |
> | <xref:System.TimeZoneInfo.AdjustmentRule?displayProperty=nameWithType> | |
> | <xref:System.TimeZoneInfo?displayProperty=nameWithType> | |
> | <xref:System.TimeZoneNotFoundException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.TimeoutException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.Transactions.TransactionAbortedException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.Transactions.TransactionException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.Transactions.TransactionInDoubtException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.Transactions.TransactionManagerCommunicationException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.Transactions.TransactionPromotionException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.Tuple?displayProperty=nameWithType> | |
> | <xref:System.TypeAccessException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.TypeInitializationException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.TypeLoadException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.TypeUnloadedException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.UInt16?displayProperty=nameWithType> | |
> | <xref:System.UInt32?displayProperty=nameWithType> | |
> | <xref:System.UInt64?displayProperty=nameWithType> | |
> | <xref:System.UIntPtr?displayProperty=nameWithType> | |
> | <xref:System.UnauthorizedAccessException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.Uri?displayProperty=nameWithType> | |
> | <xref:System.UriFormatException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.ValueTuple?displayProperty=nameWithType> | 在 .NET Framework 4.7 及早期版本中不可序列化。 |
> | <xref:System.ValueType?displayProperty=nameWithType> | |
> | <xref:System.Version?displayProperty=nameWithType> | |
> | <xref:System.WeakReference%601?displayProperty=nameWithType> | |
> | <xref:System.WeakReference?displayProperty=nameWithType> | |
> | <xref:System.Xml.Schema.XmlSchemaException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.Xml.Schema.XmlSchemaInferenceException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.Xml.Schema.XmlSchemaValidationException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.Xml.XPath.XPathException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.Xml.XmlException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.Xml.Xsl.XsltCompileException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |
> | <xref:System.Xml.Xsl.XsltException?displayProperty=nameWithType> | 从 .NET Core 2.0.4 开始。 |

## <a name="see-also"></a>请参阅

- <xref:System.Runtime.Serialization>\
包含可用于序列化和反序列化对象的类。

- [XML 和 SOAP 序列化](xml-and-soap-serialization.md)\
描述随公共语言运行库一起提供的 XML 序列化机制。

- [安全和序列化](../../framework/misc/security-and-serialization.md)\
描述写入执行序列化的代码时需要遵循的安全编码原则。

- [.NET 远程处理](/previous-versions/dotnet/netframework-4.0/72x4h507(v=vs.100))\
描述 .NET Framework 中提供的用于远程通信的多种方法。

- [使用 ASP.NET 创建的 XML Web service 以及 XML Web service 客户端](/previous-versions/dotnet/netframework-4.0/7bkzywba(v=vs.100))\
描述并解释如何对使用 ASP.NET 创建的 XML Web services 进行编程的文章。
