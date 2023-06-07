---
title: System.ServiceModel.Channels.ConnectionPoolCloseException
ms.date: 03/30/2017
ms.assetid: 8358898e-129e-4fac-a6bf-bf3aa4293ae2
ms.openlocfilehash: d8edb8e916578247e62e3a3eb3b11b80f93416cd
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96286950"
---
# <a name="systemservicemodelchannelsconnectionpoolcloseexception"></a>System.ServiceModel.Channels.ConnectionPoolCloseException

关闭此连接池中的连接时发生异常。  
  
## <a name="description"></a>描述  

 此错误级别跟踪表明关闭 Windows Communication Foundation (WCF) 的连接池功能所使用的连接池时出错。 一种可能的原因是一个池连接或一组池连接在 CloseTimeout 内未成功关闭。 当引发此异常时，将中止该池内所有剩余的未关闭连接，并放弃其他池内的未关闭连接。  
  
## <a name="see-also"></a>另请参阅

- [跟踪](index.md)
- [使用跟踪来排除应用程序故障](using-tracing-to-troubleshoot-your-application.md)
- [管理和诊断](../index.md)
