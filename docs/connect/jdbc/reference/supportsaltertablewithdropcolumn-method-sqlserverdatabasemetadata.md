---
description: Método supportsAlterTableWithDropColumn (SQLServerDatabaseMetaData)
title: Método supportsAlterTableWithDropColumn | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerDatabaseMetaData.supportsAlterTableWithDropColumn
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 55a182c1-28e5-4d32-aeb1-166a8ac76758
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d171e5129666967fcb96e292bf66116eca50e855
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99165447"
---
# <a name="supportsaltertablewithdropcolumn-method-sqlserverdatabasemetadata"></a>Método supportsAlterTableWithDropColumn (SQLServerDatabaseMetaData)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Recupera si esta base de datos admite ALTER TABLE con la eliminación de columnas.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
public boolean supportsAlterTableWithDropColumn()  
```  
  
## <a name="return-value"></a>Valor devuelto  
 **true** si es compatible. De lo contrario, se devuelve el valor **False**.  
  
## <a name="exceptions"></a>Excepciones  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Observaciones  
 El método supportsAlterTableWithDropColumn especifica este método supportsAlterTableWithDropColumn en la interfaz java.sql.DatabaseMetaData.  
  
## <a name="see-also"></a>Consulte también  
 [Métodos SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-methods.md)   
 [Miembros SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-members.md)   
 [Clase SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-class.md)  
  
  
