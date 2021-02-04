---
description: Método getPooledConnection ()
title: Método getPooledConnection () | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerConnectionPoolDataSource.getPooledConnection ()
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: aad6c325-3398-462c-aa6e-201dc89fa5ef
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 1dd6e8c826ef3ff8659ad7f4baa3bdd72574b4ec
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99175248"
---
# <a name="getpooledconnection-method-"></a>Método getPooledConnection ()
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Intenta establecer una conexión a bases de datos físicas que se pueda utilizar como una conexión agrupada.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
public javax.sql.PooledConnection getPooledConnection()  
```  
  
## <a name="return-value"></a>Valor devuelto  
 Un objeto [SQLServerPooledConnection](../../../connect/jdbc/reference/sqlserverpooledconnection-class.md).  
  
## <a name="exceptions"></a>Excepciones  
 java.sql.SQLException  
  
## <a name="remarks"></a>Observaciones  
 El método getPooledConnection especifica este método getPooledConnection en la interfaz javax.sql.ConnectionPoolDataSource.  
  
## <a name="see-also"></a>Consulte también  
 [getPooledConnection](../../../connect/jdbc/reference/getpooledconnection-method-sqlserverconnectionpooldatasource.md)   
 [Métodos SQLServerConnectionPoolDataSource](../../../connect/jdbc/reference/sqlserverconnectionpooldatasource-methods.md)   
 [Miembros SQLServerConnectionPoolDataSource](../../../connect/jdbc/reference/sqlserverconnectionpooldatasource-members.md)   
 [Clase SQLServerConnectionPoolDataSource](../../../connect/jdbc/reference/sqlserverconnectionpooldatasource-class.md)  
  
  
