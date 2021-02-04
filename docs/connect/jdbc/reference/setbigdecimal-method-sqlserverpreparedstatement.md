---
description: Método setBigDecimal (SQLServerPreparedStatement)
title: Método setBigDecimal (SQLServerPreparedStatement) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerPreparedStatement.setBigDecimal
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 860f86db-d840-401a-a5c2-cd22e8cc1e4e
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: c5d0c0d758e7b097b27b84f6d032c2335ff27cd6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99173929"
---
# <a name="setbigdecimal-method-sqlserverpreparedstatement"></a>Método setBigDecimal (SQLServerPreparedStatement)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Establece el número de parámetro designado para el objeto BigDecimal determinado.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
public final void setBigDecimal(int n,  
                                java.math.BigDecimal x)  
```  
  
#### <a name="parameters"></a>Parámetros  
 *n*  
  
 Valor **int** que indica el número de parámetro.  
  
 *x*  
  
 Un objeto BigDecimal.  
  
## <a name="exceptions"></a>Excepciones  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Observaciones  
 El método setBigDecimal especifica este método setBigDecimal en la interfaz java.sql.PreparedStatement.  
  
## <a name="see-also"></a>Consulte también  
 [Miembros de SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-members.md)   
 [Clase SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md)  
  
  
