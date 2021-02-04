---
description: Método findColumn (SQLServerResultSet)
title: Método findColumn (SQLServerResultSet) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerResultSet.findColumn
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 7c29994a-0b53-420b-8a9b-82a9eef08587
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 13f2f1f8c12ec42a4d310080c8c765f276a481d7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99163369"
---
# <a name="findcolumn-method-sqlserverresultset"></a>Método findColumn (SQLServerResultSet)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Recupera el índice de la primera columna coincidente con el nombre de la columna especificado en este objeto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
public int findColumn(java.lang.String columnName)  
```  
  
#### <a name="parameters"></a>Parámetros  
 *columnName*  
  
 Un objeto **String** que contiene el nombre del rol.  
  
## <a name="return-value"></a>Valor devuelto  
 Valor **int** que indica el índice de la columna.  
  
## <a name="exceptions"></a>Excepciones  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Observaciones  
 El método findColumn especifica este método findColumn en la interfaz java.sql.ResultSet.  
  
 Si hay varias columnas con el mismo nombre, el método findColumn devuelve la primera coincidencia con distinción entre mayúsculas y minúsculas. Si no hay coincidencias con distinción entre mayúsculas y minúsculas, este método devuelve la primera coincidencia con distinción entre mayúsculas y minúsculas.  
  
## <a name="see-also"></a>Consulte también  
 [Miembros SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [Clase SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
