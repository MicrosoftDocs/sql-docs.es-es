---
description: Método getStatement (SQLServerResultSet)
title: Método getStatement (SQLServerResultSet) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerResultSet.getStatement
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 7dea981b-b4fd-4f8d-954f-e686124627e2
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 720afe1e93fda2374427b0a9d01ad3ba069ba3e9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99174961"
---
# <a name="getstatement-method-sqlserverresultset"></a>Método getStatement (SQLServerResultSet)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Recupera el objeto [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md) que generó este objeto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
public java.sql.Statement getStatement()  
```  
  
## <a name="return-value"></a>Valor devuelto  
 Un objeto SQLServerStatement.  
  
## <a name="exceptions"></a>Excepciones  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Observaciones  
 El método getStatement especifica este método getStatement en la interfaz java.sql.ResultSet.  
  
 Si el conjunto de resultados se generara de alguna otra forma, por ejemplo, mediante un método [SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-class.md), este devolverá el valor NULL.  
  
## <a name="see-also"></a>Consulte también  
 [Miembros SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [Clase SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
