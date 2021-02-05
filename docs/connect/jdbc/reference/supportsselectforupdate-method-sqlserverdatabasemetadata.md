---
description: Método supportsSelectForUpdate (SQLServerDatabaseMetaData)
title: Método supportsSelectForUpdate (SQLServerDatabaseMetaData) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerDatabaseMetaData.supportsSelectForUpdate
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 721bc8e3-36c0-4fa6-8561-4f8d54c8265a
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 0061fd77446ab00cbf333f3d45d294e5cf3f4395
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99188398"
---
# <a name="supportsselectforupdate-method-sqlserverdatabasemetadata"></a>Método supportsSelectForUpdate (SQLServerDatabaseMetaData)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Recupera si esta base de datos admite instrucciones SELECT FOR UPDATE.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
public boolean supportsSelectForUpdate()  
```  
  
## <a name="return-value"></a>Valor devuelto  
 **true** si es compatible. De lo contrario, se devuelve el valor **False**.  
  
## <a name="exceptions"></a>Excepciones  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Observaciones  
 El método supportsSelectForUpdate especifica este método supportsSelectForUpdate en la interfaz java.sql.DatabaseMetaData.  
  
## <a name="see-also"></a>Consulte también  
 [Métodos SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-methods.md)   
 [Miembros SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-members.md)   
 [Clase SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-class.md)  
  
  
