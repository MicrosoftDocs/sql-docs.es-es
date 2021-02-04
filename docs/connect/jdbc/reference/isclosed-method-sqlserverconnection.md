---
description: Método isClosed (SQLServerConnection)
title: Método isClosed (SQLServerConnection) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerConnection.isClosed
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 3560ab18-4350-4d02-9716-439f0c2f7142
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: c8a524b7d5aa5af4cd002741786ea2fb0cea2bb2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99177453"
---
# <a name="isclosed-method-sqlserverconnection"></a>Método isClosed (SQLServerConnection)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Indica si se ha cerrado este objeto [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md).  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
public boolean isClosed()  
```  
  
## <a name="return-value"></a>Valor devuelto  
 **true** si la conexión está cerrada, **false** si no lo está.  
  
## <a name="exceptions"></a>Excepciones  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Observaciones  
 El método isClosed especifica este método isClosed en la interfaz java.sql.Connection.  
  
 Comprueba el estado del objeto SQLServerConnection llamado. Una conexión está cerrada si se ha llamado en ella al método [close](../../../connect/jdbc/reference/close-method-sqlserverconnection.md) o si se han producido ciertos errores irrecuperables. Este método devolverá el valor **true** solo cuando se ha llamado tras llamar al método close.  
  
## <a name="see-also"></a>Consulte también  
 [Miembros SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-members.md)   
 [Clase SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md)  
  
  
