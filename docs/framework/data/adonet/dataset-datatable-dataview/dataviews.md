---
title: DataView
ms.date: 03/30/2017
ms.assetid: 0fe5dfa2-c1cd-435f-90b6-b4dd2e3ef34b
ms.openlocfilehash: c5692fcfd1863642bcdf87cbd495d793bce0cbe4
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91203704"
---
# <a name="dataviews"></a>DataView

您可以利用 <xref:System.Data.DataView> 创建存储在 <xref:System.Data.DataTable>（一种通常在数据绑定应用程序中使用的功能）中的数据的不同视图。 使用 **DataView**，你可以使用不同的排序顺序公开表中的数据，并且可以按行状态或基于筛选器表达式来筛选数据。

 **DataView**提供基础**DataTable**中的数据的动态视图：内容、排序和成员身份在更改发生时反映更改。 此行为不同于**DataTable**的**Select**方法，该方法 <xref:System.Data.DataRow> 根据特定筛选器和/或排序顺序从表返回数组：此内容反映了对基础表的更改，但其成员资格和顺序保持静态。 **DataView**的动态功能使其成为数据绑定应用程序的理想之选。

 数据 **视图为您提供了** 一组数据的动态视图，这与数据库视图非常类似，您可以将不同的排序和筛选条件应用于该视图。 但是，与数据库视图不同， **DataView** 不能被视为表，也不能提供联接的表的视图。 还不能排除源表中存在的列，也不能追加源表中不存在的列，如计算列。

 您可以使用 <xref:System.Data.DataView.DataViewManager%2A> 来管理 **数据集中**所有表的视图设置。 **DataViewManager**为你提供了一种方便的方法来管理每个表的默认视图设置。 将控件绑定到 **数据集**的多个表时，绑定到 **DataViewManager** 是理想的选择。

## <a name="in-this-section"></a>本节内容

 [创建 DataView](creating-a-dataview.md)介绍如何为**DataTable**创建**DataView** 。

 [排序和筛选数据](sorting-and-filtering-data.md) 描述如何设置 **DataView** 的属性以返回满足特定筛选条件的数据行子集，或返回按特定排序顺序返回的数据。

 [Datarow 和 datarowview](datarows-and-datarowviews.md) 描述如何访问由 **DataView**公开的数据。

 [查找行](finding-rows.md) 介绍如何在 **DataView**中查找特定的行。

 [Childview 和关系](childviews-and-relations.md) 介绍如何使用 **DataView**创建父子关系中的数据视图。

 [修改 dataview](modifying-dataviews.md)介绍如何通过**DataView**修改基础**DataTable**中的数据，包括启用或禁用更新。

 [处理 DataView 事件](handling-dataview-events.md) 介绍如何使用 **ListChanged** 事件在更新 **DataView** 的内容或顺序时接收通知。

 [管理 dataview](managing-dataviews.md)介绍如何使用**DataViewManager**来管理**数据集中**每个表的**DataView**设置。

## <a name="related-sections"></a>相关章节

 [ASP.NET Web 应用程序](/previous-versions/655cec97(v=vs.100)) 提供有关创建 ASP.NET 应用程序、Web 窗体和 Web 服务的概述和详细的分步过程。

 [Windows 应用程序](/previous-versions/ms184421(v=vs.100)) 提供有关使用 Windows 窗体和控制台应用程序的详细信息。

 [数据集、数据表和 dataview](index.md) 介绍 **DataSet** 对象以及如何使用它来管理应用程序数据。

 [数据表](datatables.md) 介绍 **DataTable** 对象，以及如何使用它来管理应用程序数据或将其作为 **数据集**的一部分。

 [ADO.NET](../index.md) 介绍 ADO.NET 的体系结构和组件，以及如何使用 ADO.NET 来访问现有的数据源和管理应用程序数据。

## <a name="see-also"></a>请参阅

- [ADO.NET 概述](../ado-net-overview.md)
