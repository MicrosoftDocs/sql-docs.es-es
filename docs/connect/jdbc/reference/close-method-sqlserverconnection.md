---
description: Método close (SQLServerConnection)
title: Método close (SQLServerConnection) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerConnection.close
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: f0f26585-bdf7-4737-b434-8c7e115c8e94
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 47f263e398245cd766fcf7bef52c9863f66fa43d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99163502"
---
# <a name="close-method-sqlserverconnection"></a>Método close (SQLServerConnection)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Libera inmediatamente la base de datos de este objeto [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md) y los recursos de JDBC en lugar de esperar a que se liberen automáticamente.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
public void close()  
```  
  
## <a name="exceptions"></a>Excepciones  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Observaciones  
 El método close especifica este método close en la interfaz java.sql.Connection.  
  
 Si se llama al método close en el medio de una transacción hace que la transacción se revierta.  
  
## <a name="see-also"></a>Consulte también  
 [Miembros SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-members.md)   
 [Clase SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md)  
  
  
