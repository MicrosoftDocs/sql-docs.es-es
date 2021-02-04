---
description: Método getBoolean (java.lang.String) (SQLServerResultSet)
title: Método getBoolean (java.lang.String) (SQLServerResultSet) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerResultSet.getBoolean (java.lang.String)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: ba98a27b-722d-4904-ac65-0f082fde1fe6
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 28886a0ceea38b0c465de8bde215cbdc277ee829
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99168137"
---
# <a name="getboolean-method-javalangstring-sqlserverresultset"></a>Método getBoolean (java.lang.String) (SQLServerResultSet)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Recupera el valor del nombre de la columna designada en la fila actual de este objeto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) como un valor de tipo **boolean** en el lenguaje de programación Java.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
public boolean getBoolean(java.lang.String columnName)  
```  
  
#### <a name="parameters"></a>Parámetros  
 *columnName*  
  
 Valor **String** que contiene el nombre de columna.  
  
## <a name="return-value"></a>Valor devuelto  
 Un valor **boolean**.  
  
## <a name="exceptions"></a>Excepciones  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Observaciones  
 El método getBoolean especifica este método getBoolean en la interfaz java.sql.ResultSet.  
  
 Este método se admite únicamente en los tipos de datos numéricos y de caracteres. Convierte los valores "1", 1 y "**true**" en **true**, y los valores "0", 0 y "**false**" en **false**. Para el resto de los valores no se ha definido el comportamiento.  
  
## <a name="see-also"></a>Consulte también  
 [Método getBoolean &#40;SQLServerResultSet&#41;](../../../connect/jdbc/reference/getboolean-method-sqlserverresultset.md)   
 [Miembros SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [Clase SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
