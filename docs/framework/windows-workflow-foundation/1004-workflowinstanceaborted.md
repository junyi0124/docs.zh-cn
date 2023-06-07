---
title: 1004 - WorkflowInstanceAborted
ms.date: 03/30/2017
ms.assetid: edb9ab8c-0b9a-488d-aa96-9c8c7984b53c
ms.openlocfilehash: d34f6f1ab6af8e06a0f28fb043faf9fe16a8b211
ms.sourcegitcommit: 9b552addadfb57fab0b9e7852ed4f1f1b8a42f8e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/23/2019
ms.locfileid: "62008611"
---
# <a name="1004---workflowinstanceaborted"></a>1004 - WorkflowInstanceAborted

## <a name="properties"></a>属性

|||
|-|-|
|Id|1004|
|关键字|WFRuntime|
|级别|信息|
|通道|Microsoft-Windows-应用程序服务器-应用程序/调试|

## <a name="description"></a>描述

指示工作流实例已中止并且具有异常。

## <a name="message"></a>消息

WorkflowInstance Id“%1”因出现异常而中止。

## <a name="details"></a>详细信息

|数据项名称|数据项类型|描述|
|--------------------|--------------------|-----------------|
|WorkflowInstanceId|`xs:string`|工作流的实例 ID|
|例外|`xs:string`|异常的异常详细信息|
|应用程序域|`xs:string`|由 AppDomain.CurrentDomain.FriendlyName 返回的字符串。|
