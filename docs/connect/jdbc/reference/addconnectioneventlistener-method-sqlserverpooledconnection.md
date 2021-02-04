---
description: Método addConnectionEventListener (SQLServerPooledConnection)
title: Método addConnectionEventListener (SQLServerPooledConnection) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerPooledConnection.addConnectionEventListener
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 142830a8-8d4e-48ca-911d-85bf195ca4fe
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 2354562eb4cdfc81145aaf139e2cd5cdc776d2cd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99163572"
---
# <a name="addconnectioneventlistener-method-sqlserverpooledconnection"></a>Método addConnectionEventListener (SQLServerPooledConnection)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Registra la escucha de evento determinado de forma que se notificará si se produce un evento en este objeto [SQLServerPooledConnection](../../../connect/jdbc/reference/sqlserverpooledconnection-class.md).  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
public void addConnectionEventListener(javax.sql.ConnectionEventListener listener)  
```  
  
#### <a name="parameters"></a>Parámetros  
 *listener*  
  
 Un objeto ConnectionEventListener.  
  
## <a name="remarks"></a>Observaciones  
 El método addConnectionEventListener especifica este método addConnectionEventListener en la interfaz javax.sql.PooledConnection.  
  
## <a name="see-also"></a>Consulte también  
 [Métodos SQLServerPooledConnection](../../../connect/jdbc/reference/sqlserverpooledconnection-methods.md)   
 [Miembros SQLServerPooledConnection](../../../connect/jdbc/reference/sqlserverpooledconnection-members.md)   
 [Clase SQLServerPooledConnection](../../../connect/jdbc/reference/sqlserverpooledconnection-class.md)  
  
  
