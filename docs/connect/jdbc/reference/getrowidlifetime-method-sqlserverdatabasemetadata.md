---
description: Método getRowIdLifetime (SQLServerDatabaseMetaData)
title: Método getRowIdLifetime Method (SQLServerDatabaseMetaData) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 317c0b44-fe3f-4142-9cab-e40e4c4fe070
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 4da5d30b347d30acfa3a6fd7b96214bc7955278d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99162383"
---
# <a name="getrowidlifetime-method-sqlserverdatabasemetadata"></a>Método getRowIdLifetime (SQLServerDatabaseMetaData)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Devuelve un estado que indica si se admite el tipo de datos RowId de SQL. Si así fuera, devuelve la duración de un objeto RowId.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
public java.sql.RowIdLifetime getRowIdLifetime()  
```  
  
## <a name="return-value"></a>Valor devuelto  
 Un objeto RowIdLifetime.  
  
> [!NOTE]  
>  En la versión 2,0 del controlador JDBC, este método devuelve el valor java.sql.RowIdLifetime.ROWID_UNSUPPORTED.  
  
## <a name="exceptions"></a>Excepciones  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Observaciones  
 El método getRowIdLifetime especifica este método getRowIdLifetime en la interfaz java.sql.DatabaseMetaData.  
  
## <a name="see-also"></a>Consulte también  
 [Métodos SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-methods.md)   
 [Miembros SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-members.md)   
 [Clase SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-class.md)  
  
  
