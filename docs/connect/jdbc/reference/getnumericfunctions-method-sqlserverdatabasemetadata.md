---
description: Método getNumericFunctions (SQLServerDatabaseMetaData)
title: Método getNumericFunctions (SQLServerDatabaseMetaData) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerDatabaseMetaData.getNumericFunctions
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 8d1c3848-bdb7-452a-862f-6421e1a7ce8b
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 11044d1bbd72f0beb8c07ab12c7fcc84dde54e56
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99162563"
---
# <a name="getnumericfunctions-method-sqlserverdatabasemetadata"></a>Método getNumericFunctions (SQLServerDatabaseMetaData)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Recupera una lista separada por comas de funciones matemáticas que están disponibles con esta base de datos.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
public java.lang.String getNumericFunctions()  
```  
  
## <a name="return-value"></a>Valor devuelto  
 **String** que contiene las funciones matemáticas disponibles.  
  
## <a name="exceptions"></a>Excepciones  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Observaciones  
 El método getNumericFunctions especifica este método getNumericFunctions en la interfaz java.sql.DatabaseMetaData.  
  
## <a name="see-also"></a>Consulte también  
 [Métodos SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-methods.md)   
 [Miembros SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-members.md)   
 [Clase SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-class.md)  
  
  
