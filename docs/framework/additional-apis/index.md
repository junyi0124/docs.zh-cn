---
title: 其他类库和 API
description: 探索 .NET 中的其他类库和 Api，包括带外 (OOB) 项目、平台特定的库和私有 Api。
ms.date: 08/11/2020
helpviewer_keywords:
- Additional class libraries
- Additional managed libraries
- .NET Framework out-of-band releases
- out-of-band releases
ms.assetid: cf2d9006-b631-4e5d-81cd-20aab78c60f1
ms.topic: conceptual
ms.openlocfilehash: 55cb37cc2c9184eeb55ee0aab39e97f4a3f7b7d8
ms.sourcegitcommit: eb7e87496f42361b1da98562dd75b516c9d58bbc
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/09/2020
ms.locfileid: "91877634"
---
# <a name="additional-class-libraries-and-apis"></a>其他类库和 API

本文列出了以带外方式发布的 .NET Framework Api，这些 Api 以特定的平台为目标，或者是私有或内部类型。

## <a name="oob-projects"></a>OOB 项目

为了改善跨平台开发并及早引入新功能，一些 .NET Framework 功能已在 (OOB) 中发布。

| 项目 | 说明 |
| ------- | ----------- |
| <xref:System.Collections.Immutable> | 提供的集合数是线程安全的，并且保证决不会更改该集合的内容。 |
| <xref:System.Net.Http.WinHttpHandler> | 基于 Windows 的 WinHTTP 接口为 <xref:System.Net.Http.HttpClient> 提供消息处理程序。 |
| <xref:System.Numerics> | 提供可利用基于 SIMD 硬件的加速的向量类型库。|
| <xref:System.Threading.Tasks.Dataflow> | TPL 数据流库提供数据流组件以帮助提高启用并发的应用程序的稳健性。 |

## <a name="platform-specific-libraries"></a>特定于平台的库

某些库面向特定的平台。 例如， <xref:System.Text.CodePagesEncodingProvider> 类使代码页编码可用于使用 .NET Framework 开发的 UWP 应用。

| 项目 | 说明 |
| ------- | ----------- |
| <xref:System.Text.CodePagesEncodingProvider> | 扩展 <xref:System.Text.EncodingProvider> 类，使代码页编码可用于面向通用 Windows 平台的应用程序。 |

## <a name="private-apis"></a>私有 API

这些 Api 支持产品基础结构，不应在代码中直接使用。

