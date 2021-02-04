---
description: Método getTimeDateFunctions (SQLServerDatabaseMetaData)
title: Método getTimeDateFunctions (SQLServerDatabaseMetaData) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerDatabaseMetaData.getTimeDateFunctions
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: a56e08ae-6f4e-4dc6-b175-ff457d0d7a81
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 6771faae716ead956051ebfde3431a38e4078266
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99162108"
---
# <a name="gettimedatefunctions-method-sqlserverdatabasemetadata"></a>Método getTimeDateFunctions (SQLServerDatabaseMetaData)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Recupera una lista separada por comas de las funciones de fecha y hora que están disponibles con esta base de datos.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
public java.lang.String getTimeDateFunctions()  
```  
  
## <a name="return-value"></a>Valor devuelto  
 Un valor **String** que contiene una lista de las funciones de fecha y tiempo.  
  
## <a name="exceptions"></a>Excepciones  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Observaciones  
 El método getTimeDateFunctions especifica este método getTimeDateFunctions en la interfaz java.sql.DatabaseMetaData.  
  
## <a name="see-also"></a>Consulte también  
 [Métodos SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-methods.md)   
 [Miembros SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-members.md)   
 [Clase SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-class.md)  
  
  
