---
description: Método getDate (java.lang.String, java.util.Calendar) (SQLServerResultSet)
title: Columna del método getDate (java.util.Calendar) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerResultSet.getDate (java.lang.String, java.util.Calendar)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 3fa2a72a-7499-44ec-8f76-a8e646e0190c
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: ae7e6ecbed04e1df21258ed6cb547c0972f25584
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99163251"
---
# <a name="getdate-method-javalangstring-javautilcalendar-sqlserverresultset"></a>Método getDate (java.lang.String, java.util.Calendar) (SQLServerResultSet)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Recupera el valor del nombre de columna designado en la fila actual de este objeto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) como un objeto java.sql.Date en el lenguaje de programación Java; para ello, usa el objeto Calendar determinado.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
public java.sql.Date getDate(java.lang.String colName,  
                             java.util.Calendar cal)  
```  
  
#### <a name="parameters"></a>Parámetros  
 *colName*  
  
 Valor **String** que contiene el nombre de columna.  
  
 *cal*  
  
 Un objeto Calendar.  
  
## <a name="return-value"></a>Valor devuelto  
 Un objeto Date.  
  
## <a name="exceptions"></a>Excepciones  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Observaciones  
 El método getDate especifica este método getDate en la interfaz java.sql.ResultSet.  
  
 Este método devuelve una fecha válida que forma parte de un tipo de datos datetime o smalldatetime de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], con la parte correspondiente a la hora establecida en la hora de inicio de Java de 00:00 (media noche) en la zona horaria del calendario suministrado.  
  
## <a name="see-also"></a>Consulte también  
 [Método getDate &#40;SQLServerResultSet&#41;](../../../connect/jdbc/reference/getdate-method-sqlserverresultset.md)   
 [Miembros SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [Clase SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
