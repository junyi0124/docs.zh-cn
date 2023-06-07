---
title: 浮点数
ms.date: 03/30/2017
ms.assetid: 73c218c6-1c44-4402-a167-4f6262629a91
ms.openlocfilehash: 754338ce842fdfccb8c3874e63a75a3aa2fe3eb3
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91169552"
---
# <a name="floating-point-numbers"></a>浮点数

本主题介绍开发人员在使用 ADO.NET 中的浮点数时经常遇到的一些问题。 这些问题是由计算机存储浮点数的方式所导致的，并不是特定提供程序（如 <xref:System.Data.SqlClient> 或 <xref:System.Data.OracleClient>）所特有的。  
  
 浮点数一般没有准确的二进制表示形式， 计算机只是存储数字的近似值。 不同情况下，可能会使用不同的二进制位数来表示同一数字。 当浮点数从一种表示形式转换为另一种表示形式时，该数字的最低有效位数可能稍有变化。 当数字从一种类型强制转换为另一种类型时，通常会发生转换。 无论转换是发生在数据库中表示数据库值的类型之间，还是发生在类型之间，都会产生差异。 由于这些变化，逻辑上相等的数字其最低有效位数可能会发生变化，从而导致它们具有不同的值。 数字中的精度位数可能比预期的更大或更小。 将数字的格式设置为字符串时，可能不会显示预期值。  
  
 若要最大限度地减少这些影响，应在 numeric 类型之间使用您可用的最近似匹配。 例如，如果使用的是 SQL Server，则在将 real 类型的 Transact-sql 值转换为 float 类型的值时，确切的数值可能会发生变化。 在 .NET Framework 中，将转换 <xref:System.Single> 为 <xref:System.Double> 可能还会产生意外结果。 在这两种情况下，比较好的方法是使应用程序中的所有值都使用同一 numeric 类型。 您也可以使用固定精度的十进制类型，或者在使用浮点数之前将它们强制转换为固定精度的十进制类型。  
  
 若要使用相等比较来解决这些问题，请考虑对您的应用程序进行编码，以便忽略最低有效位数的变化。 例如，用一个数减去另一个数，而不是通过比较，来了解两个数字是否相等。 如果差异在可接受舍入范围内，您的应用程序可以将这些数视为同一个数。  
  
## <a name="see-also"></a>请参阅

- [为何浮点数可能丢失精度](/cpp/build/why-floating-point-numbers-may-lose-precision)
- [ADO.NET 概述](ado-net-overview.md)
