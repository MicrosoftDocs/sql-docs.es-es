---
description: Método supportsOpenStatementsAcrossRollback (SQLServerDatabaseMetaData)
title: Método supportsOpenStatementsAcrossRollback | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerDatabaseMetaData.supportsOpenStatementsAcrossRollback
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 4e38b938-f39f-4c5d-9b32-4ba489535c45
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 6384484a621f0317897b5808de1e454f5688e6b3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186115"
---
# <a name="supportsopenstatementsacrossrollback-method-sqlserverdatabasemetadata"></a>Método supportsOpenStatementsAcrossRollback (SQLServerDatabaseMetaData)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Recupera si esta base de datos permite mantener instrucciones abiertas en las reversiones.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
public boolean supportsOpenStatementsAcrossRollback()  
```  
  
## <a name="return-value"></a>Valor devuelto  
 **true** si es compatible. De lo contrario, se devuelve el valor **False**.  
  
## <a name="exceptions"></a>Excepciones  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Observaciones  
 El método supportsOpenStatementsAcrossRollback especifica este método supportsOpenStatementsAcrossRollback en la interfaz java.sql.DatabaseMetaData.  
  
## <a name="see-also"></a>Consulte también  
 [Métodos SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-methods.md)   
 [Miembros SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-members.md)   
 [Clase SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-class.md)  
  
  
