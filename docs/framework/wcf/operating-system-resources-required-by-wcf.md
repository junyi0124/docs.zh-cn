---
title: WCF 所要求的操作系统资源
ms.date: 03/30/2017
ms.assetid: cdd9a331-53fe-4e0d-bdfe-782264aec5c9
ms.openlocfilehash: c2c26d769424a8d0655591cb10b4b13df8a15884
ms.sourcegitcommit: 9b2ef64c4fc10a4a10f28a223d60d17d7d249ee8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/26/2019
ms.locfileid: "72961139"
---
# <a name="operating-system-resources-required-by-wcf"></a>WCF 所要求的操作系统资源

Windows Communication Foundation （WCF）依赖于操作系统提供的多个资源才能正常运行。 下表列出了这些资源：

|资源|描述|
|--------------|-----------------|
|Microsoft 分布式事务协调器 (MSDTC)|支持 OleTx 事务所必需。|
|Internet Information Services (IIS)|如果希望使用 IIS 来承载应用程序，则是必需的。|
|Windows 进程激活服务 (WAS)|如果希望使用 WAS 来承载应用程序，则是必需的。|
