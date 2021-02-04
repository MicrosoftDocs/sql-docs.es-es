---
description: Método supportsOpenCursorsAcrossCommit (SQLServerDatabaseMetaData)
title: Método supportsOpenCursorsAcrossCommit | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerDatabaseMetaData.supportsOpenCursorsAcrossCommit
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: b7eed108-64cc-4be6-b297-8af6c1e3dc72
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: cbadefdb14c2a6ec49d8529da248055f2686f7ef
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99167913"
---
# <a name="supportsopencursorsacrosscommit-method-sqlserverdatabasemetadata"></a>Método supportsOpenCursorsAcrossCommit (SQLServerDatabaseMetaData)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Recupera si esta base de datos permite mantener cursores abiertos en las confirmaciones.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
public boolean supportsOpenCursorsAcrossCommit()  
```  
  
## <a name="return-value"></a>Valor devuelto  
 **true** si es compatible. De lo contrario, se devuelve el valor **False**.  
  
## <a name="exceptions"></a>Excepciones  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Observaciones  
 El método supportsOpenCursorsAcrossCommit especifica este método supportsOpenCursorsAcrossCommit en la interfaz java.sql.DatabaseMetaData.  
  
## <a name="see-also"></a>Consulte también  
 [Métodos SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-methods.md)   
 [Miembros SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-members.md)   
 [Clase SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-class.md)  
  
  
