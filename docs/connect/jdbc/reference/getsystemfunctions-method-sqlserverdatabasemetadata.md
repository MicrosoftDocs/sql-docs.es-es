---
description: Método getSystemFunctions (SQLServerDatabaseMetaData)
title: Método getSystemFunctions (SQLServerDatabaseMetaData) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerDatabaseMetaData.getSystemFunctions
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 6d390ec5-9ee2-49c4-b661-a93b55691dd1
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 0cdb3113a554e1a0a69b37f45c8959245b8b70a9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99162152"
---
# <a name="getsystemfunctions-method-sqlserverdatabasemetadata"></a>Método getSystemFunctions (SQLServerDatabaseMetaData)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Recupera una lista separada por comas de funciones del sistema que están disponibles con esta base de datos.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
public java.lang.String getSystemFunctions()  
```  
  
## <a name="return-value"></a>Valor devuelto  
 Un objeto **String** que contiene la lista de funciones del sistema.  
  
## <a name="exceptions"></a>Excepciones  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Observaciones  
 El método getSystemFunctions especifica este método getSystemFunctions en la interfaz java.sql.DatabaseMetaData.  
  
## <a name="see-also"></a>Consulte también  
 [Métodos SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-methods.md)   
 [Miembros SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-members.md)   
 [Clase SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-class.md)  
  
  
