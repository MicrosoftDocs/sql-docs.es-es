---
description: Método supportsFullOuterJoins (SQLServerDatabaseMetaData)
title: Método supportsFullOuterJoins (SQLServerDatabaseMetaData) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerDatabaseMetaData.supportsFullOuterJoins
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 836f1f45-59ed-4a34-9809-2000d3062576
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 8eae4dd09fa4fd1534ad79b3d0fa53228f2f6e2e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99182967"
---
# <a name="supportsfullouterjoins-method-sqlserverdatabasemetadata"></a>Método supportsFullOuterJoins (SQLServerDatabaseMetaData)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Recupera si esta base de datos admite las combinaciones externas anidadas completas.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
public boolean supportsFullOuterJoins()  
```  
  
## <a name="return-value"></a>Valor devuelto  
 **true** si es compatible. De lo contrario, se devuelve el valor **False**.  
  
## <a name="exceptions"></a>Excepciones  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Observaciones  
 El método supportsFullOuterJoins especifica este método supportsFullOuterJoins en la interfaz java.sql.DatabaseMetaData.  
  
## <a name="see-also"></a>Consulte también  
 [Métodos SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-methods.md)   
 [Miembros SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-members.md)   
 [Clase SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-class.md)  
  
  
