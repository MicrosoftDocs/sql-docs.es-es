---
description: Método getDateTimeOffset (int)
title: Método getDateTimeOffset (int) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 8bb00356-4d6e-4625-b924-67646930fdf2
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 4e1c1132c0fd7d788636cedb8cdf5eb9f2ea55f0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99163202"
---
# <a name="getdatetimeoffset-method-int"></a>Método getDateTimeOffset (int)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Este método se agregó en [!INCLUDE[msCoName](../../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] JDBC Driver 3.0.  
  
 Recupera el valor del parámetro designado como un objeto [DateTimeOffset Class](../../../connect/jdbc/reference/datetimeoffset-class.md) en el lenguaje de programación Java según el índice de parámetro.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
public microsoft.sql.DateTimeOffset getDateTimeOffset(int index)  
```  
  
#### <a name="parameters"></a>Parámetros  
 *índice*  
  
 El ordinal del parámetro que se basa en uno.  
  
## <a name="return-value"></a>Valor devuelto  
 Un objeto [Clase DateTimeOffset](../../../connect/jdbc/reference/datetimeoffset-class.md).  
  
## <a name="exceptions"></a>Excepciones  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Observaciones  
 Puede establecer un valor de parámetro [Clase DateTimeOffset](../../../connect/jdbc/reference/datetimeoffset-class.md) con [SQLServerCallableStatement.setDateTimeOffset](../../../connect/jdbc/reference/setdatetimeoffset-method-sqlservercallablestatement.md).  
  
## <a name="see-also"></a>Consulte también  
 [Método getDateTimeOffset &#40;SQLServerCallableStatement&#41;](../../../connect/jdbc/reference/getdatetimeoffset-method-sqlservercallablestatement.md)   
 [Miembros SQLServerCallableStatement](../../../connect/jdbc/reference/sqlservercallablestatement-members.md)   
 [Clase SQLServerCallableStatement](../../../connect/jdbc/reference/sqlservercallablestatement-class.md)  
  
  
