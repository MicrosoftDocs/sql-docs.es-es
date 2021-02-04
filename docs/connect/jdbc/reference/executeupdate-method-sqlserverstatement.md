---
description: Método executeUpdate (SQLServerStatement)
title: Método executeUpdate (SQLServerStatement) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerStatement.executeUpdate
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 10ae662a-ce3c-4b24-875c-5c2df319d93b
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 8134bd0c4523da21bd46ebea7547c6ca85e978e7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99168515"
---
# <a name="executeupdate-method-sqlserverstatement"></a>Método executeUpdate (SQLServerStatement)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Ejecuta la instrucción SQL determinada, que puede ser una instrucción INSERT, UPDATE o DELETE; o una instrucción SQL que no devuelve nada, como una instrucción DDL de SQL. A partir del controlador JDBC 3.0 de [!INCLUDE[msCoName](../../../includes/msconame_md.md)][!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], executeUpdate devolverá el número correcto de filas actualizado en una operación MERGE.  
  
## <a name="overload-list"></a>Lista de sobrecargas  
  
|NOMBRE|Descripción|  
|----------|-----------------|  
|[executeUpdate (java.lang.String)](../../../connect/jdbc/reference/executeupdate-method-java-lang-string-sqlserverstatement.md)|Ejecuta la instrucción SQL determinada, que puede ser una instrucción INSERT, UPDATE, DELETE o MERGE; o una instrucción SQL que no devuelve nada, como una instrucción DDL de SQL.|  
|[executeUpdate (java.lang.String, int)](../../../connect/jdbc/reference/executeupdate-method-java-lang-string-int.md)|Ejecuta la instrucción SQL especificada y señala el [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)] con la marca indicada sobre si las claves generadas automáticamente por este objeto [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md) deben estar disponibles para su recuperación.|  
|[executeUpdate (java.lang.String, int&#91;&#93;)](../../../connect/jdbc/reference/executeupdate-method-java-lang-string.md)|Ejecuta la instrucción SQL determinada e indica al controlador JDBC que las claves que se han generado automáticamente y están presentes en la matriz determinada deben estar disponibles para su recuperación.|  
|[executeUpdate (java.lang.String, java.lang.String&#91;&#93;)](../../../connect/jdbc/reference/executeupdate-method-java-lang-string-java-lang-string.md)|Ejecuta la instrucción SQL determinada e indica al controlador JDBC que las claves que se han generado automáticamente y están presentes en la matriz determinada deben estar disponibles para su recuperación.|  
  
## <a name="see-also"></a>Consulte también  
 [Miembros SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-members.md)   
 [Clase SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)  
  
  
