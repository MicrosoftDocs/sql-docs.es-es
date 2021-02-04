---
description: Método getSQLStateType (SQLServerDatabaseMetaData)
title: Método getSQLStateType (SQLServerDatabaseMetaData) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerDatabaseMetaData.getSQLStateType
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: ee4d6751-68a3-4d04-831c-e6d704c59e63
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 33b0553b18dc09306e7d84effa30b81b20b84768
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99162277"
---
# <a name="getsqlstatetype-method-sqlserverdatabasemetadata"></a>Método getSQLStateType (SQLServerDatabaseMetaData)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Indica si SQLSTATE, que devolvió el método SQLException.getSQLState, es X/Open (ahora se denomina Open Group), SQL CLI, SQL99 (JDBC 3.0) o SQL:2003 (JDBC 4.0).  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
public int getSQLStateType()  
```  
  
## <a name="return-value"></a>Valor devuelto  
 Un valor **int** que indica el tipo de SQLSTATE, que puede ser uno de los siguientes valores:  
  
-   Para Java Runtime Environment versión 5.0: Si la propiedad de conexión **xopenStates** está establecida en **true**, este método devuelve DatabaseMetaData.sqlStateXOpen. En caso contrario, DatabaseMetaData.sqlStateSQL99.  
  
-   Para Java Runtime Environment versión 6.0: Si la propiedad de conexión **xopenStates** está establecida en **true**, este método devuelve DatabaseMetaData.sqlStateXOpen. En caso contrario, DatabaseMetaData.sqlStateSQL.  
  
## <a name="exceptions"></a>Excepciones  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Observaciones  
 El método getSQLStateType especifica este método getSQLStateType en la interfaz java.sql.DatabaseMetaData.  
  
## <a name="see-also"></a>Consulte también  
 [Métodos SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-methods.md)   
 [Miembros SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-members.md)   
 [Clase SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-class.md)  
  
  