* [SmiOrderProperty 属性（& e）](microsoft.sqlserver.server.smiorderproperty.item.md)
* [PrepForRemoting 方法](system.exception.prepforremoting.md)
* [SqlTypes. SqlChars 属性](system.data.sqltypes.sqlchars.stream.md)
* [SqlTypes. SqlStreamChars 构造函数](system.data.sqltypes.sqlstreamchars.-ctor.md)
* [SqlTypes. SqlStreamChars. CanSeek 属性](system.data.sqltypes.sqlstreamchars.canseek.md)
* [SqlTypes. SqlStreamChars 属性](system.data.sqltypes.sqlstreamchars.isnull.md)
* [SqlTypes. SqlStreamChars 属性](system.data.sqltypes.sqlstreamchars.length.md)
* [SqlTypes. SqlStreamChars 方法](system.data.sqltypes.sqlstreamchars.close.md)
* [SqlTypes. SqlStreamChars 方法](system.data.sqltypes.sqlstreamchars.dispose.md)
* [SqlTypes. SqlStreamChars 方法](system.data.sqltypes.sqlstreamchars.flush.md)
* [SqlTypes. SqlStreamChars 方法](system.data.sqltypes.sqlstreamchars.read.md)
* [SqlTypes. SqlStreamChars 方法](system.data.sqltypes.sqlstreamchars.seek.md)
* [SqlTypes. SqlStreamChars. SetLength 方法](system.data.sqltypes.sqlstreamchars.setlength.md)
* [SqlTypes. SqlStreamChars 方法](system.data.sqltypes.sqlstreamchars.write.md)
* [MemoryStream. InternalGetOriginAndLength 方法](system.io.memorystream.internalgetoriginandlength.md)
* [ComNetOS 类](system.net.comnetos.md)
* [系统 .Net. 连接类](connection.md)
* [系统 \_ WriteList 字段](m_writelist.md)
* [ConnectionGroup 类](connectiongroup.md)
* [系统 ConnectionGroup \_ ConnectionList 字段](m_connectionlist.md)
* [系统 ConnectStream 属性](system.net.connectstream.connection.md)
* [CoreResponseData 类](coreresponsedata.md)
* [系统 CoreResponseData \_ ResponseHeaders 字段](coreresponsedata_m_responseheaders.md)
* [系统 CoreResponseData \_ StatusCode 字段](coreresponsedata_m_statuscode.md)
* [ExceptionHelper 类](system.net.exceptionhelper.md)
* [HttpStatusDescription 类](system.net.httpstatusdescription.md)
* [系统 HttpWebRequest。 \_AutoRedirects 字段](_autoredirects.md)
* [系统 HttpWebRequest。 \_CoreResponse 字段](httpwebrequest__coreresponse.md)
* [系统 HttpWebRequest。 \_Httpresponse.cache 字段](_httpresponse.md)
* [系统 .Net. 日志类](system.net.logging.md)
* [系统 MailAddressParser 类](system.net.mail.mailaddressparser.md)
* [系统 QuotedPairReader 类](system.net.mail.quotedpairreader.md)
* [MailBnfHelper 类](system.net.mime.mailbnfhelper.md)
* [PooledStream. NetworkStream 属性](system.net.pooledstream.networkstream.md)
* [RtcState 类](system.net.rtcstate.md)
* [SslState. SslProtocol 属性](system.net.security.sslstate.sslprotocol.md)
* [系统 ServicePoint \_ ConnectionGroupList 字段](m_connectiongrouplist.md)
* [ServicePointManager. CloseConnectionGroups 方法](system.net.servicepointmanager.closeconnectiongroups.md)
* [系统 ServicePointManager \_ ServicePointTable 字段](s_servicepointtable.md)
* [System.Net.TlsStream.m_Worker 字段](system.net.tlsstream.m_worker.md)
* [UnsafeNclNativeMethods 类](system.net.unsafenclnativemethods.md)
* [设置了 webheadercollection. AddInternal 方法](system.net.webheadercollection.addinternal.md)
* [System.servicemodel. BodyToString 方法](system.servicemodel.channels.message.bodytostring.md)
* [System.servicemodel. WriteStartHeaders 方法](system.servicemodel.channels.message.writestartheaders.md)
* [ControlBuilderInterceptor 类。](controlbuilderinterceptor-class.md)
* [GridViewHeaderRowPresenter. FindHeaderByColumn 方法](system.windows.controls.gridviewheaderrowpresenter.findheaderbycolumn.md)
* [GridViewHeaderRowPresenter. MakeParentItemsControlGotFocus 方法](system.windows.controls.gridviewheaderrowpresenter.makeparentitemscontrolgotfocus.md)
* [GridViewHeaderRowPresenter. PrepareHeaderDrag 方法](system.windows.controls.gridviewheaderrowpresenter.prepareheaderdrag.md)
* [VisualDiagnostics. s \_ isDebuggerCheckDisabledForTestPurposes 字段](s-isdebuggercheckdisabledfortestpurposes-field.md)
* ["DataMemberFieldEditor" 类](datamemberfieldeditor-class.md)
* ["DataMemberListEditor" 类](datamemberlisteditor-class.md)
* [System.Xml.XmlCreateSqlReader 方法](system.xml.xmlreader.createsqlreader.md)
* [adodb.recordset.连接接口](adodb.connection.md)
* [adodb.recordset.EventReason 枚举](adodb.eventreasonenum.md)
* [adodb.recordset.EventStatus 枚举](adodb.eventstatusenum.md)
* [stdole.DISPPARAMS 结构](stdole.dispparams.md)
* [stdole.EXCEPINFO 结构](stdole.excepinfo.md)
* [stdole.IFont.Name 属性](stdole.ifont.name.md)
* [stdole.IFontDisp 接口](stdole.ifontdisp.md)
* [stdole.IPicture 属性](stdole.ipicture.handle.md)
* [stdole.IPictureDisp 属性](stdole.ipicturedisp.handle.md)
* [stdole.StdFont 接口](stdole.stdfont.md)
* [stdole.StdPicture 接口](stdole.stdpicture.md)

## <a name="see-also"></a>请参阅

* [.NET Framework 和带外版本](../get-started/the-net-framework-and-out-of-band-releases.md)
