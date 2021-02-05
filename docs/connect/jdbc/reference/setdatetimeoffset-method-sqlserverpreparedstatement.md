---
description: Método setDateTimeOffset (SQLServerPreparedStatement)
title: Método setDateTimeOffset (SQLServerPreparedStatement) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 5014dba9-1755-4769-b070-6cbeecee864e
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 1734cb149b45175f6a6df9a4b19711d05281a225
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99179053"
---
# <a name="setdatetimeoffset-method-sqlserverpreparedstatement"></a>Método setDateTimeOffset (SQLServerPreparedStatement)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Este método se agregó en [!INCLUDE[msCoName](../../../includes/msconame_md.md)][!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] JDBC Driver 3.0.  
  
 Establece el valor de la columna especificada en el valor de la [clase DateTimeOffset](../../../connect/jdbc/reference/datetimeoffset-class.md).  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
public final void setDateTimeOffset(int n, microsoft.sql.DateTimeOffset x)  
```  
  
#### <a name="parameters"></a>Parámetros  
 *n*  
  
 El ordinal basado en cero de una columna.  
  
 *x*  
  
 El objeto [Clase DateTimeOffset](../../../connect/jdbc/reference/datetimeoffset-class.md).  
  
## <a name="exceptions"></a>Excepciones  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="see-also"></a>Consulte también  
 [Miembros de SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-members.md)   
 [Clase SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md)  
  
  
