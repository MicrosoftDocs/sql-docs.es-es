---
description: Método getString (java.lang.String) (SQLServerResultSet)
title: Método getString (java.lang.String) (SQLServerResultSet) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerResultSet.getString (java.lang.String)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 8a98c8a8-61d0-40c9-9335-25a87b732dc3
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 3abe4f260c8e2fffb8fa32460d9e233ea65cdfbf
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99174936"
---
# <a name="getstring-method-javalangstring-sqlserverresultset"></a>Método getString (java.lang.String) (SQLServerResultSet)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Recupera el valor del nombre de columna que se ha designado en la fila actual de este objeto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) como un valor **String** en el lenguaje de programación Java.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
public java.lang.String getString(java.lang.String columnName)  
```  
  
#### <a name="parameters"></a>Parámetros  
 *columnName*  
  
 Valor **String** que contiene el nombre de columna.  
  
## <a name="return-value"></a>Valor devuelto  
 Un valor **String**.  
  
## <a name="exceptions"></a>Excepciones  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Observaciones  
 El método getString especifica este método getString en la interfaz java.sql.ResultSet.  
  
 Todas las columnas en SQL Server se pueden devolver como una cadena. Esto indica que se puede devolver una representación de tipo **String** de todos los tipos basados en número y en caracteres y una representación de una cadena hexadecimal de columnas binarias como binary, varbinary, varbinary(max), image, timestamp y uniqueidentifier.  
  
 Los tipos importantes para la ubicación como money, smallmoney, datetime, smalldatetime, float, real, decimal y numeric devolverán el formato canónico toString() para el valor subyacente del tipo.  
  
 Los tipos definidos por el usuario se devuelven como valores de tipo **String**.  
  
## <a name="see-also"></a>Consulte también  
 [Método getString &#40;SQLServerResultSet&#41;](../../../connect/jdbc/reference/getstring-method-sqlserverresultset.md)   
 [Miembros SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [Clase SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
