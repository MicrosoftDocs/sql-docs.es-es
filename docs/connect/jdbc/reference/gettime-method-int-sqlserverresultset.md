---
description: Método getTime (int) (SQLServerResultSet)
title: Método getTime (int) (SQLServerResultSet) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerResultSet.getTime (int)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: e18c84f5-7171-4057-8c9e-fe1d43ae9c20
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 11eb98c03c27387f5a8b9be444cc6ca937276fb9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99174853"
---
# <a name="gettime-method-int-sqlserverresultset"></a>Método getTime (int) (SQLServerResultSet)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Recupera el valor del índice de la columna designada en la fila actual de este objeto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) como un objeto java.sql.Time en el lenguaje de programación Java.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
public java.sql.Time getTime(int columnIndex)  
```  
  
#### <a name="parameters"></a>Parámetros  
 *columnIndex*  
  
 Valor **int** que indica el índice de la columna.  
  
## <a name="return-value"></a>Valor devuelto  
 Un objeto Time.  
  
## <a name="exceptions"></a>Excepciones  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Observaciones  
 El método getTime especifica este método getTime en la interfaz java.sql.ResultSet.  
  
 Este método devuelve una hora válida que forma parte de un tipo de datos datetime o smalldatetime de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], con la parte correspondiente a la fecha establecida en la fecha de inicio de Java de 1970/01/01.  
  
## <a name="see-also"></a>Consulte también  
 [Método getTime &#40;SQLServerResultSet&#41;](../../../connect/jdbc/reference/gettime-method-sqlserverresultset.md)   
 [Miembros SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [Clase SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
