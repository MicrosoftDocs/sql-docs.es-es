---
description: Método supportsGroupByUnrelated (SQLServerDatabaseMetaData)
title: Método supportsGroupByUnrelated (SQLServerDatabaseMetaData) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerDatabaseMetaData.supportsGroupByUnrelated
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 455fe02e-3877-409b-8281-8e0491acd3e8
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: ab37191747a8f0df1580a1a68ed2196793cf7882
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185578"
---
# <a name="supportsgroupbyunrelated-method-sqlserverdatabasemetadata"></a>Método supportsGroupByUnrelated (SQLServerDatabaseMetaData)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Recupera si esta base de datos admite la utilización de una columna que no esté en la instrucción SELECT en una cláusula GROUP BY.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
public boolean supportsGroupByUnrelated()  
```  
  
## <a name="return-value"></a>Valor devuelto  
 **true** si es compatible. De lo contrario, se devuelve el valor **False**.  
  
## <a name="exceptions"></a>Excepciones  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Observaciones  
 El método supportsGroupByUnrelated especifica este método supportsGroupByUnrelated en la interfaz java.sql.DatabaseMetaData.  
  
## <a name="see-also"></a>Consulte también  
 [Métodos SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-methods.md)   
 [Miembros SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-members.md)   
 [Clase SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-class.md)  
  
  
